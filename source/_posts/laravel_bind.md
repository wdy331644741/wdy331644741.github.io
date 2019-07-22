---
title: Laravel中使用Ioc容器的singleton方法和bind方法创建实例的区别
date: 2018-04-28 23:00:34
categories:
- php
tags:
- laravel
- IoC
#top: true
---


> void bind(string|array $abstract, Closure|string|null $concrete = null, bool $shared = false)
void singleton(string|array $abstract, Closure|string|null $concrete = null)  等同于 bind($abstract,$concrete ,true)
void instance(string $abstract, mixed $instance)  绑定实例化对象


Laravel 服务容器在使用时一般分为两个阶段：使用之前进行绑定（bind）完成将实现类绑定到接口；使用时对通过接口解析（make）出服务。
Laravel 内置多种不同的绑定方法以用于不同的使用场景。但无论哪种绑定方式，它们的最终目标是一致的：绑定接口到实现。
这样的好处是在项目的编码阶段建立起接口和实现的映射关系，到使用阶段通过抽象类（接口）解析出它的具体实现，这样就实现了项目中的解耦。



在 bind 方法中，主要完成以下几个方面的处理：
* 干掉之前解析过的服务实例；
* 将绑定的实现类封装成闭包，以确保后续处理的统一；
* 针对已解析过的服务实例，再次触发重新绑定回调函数，同时将最新的实现类更新到接口里面。



它们两个都是返回一个类的实例，不同的是singleton是单例模式，而bind是每次返回一个新的实例，看下面的两个例子。

1. Ioc容器bind方法
```
<?php
require __DIR__.'/../bootstrap/autoload.php';


class tanteng

{

    public $name='init_str';

}



$container = new Illuminate\Container\Container();

$container->bind('tanteng');

$instance = $container->make('tanteng');

$instance->name = 'tanteng';

$instance2 = $container->make('tanteng');

//$instance2->name = 'tuntun';


echo $instance->name.' '.$instance2->name;
```
> 输出的结果：
tanteng init_str

结论：
通过bind方法创建实例不是单例模式，而是创建新的实例。

2. Ioc容器singleton方法
```
<?php
require __DIR__.'/../bootstrap/autoload.php';


class single

{

    public $value;

}


$container = new Illuminate\Container\Container();


$container->singleton('single');

$instance3 = $container->make('single');

$instance4 = $container->make('single');



$instance3->value = 'aaaa';

$instance4->value = 'bbbb';



echo $instance3->value.' '.$instance4->value;
```
> 输出结果：
bbbb bbbb

结论：
使用singleton创建实例使用的是单例模式，每次返回同一个实例。
以上代码可以放在public下，如test.php运行。

再看框架底层代码：
```
public function singleton($abstract, $concrete = null)
{
    $this->bind($abstract, $concrete, true);
}
```
发现singleton方法其实也是调用bind方法，只是最后一个参数是true，表示单例模式。框架源代码：Illuminate/Container/Container.php

附上bind()方法
```
public function bind($abstract, $concrete = null, $shared = false)
    {
        $abstract = $this->normalize($abstract);

        $concrete = $this->normalize($concrete);

        // If the given types are actually an array, we will assume an alias is being
        // defined and will grab this "real" abstract class name and register this
        // alias with the container so that it can be used as a shortcut for it.
        if (is_array($abstract)) {
            list($abstract, $alias) = $this->extractAlias($abstract);

            $this->alias($abstract, $alias);
        }

        // If no concrete type was given, we will simply set the concrete type to the
        // abstract type. After that, the concrete type to be registered as shared
        // without being forced to state their classes in both of the parameters.
        $this->dropStaleInstances($abstract);

        if (is_null($concrete)) {
            $concrete = $abstract;
        }

        // If the factory is not a Closure, it means it is just a class name which is
        // bound into this container to the abstract type and we will just wrap it
        // up inside its own Closure to give us more convenience when extending.
        if (! $concrete instanceof Closure) {
            $concrete = $this->getClosure($abstract, $concrete);
        }

        $this->bindings[$abstract] = compact('concrete', 'shared');

        // If the abstract type was already resolved in this container we'll fire the
        // rebound listener so that any objects which have already gotten resolved
        // can have their copy of the object updated via the listener callbacks.
        if ($this->resolved($abstract)) {
            $this->rebound($abstract);
        }
    }
```
