---
title: 并发锁
date: 2019-05-17 13:30:32
categories:
- php
tags:
- redis
#top: true
---



####问题描述
在工作项目中，会遇到一些php并发访问去修改一个数据问题，如果这个数据不加锁，就会造成数据的错误。

## 1. 举例说明
 
> 如果有两个操作人(p和m)，都用用户编号100账户,分别在pc和手机端同时登陆，100账户总余额有1000，p操作人花200，m操作人花300。并发过程如下。
p操作人：
     1 取出用户的余额1000。
     2 支付后剩余 800 = 1000 - 200。
     3 更新后账户余额800。
m操作人：
       1 取出用户余额1000。
       2 支付后剩余700 = 1000 - 300。
       3 支付后账户余额700。
两次支付后，账户的余额居然还有700，应该的情况是花费了500，账户余额500才对。造成这个现象的根本原因，是并发的时候，p和m同时操作取到的余额数据都是1000。

## 2.加锁设计

锁的操作一般只有两步，一 获取锁(getLock)；二是释放锁(releaseLock)。但现实锁的方式有很多种，可以是文件方式实现；sql实现；redis实现；

## 3.code

```
<?php
namespace Lib;
class LockSys
{

	//const LOCK_TYPE_DB = 'SQLLock';  
    const LOCK_TYPE_FILE = '\Lib\FileLock';
    const LOCK_TYPE_REDIS = '\Lib\RedisLock';

	private $_lock = null;  
    private static $_supportLocks = array('FileLock', 'SQLLock', 'RedisLock'); 



	public function __construct($type, $options = array())   
    {  
        if(false == empty($type))
        {
            $this->createLock($type, $options);
        }
    }

    public function createLock($type, $options=array())
    {
        $this->_lock = new $type();//实例化 锁
        // $this->_lock = new RedisLock();//实例化 锁
    }

    public function getLock($key, $timeout = ILock::EXPIRE)
    {  
        if (false == $this->_lock instanceof ILock)
        {
            throw new Exception('false == $this->_lock instanceof ILock');
        }
        $this->_lock->getLock($key, $timeout);
    }  

    public function releaseLock($key)  
    {  
        if (false == $this->_lock instanceof ILock)
        {
            throw new Exception('false == $this->_lock instanceof ILock');
        }
        $this->_lock->releaseLock($key);
    }
}


interface ILock
{  
    const EXPIRE = 5;
    public function getLock($key, $timeout=self::EXPIRE);
    public function releaseLock($key);
}

//redis 锁
class RedisLock implements ILock
{  
    public function __construct()  
    {
        $this->redis = getReidsInstance();
        // $this->memcache = new Memcache();  
    }  
  
    public function getLock($key, $timeout=self::EXPIRE)
    {       
        $waitime = 20000;  
        $totalWaitime = 0;  
        $time = $timeout*1000000;
        // while ($totalWaitime < $time && false == $this->memcache->add($key, 1, $timeout))
        while ($totalWaitime < $time && false == $this->redis->set($key, 1, array('nx','ex'=> $timeout) ))   
        {  
            usleep($waitime);  
            $totalWaitime += $waitime;  
        }  
        if ($totalWaitime >= $time)  
            throw new Exception('can not get lock for waiting '.$timeout.'s.');  
  
    }  
  
    public function releaseLock($key)  
    {  
        $this->redis->delete($key);  
    }  
}

//文件锁
class FileLock implements ILock
{  
    private $_fp;  
    private $_single;  
  
    public function __construct($options)  
    {  
        if (isset($options['path']) && is_dir($options['path']))  
        {  
            $this->_lockPath = $options['path'].'/';  
        }  
        else  
        {  
            $this->_lockPath = '/tmp/';  
        }  
         
        $this->_single = isset($options['single'])?$options['single']:false;  
    }  
  
    public function getLock($key, $timeout=self::EXPIRE)  
    {  
        $startTime = Timer::getTimeStamp();  
  
        $file = md5(__FILE__.$key);  
        $this->fp = fopen($this->_lockPath.$file.'.lock', "w+");  
        if (true || $this->_single)  
        {  
            $op = LOCK_EX + LOCK_NB;  
        }  
        else  
        {  
            $op = LOCK_EX;  
        }  
        if (false == flock($this->fp, $op, $a))  
        {  
            throw new Exception('failed');  
        }  
         
        return true;  
    }  
  
    public function releaseLock($key)  
    {  
        flock($this->fp, LOCK_UN);  
        fclose($this->fp);  
    }  
}

?>
```

## 4.应用锁

```
    //单笔充值大于1w  或者 累计本金大于2w
    //单笔充值 小于1w 发放一张0.5阶梯加息劵 7天
    //发放1%加息 结束时间=阶梯加息活动结束时间

    //开启redis锁
    $lockSystem = new \Lib\LockSys(LockSys::LOCK_TYPE_REDIS);
    $lockKey = "ladder.".$userId;
    $lockSystem->getLock($lockKey,3);//redis单机锁  延迟8秒
    
    #code......
    $lockSystem->releaseLock($lockKey);
```

## 5.锁分析
当多个程序进程过来时（微秒级），对同一资源读取时，立马加上一个锁，然后紧接着的进程会获取到该锁（每0.02s 轮训一次）
