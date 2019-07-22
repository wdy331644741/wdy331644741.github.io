---
title: 设计模式-装饰着模式
date: 2018-04-27 23:00:34
categories:
- php
tags:
#top: true
---



**装饰者模式，动态地将责任附加到对象上。若要扩展功能，装饰者提供了比继承更加有弹性的替代方案。**
### 组合和继承的区别
- 继承

> 继承是给一个类添加行为的比较有效的途径。通过使用继承，可以使得子类在拥有自身方法的同时，还可以拥有父类的方法。但是使用继承是静态的，在编译的时候就已经决定了子类的行为，我们不便于控制增加行为的方式和时机。

- 组合

> 组合即将一个对象嵌入到另一个对象中，由另一个对象来决定是否引用该对象来扩展自己的行为。这是一种动态的方式，我们可以在应用程序中动态的控制。
与继承相比，组合关系的优势就在于不会破坏类的封装性，且具有较好的松耦合性，可以使系统更加容易维护。但是它的缺点就在于要创建比继承更多的对象。

### 装饰者模式的优缺点
- 优点
> 

1. 装饰者模式可以提供比继承更多的灵活性
2. 可以通过一种动态的方式来扩展一个对象的功能，在运行时选择不同的装饰器，从而实现不同的行为
3. 通过使用不同的具体装饰类以及这些装饰类的排列组合，可以创造出很多不同行为的组合。可以使用多个具体装饰类来装饰同一对象，得到功能更为强大的对象
4. 具体构件类与具体装饰类可以独立变化，用户可以根据需要增加新的具体构件类和具体装饰类，在使用时再对其进行组合，原有代码无须改变，符合“开闭原则”

- 缺点

> 
1. 会产生很多的小对象，增加了系统的复杂性
2. 这种比继承更加灵活机动的特性，也同时意味着装饰模式比继承更加易于出错，排错也很困难，对于多次装饰的对象，调试时寻找错误可能需要逐级排查，较为烦琐

### 装饰者的使用场景
1. 在不影响其他对象的情况下，以动态、透明的方式给单个对象添加职责
2. 需要动态地给一个对象增加功能，这些功能也可以动态地被撤销。  当不能采用继承的方式对系统进行扩充或者采用继承不利于系统扩展和维护时

![image](https://user-images.githubusercontent.com/9856659/51297117-07197b00-1a5a-11e9-940d-75765e148704.png)


### Ex:
_装饰者基类_
```
package com.xinye.test.decoration;
  /**
   * 食物基类
   * @author xinye
   *
   */
  public abstract class Food {
      protected String desc;
     public abstract String getDesc();
  }
```
_鸡肉_
```
package com.xinye.test.decoration;
  /**
   * 鸡肉
   * @author xinye
   */
  public class Chicken extends Food {
      public Chicken(){
          desc = "鸡肉";
     }
     @Override
     public String getDesc() {
         return desc;
     }
 }
```

_鸭肉_
```
package com.xinye.test.decoration;
  /**
   * 鸭肉
   * @author xinye
   *
   */
  public class Duck extends Food {
      public Duck(){
          desc = "鸭肉";
     }
     @Override
     public String getDesc() {
         return desc;
     }
 }
```
_装饰者基类_

```
package com.xinye.test.decoration;
  /**
   * @author xinye
   */
  public abstract class FoodDecoration extends Food {
      @Override
     public abstract String getDesc();   
 }
```

_蒸-装饰者_

```
package com.xinye.test.decoration;
 /**
  * 蒸食物
  * @author xinye
  */
 public class SteamedFood extends FoodDecoration {
     private Food food;
     public SteamedFood(Food f){
         this.food = f;
     }
     
     @Override
     public String getDesc() {
         return getDecoration() + food.getDesc();
     }
    
     private String getDecoration(){
         return "蒸";
     }
 }
```


_烤-装饰者_
```
 package com.xinye.test.decoration;
 /**
  * 烤食物
  */
 public class RoastFood extends FoodDecoration {
     private Food food;
     
     public RoastFood(Food f){
         this.food = f;
     }
     
     @Override
     public String getDesc() {
         return getDecoration() + food.getDesc();
    }
     
     private String getDecoration(){
         return "烤";
     }
 }
```

_客户端_
```
 package com.xinye.test.decoration;
 /**
  * 客户端
  * @author xinye
  *
  */
 public class Client {
     public static void main(String[] args) {
         // 测试单纯的食物
         Food f1 = new Chicken();
         System.out.println(f1.getDesc());
         
         System.out.println("----------------------");
         // 测试单重修饰的食物
         RoastFood rf = new RoastFood(f1);
         System.out.println(rf.getDesc());
         
         System.out.println("----------------------");
         // 测试多重修饰的食物
         SteamedFood sf = new SteamedFood(rf);
         System.out.println(sf.getDesc());
     }
 }
```


> 执行结果： 
鸡肉 ---------------------- 
烤鸡肉 ---------------------- 
蒸烤鸡肉
