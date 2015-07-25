# DesignPattern-principle
关于设计模式原则，有的按照solid原则总结，有的说六原则，大家都总结的都不一致。今天我这里把我知道给大家总结一下：
>* 单一职责原则
>* 开闭原则
>* 接口分离原则
>* 里氏代换原则
>* 依赖倒置原则
>* 迪米特原则 
>* 优先使用组合，而不是使用继承

###单一职责原则
定义：不要存在多于一个导致类变更的原因。通俗的说，即单一职责是说一个类或者一个方法，只做一件事，或者完成一个任务。

问题由来：类T负责两个不同的职责：职责Z1，职责Z2。当由于职责Z1需求发生改变而需要修改类T时，有可能会导致原本运行正常的职责Z2功能发生故障。

解决：遵循单一职责原则。分别建立两个类T1、T2，使T1完成职责Z1功能，T2完成职责Z2功能。这样，当修改类T1时，不会使职责Z2发生故障风险；同理，当修改T2时，也不会使职责Z1发生故障风险。
举例说明：
用一个类描述动物行走的
```java 
public class Animal {
		public void run(String str) {
			System.out.println("行走 " + str);
		}
	}
public class Client {
	public static void main(String[] args) {
		Animal animal = new Animal();
		animal.run("牛");
		animal.run("羊");
	}
}
```
运行了一下后，发现不对哈。鱼等水生的动物是不能行走的。所以我们要修改一下，遵循单一职责原则，让陆生动物使用Terrestrial这个类；而水生动物使用Aquatic这个类。
```java 
class Terrestrial{
		public void run(String animal){
			System.out.println(animal+"陆地行走");
		}
	}
class Aquatic{
		public void run(String animal){
			System.out.println(animal+"水中游行");
		}
	}
public class Client {
	public static void main(String[] args) {
		Terrestrial terrestrial = new Terrestrial();
		terrestrial.breathe("牛");
		terrestrial.breathe("羊");
		terrestrial.breathe("猪");

		Aquatic aquatic = new Aquatic();
		aquatic.breathe("鱼");
		}
	}	
```
遵循单一职责原的优点有：

>* 可以降低类的复杂度，一个类只负责一项职责，其逻辑肯定要比负责多项职责简单的多；
>* 提高类的可读性，提高系统的可维护性；
>* 变更引起的风险降低，变更是必然的，如果单一职责原则遵守的好，当修改一个功能时，可以显著降低对其他功能的影响。
>* 需要说明的一点是单一职责原则不只是面向对象编程思想所特有的，只要是模块化的程序设计，都适用单一职责原则。

### 开闭职责
定义：对修改关闭，对扩展开放。

### 接口分离原则
定义：客户端不应该依赖它不需要的接口；一个类对另一个类的依赖应该建立在最小的接口上。 

### 里氏代换原则
定义是：一个子类可以代替一个父类，完成其父类的功能。

### 依赖倒置原则
定义：高层模块不应该依赖低层模块，二者都应该依赖其抽象；抽象不应该依赖细节；细节应该依赖抽象。编程应该依赖抽象，不应该依赖细节。

### 迪米特原则
定义：类之间应该减少耦合。

### 优先使用组合，而不是使用继承
定义：这个就不用解释了吧，学习过面向对象编程的同学，都应该知道这个事。





