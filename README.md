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
运行了一下后，发现不对哈。鱼等水生的动物是不能行走的,那么怎么办呢？所以我们要修改一下，遵循单一职责原则，让陆生动物使用Terrestrial这个类；而水生动物使用Aquatic这个类。
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
>定义：高层模块不应该依赖低层模块，二者都应该依赖其抽象；抽象不应该依赖细节；细节应该依赖抽象。编程应该依赖抽象，不应该依赖细节。

>问题由来：类A直接依赖类B，假如要将类A改为依赖类C，则必须通过修改类A的代码来达成。这种场景下，类A一般是高层模块，负责复杂的业务逻辑；类B和类C是低层模块，负责基本的原子操作；假如修改类A，会给程序带来不必要的风险。

>解决方案：将类A修改为依赖接口I，类B和类C各自实现接口I，类A通过接口I间接与类B或者类C发生联系，则会大大降低修改类A的几率。

依赖倒置原则的核心思想是面向接口编程，我们依旧用一个例子来说明面向接口编程比相对于面向实现编程好在什么地方。场景是这样的，母亲给孩子讲故事，只要给她一本书，她就可以照着书给孩子讲故事了。举个例子说明一下：
```java
class Book {
		public String getContent() {
			return "这个是一个关于毛主席的故事……";
		}
	}

	class Mother {
		public void narrate(Book book) {
			System.out.println("妈妈开始讲故事");
			System.out.println(book.getContent());
		}
	}

	public class Client {
		public static void main(String[] args) {
			Mother mother = new Mother();
			mother.narrate(new Book());
		}
	}
```
运行结果：

妈妈开始讲故事

这个是一个关于毛主席的故事……

运行良好，假如有一天，需求变成这样：不是给书而是给一份报纸，让这位母亲讲一下报纸上的故事，报纸的代码如下：
```java 
class NewsPaper {
		public String getContent() {
			return "中国篮球超人姚明……";
		}
	}

```
这位母亲就办不到，因为她居然不会读报纸上的故事，这太荒唐了，只是将书换成报纸，居然必须要修改Mother才能读。假如以后需求换成杂志呢？换成网页呢？还要不断地修改Mother，这显然不是好的设计。原因就是Mother与Book之间的耦合性太高了，必须降低他们之间的耦合度才行。

我们引入一个抽象的接口IReader。读物，只要是带字的都属于读物：
```java
public Interface IReader{
    public String getContent();
}
```
Mother类与接口IReader发生依赖关系，而Book和Newspaper都属于读物的范围，它们各自都去实现IReader接口，这样就符合依赖倒置原则了，代码修改为：
```java
	class Newspaper implements IReader {
		public String getContent() {
			return "中国篮球超人姚明……";
		}
	}

	class Book implements IReader {
		public String getContent() {
			return "这个是一个关于毛主席的故事……";
		}
	}

	class Mother {
		public void narrate(IReader reader) {
			System.out.println("妈妈开始讲故事");
			System.out.println(reader.getContent());
		}
	}

	public class Client {
		public static void main(String[] args) {
			Mother mother = new Mother();
			mother.narrate(new Book());
			mother.narrate(new Newspaper());
		}
	}
```
这样以后，妈妈就是万能的了，主要是读物，妈妈都可以讲给我了。
>这样修改后，无论以后怎样扩展Client类，都不需要再修改Mother类了。这只是一个简单的例子，实际情况中，代表高层模块的Mother类将负责完成主要的业务逻辑，一旦需要对它进行修改，引入错误的风险极大。所以遵循依赖倒置原则可以降低类之间的耦合性，提高系统的稳定性，降低修改程序造成的风险。

>采用依赖倒置原则给多人并行开发带来了极大的便利，比如上例中，原本Mother类与Book类直接耦合时，Mother类必须等Book类编码完成后才可以进行编码，因为Mother类依赖于Book类。修改后的程序则可以同时开工，互不影响，因为Mother与Book类一点关系也没有。参与协作开发的人越多、项目越庞大，采用依赖导致原则的意义就越重大。现在很流行的TDD开发模式就是依赖倒置原则最成功的应用。

>传递依赖关系有三种方式，以上的例子中使用的方法是接口传递，另外还有两种传递方式：构造方法传递和setter方法传递，相信用过Spring框架的，对依赖的传递方式一定不会陌生。

>在实际编程中，我们一般需要做到如下3点：

>*低层模块尽量都要有抽象类或接口，或者两者都有。
>*变量的声明类型尽量是抽象类或接口。
>*使用继承时遵循里氏替换原则。
依赖倒置原则的核心就是要我们面向接口编程，理解了面向接口编程，也就理解了依赖倒置。


### 迪米特原则
定义：类之间应该减少耦合。

### 优先使用组合，而不是使用继承
定义：这个就不用解释了吧，学习过面向对象编程的同学，都应该知道这个事。





