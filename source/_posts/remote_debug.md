---
title: 远程调试Debug配置
date: 2018-04-27 23:00:34
categories:      
- php
tags:
- debug
top: false
---

#### 1.服务器上安装xdebug，参考https://xdebug.org/wizard.php

![image](https://user-images.githubusercontent.com/9856659/60659778-d05ce300-9e88-11e9-8216-e37a01047b82.png)


#### 2.添加server上面php.ini配置

![image](https://user-images.githubusercontent.com/9856659/60659867-0c904380-9e89-11e9-9d9c-6754647e40d9.png)

> 其中xdebug.remote_host 是本地IDE的ip
xdebug.idekey 要和后面IDE中的配置一致
xdebug.remote_handler=dbgp 代理模式

#### 3.phpstorm配置
![image](https://user-images.githubusercontent.com/9856659/60659924-26ca2180-9e89-11e9-9c65-3073eddcb97c.png)

> 代理配置：IDE key  和服务器上面php.ini一致

![image](https://user-images.githubusercontent.com/9856659/60659943-30538980-9e89-11e9-8c56-e2733eb66ead.png)


#### 4.debug配置
![image](https://user-images.githubusercontent.com/9856659/60659959-3ba6b500-9e89-11e9-8de1-310eec41f551.png)



