> 高级模块不应该依赖于低级模块，两者都应该依赖于抽象对象。 抽象不应该依赖于细节，细节应该取决于抽象。
---Robert C. Martin

> 「ioc 依赖注入」的结果就是「DI 控制反转」的目的，也就说 控制反转 的最终目标是为了 实现项目的高内聚低耦合，而 实现这种目标 的方式则是通过 依赖注入。 

_说白了  DI/ioc 其实是抽象工厂模式的一种升华(超级工厂)。_


## 一、依赖的产生

直接上代码：

```
interface interfaceB{}
class B implements interfaceB
{
	public function someFun(){
		echo "DI/IoC";
	}
}

class A
{
	protected $a; //放入B的实例
	public function __construct(interfaceB $objectB){ //类型提示类(面向接口)
		$this->a = $objectB;
	}
	public function aFun(){
		$this->a->someFun();//调用B类的方法
	}
}

$bb = new B();
$a = new A($bb);
$a->aFun();
```

## 二、IoC容器

上面的例子我们是通过手动的方式注入依赖，而IoC可以自动实现依赖注入。下面通过laravel框架中的设计方法进行简化。
```
class Container
{
	protected $bindings = [];

	//2.1绑定*************************
	public function bind($abstract, $concrete=null, $shared = false){

		//如果不是回调函数，则产生一个回调函数
		if(! $concrete instanceof Closure){
			$concrete = $this->getClosure($abstract,$concrete);
		}
		$this->bindings[$abstract] = compact('concrete','share');
	}

	//默认生成的回调函数
	protected function getClosure($abstract, $concrete){
		return function($c) use($abstract, $concrete){
			$method = ($abstract == $concrete)?'build':'make';
			return $c->$method($concrete);
		};
	}
	//*************************************
	//2.2解析-解决依赖
	public function make($abstract){
		$concrete = isset($this->bindings[$abstract])? $this->bindings[$abstract]['concrete'] : $abstract;

		if($this->isBuildable($concrete,$abstract)){
			$object = $this->build($concrete);
		}else{
			$object = $this->make($concrete);
		}
		return $object;
	}

	protected function isBuildable($concrete, $abstract){
		return $concrete === $abstract || $concrete instanceof Closure;
	}
	//2.3实例*****************************************
	public function build($concrete){
		if ($concrete instanceof Closure) {
			return $concrete($this);
		}

		$reflector = new ReflectionClass($concrete);

		if (! $reflector->isInstantiable()) {
			echo $message = "Target [$concrete] is not instantiable.";
		}

		$constructor = $reflector->getConstructor();//获取类的构造函数
		if (is_null($constructor)) {
			return new $concrete;
		}

		$dependencies = $constructor->getParameters();//构造函数中的参数
		$instances = $this->getDependencies($dependencies);

		return $reflector->newInstanceArgs($instances);
	}

	//解决构造函数参数中存在依赖，则继续解析依赖
	protected function getDependencies($parameters)
	{
		$dependencies = [];

		foreach ($parameters as $parameter) {
			$dependency = $parameter->getClass();

			if(is_null($dependency)) {
				$dependencies[] = null;
			}else{
				$dependencies[] = $this->resolveClass($parameter);
			}
		}

		return $dependencies;
	}
	protected function resolveClass(ReflectionParameter $parameter)
	{
		return $this->make($parameter->getClass()->name);//获得类型提示类
	}
	//************************************************************
}
```
-------

```
$app = new Container();
$app->bind('interfaceB','B');
$app->bind('aTestObj','A');

$aTestObj = $app->make('aTestObj');
$aTestObj->aFun();
```

## 三、超级工厂

> 工厂模式以及抽象工厂模式大家应该都有了解，为什么说ioc是超级工厂呢。假如上面的例子拓展一下。实例A这时不要依赖B类了，要用C类。


_ 实际的场景如（用烂了的例子），工厂生产汽车。之前用的是德国牌子的TUV轮毂，现在要用日本牌子的轮毂。当然同样都是轮子，因为材料、重量 甚至尺寸以及功能有出入。如果在原用的基础上改甚是麻烦，这时只需重新再写一个类C，而不动A类 _


```
class C implements interfaceB
{
    protected $c = '日本';
	public function someFun(){
		echo $this->c;
	}
}

//修改绑定关系
$app = new Container();
$app->bind('interfaceB','C');//达到DI控制反转目的
$app->bind('aTestObj','A');

$aTestObj = $app->make('aTestObj');
$aTestObj->aFun();
```

## 四、容器

对服务容器进行一个理解：

> 容器就是一个装东西的，好比碗。而服务就是这个碗要装的饭呀，菜呀，等等东西。当我们需要饭时，我们就能从这个碗里拿到。如果你想在饭里加点菜（也就是饭依赖注入了菜），我们从碗里直接拿饭就可以了，而这些依赖都由容器解决了（这也就是控制反转）。

我们需要做的就是对提供的服务进行维护。


