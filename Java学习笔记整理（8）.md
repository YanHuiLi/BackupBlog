---
title: Java学习笔记整理（8）
date: 2017-07-14 11:28:20
tags: [J2SE]
categories: [J2SE]
top: 8
---

---
1.局部代码块，构造代码块，静态代码块
2.Java的继承问题，不支持多继承。
3.this,super关键字的使用。构造函数的第一句。
4.方法的重写和重载。
5.final关键字的学习。
<!--more-->


### 代码块的概述和分类)
* A:代码块概述
  * 在Java中，使用{}括起来的代码被称为代码块。
* B:代码块分类
  * 根据其位置和声明的不同，可以分为局部代码块，构造代码块，静态代码块，同步代码块(多线程讲解)。
* C:常见代码块的应用
  * a:局部代码块 
    * 在方法中出现；限定变量生命周期，及早释放，提高内存利用率
  * b:构造代码块 
    * 在类中方法外出现；多个构造方法方法中相同的代码存放到一起，每次调用构造都执行，并且在构造方法前执行
  * c:静态代码块 
    * 在类中方法外出现，加了static修饰
    * 在类中方法外出现，并加上static修饰；用于给类进行初始化，在加载的时候就执行，并且只执行一次。


```java
class Demo1Code {
	static {
		System.out.println("Demo1Code");
	}
	
	public static void main(String[] args) {
		/*{									//局部代码块,开发不用
			int x = 10;						//限定变量的生命周期
			System.out.println(x);
		}*/

		Person p1 = new Person();
		p1.name = "张三";
		p1.age = 23;

		System.out.println("-------------------------");
		Person p2 = new Person("李四",24);
		
	}

	static {
		System.out.println("Demo1Code222222222222");
	}
}

class Person {
	String name;
	int age;

	public Person(){
		//cry();
		System.out.println("空参构造");
	}

	public Person(String name,int age) {
		//cry();
		this.name = name;
		this.age = age;
		System.out.println("有参构造");
	}

	{											//构造代码块(初始化块),开发很少用
		//System.out.println("构造代码块");		//构造代码块的作用,当对象具备相同的属性或相同的行为
		cry();									//就需要在每个构造函数中定义相同的属性或行为,那么太麻烦
	}											//都在构造代码块中定义即可

	static {									//静态代码块是随着类的加载而加载,而且只执行一次
		System.out.println("静态代码块");		//静态代码块是优先于主方法执行的
	}											//静态代码块的作用,用于加载驱动
	public void cry() {
		System.out.println("哇哇!!!");
	}
}
```

### 代码块的面试题
* A:看程序写结果
* 
   class Student {
   		static {
   			System.out.println("Student 静态代码块");
   		}
   		
   		{
   			System.out.println("Student 构造代码块");
   		}
   		
   		public Student() {
   			System.out.println("Student 构造方法");
   		}
   	}
   	
   	class StudentDemo {
   		static {
   			System.out.println("静态代码块,带你装逼带你飞");
   		}
   		
   		public static void main(String[] args) {
   			System.out.println("我是main方法");
   			
   			Student s1 = new Student();
   			Student s2 = new Student();
   		}
   	}
```java
class Student {
	static {
		System.out.println("Student 静态代码块");
	}
	
	{
		System.out.println("Student 构造代码块");
	}
	
	public Student() {
		System.out.println("Student 构造方法");
	}
}
	
class StudentDemo {
	static {
		System.out.println("静态代码块,带你装逼带你飞");
	}
	
	public static void main(String[] args) {
		System.out.println("我是main方法");
		
		Student s1 = new Student();
		Student s2 = new Student();
	}
}

/*
out:
静态代码块,带你装逼带你飞
我是main方法
Student 静态代码块
Student 构造代码块
Student 构造方法
Student 构造代码块
Student 构造方法
*/
```

### 继承案例演示
* A:继承的格式(extends)
* B:继承案例演示：
  * 动物类,猫类,狗类
  * 定义两个属性(颜色,腿的个数)两个功能(吃饭，睡觉)
* C:案例演示
  * 使用继承前
* D:案例演示
  * 使用继承后

```java
class Demo1Extends {
	/*
	 B:继承案例演示：
	* 动物类,猫类,狗类
	* 定义两个属性(颜色,腿的个数)两个功能(吃饭，睡觉)
	* 代码复用性提到非常大的提高。
	* 原则是少用继承多用组合。
	*/
	public static void main(String[] args) {
		Dog d = new Dog();
		d.eat();
		d.sleep();
		d.color = "黑";
		d.leg = 2;

		System.out.println(d.color + "," + d.leg);
	}
}

class Animal {
	String color;
	int leg;

	public void eat() {
		System.out.println("吃饭");
	}
	
	public void sleep() {
		System.out.println("睡觉");
	}
}

class Cat extends Animal {
	/*String color;
	int leg;

	public void eat() {
		System.out.println("吃饭");
	}
	
	public void sleep() {
		System.out.println("睡觉");
	}*/
}

class Dog extends Animal {
	
}
```
### 继承的好处和弊端
* A:继承的好处
  * a:提高了代码的复用性
  * b:提高了代码的维护性
  * c:让类与类之间产生了关系，是多态的前提(基础)
* B:继承的弊端
  * 类的耦合性增强了。

  * 开发的原则：高内聚，低耦合。
  * 耦合：类与类的关系
  * 内聚：就是自己完成某件事情的能力

### Java中类的继承特点
* A:Java中类的继承特点
  * a:Java只支持单继承，不支持多继承。(一个儿子只能有一个爹)
    * 有些语言是支持多继承，格式：extends 类1,类2,...
  * b:Java支持多层继承(继承体系)
* B:案例演示
  * Java中类的继承特点
    * 如果想用这个体系的所有功能用最底层的类创建对象
    * 如果想看这个体系的共性功能,看最顶层的类 
```java
class Demo2 {
	public static void main(String[] args) {
		DemoC d = new DemoC();
		d.show1();
		d.show2();
	}
}

class DemoA {
	public void show1() {
		System.out.println("DemoA");
	}
}

class DemoB extends DemoA {
	public void show2() {
		System.out.println("DemoB");
	}
}

class DemoC extends DemoB {	//java只支持单继承,java支持的多层继承
	public void show3() {
		System.out.println("DemoC");
	}
}
```
### 继承的注意事项和什么时候使用继承
* A:继承的注意事项
  * a:子类只能继承父类所有非私有的成员(成员方法和成员变量)
  * b:子类不能继承父类的构造方法，但是可以通过super(马上讲)关键字去访问父类构造方法。
  * c:不要为了部分功能而去继承
* B:什么时候使用继承
  * 继承其实体现的是一种关系："is a"。
    Person
    	Student
    	Teacher
    水果
    	苹果
    	香蕉
    	橘子

  采用假设法。
  	如果有两个类A,B。只有他们符合A是B的一种，或者B是A的一种，就可以考虑使用继承。



### 继承中成员变量的关系
* A:案例演示
  * a:不同名的变量
  * b:同名的变量


### this和super的区别和应用
* A:通过问题引出super
  * 子类局部范围访问父类成员变量
* B:说说this和super的区别
* C:this和super的使用
  * a:调用成员变量
    * this.成员变量 调用本类的成员变量,也可以调用父类的成员变量
    * super.成员变量 调用父类的成员变量
  * b:调用构造方法
    * this(...)	调用本类的构造方法
      * super(...)调用父类的构造方法
  * c:调用成员方法
    * this.成员方法 调用本类的成员方法,也可以调用父类的方法
    * super.成员方法 调用父类的成员方法

* D:案例演示
* 
   看程序写结果1
   	class Fu{
   		public int num = 10;
   		public Fu(){
   			System.out.println("fu");
   		}
   	}
   	class Zi extends Fu{
   		public int num = 20;
   		public Zi(){
   			System.out.println("zi");
   		}
   		public void show(){
   			int num = 30;
   			System.out.println(num);
   			System.out.println(this.num);
   			System.out.println(super.num);
   		}
   	}
   	class Test {
   		public static void main(String[] args) {
   			Zi z = new Zi();
   			z.show();
   		}
   	}
   注意：b，c马上讲
```java
class Fu{
	public int num = 10;
	public Fu(){
		//super();
		//System.out.println("fu");
	}
}
class Zi extends Fu{
	public int num = 20;
	public Zi(){
		//super();								//访问父类中空参的构造函数
		System.out.println("zi");
	}
	public void show(){
		int num = 30;
		System.out.println(num);
		System.out.println(this.num);
		System.out.println(super.num);
	}
}
class Test {
	public static void main(String[] args) {
		Zi z = new Zi();
		z.show();
	}
}
/*
实例化zi对象的时候，会优先调用fu的构造函数，因为里面有输出语句。
平时看不见是因为里面没有语句，把语句注释了就没有输出
out：
fu
zi
30
20
10

在每个构造方法里面，都会存在一个super();方法。
因为是层层继承下来的，根节点在object类
*/
```



```java
class Demo3Extends {
	public static void main(String[] args) {
		Zi z = new Zi();
		z.show();
	}
}

class Fu {
	int num1 = 10;
	int num2 = 30;
}

class Zi extends Fu {
	int num2 = 20;

	public void show() {				//就近原则,自己有就不用父类的
		System.out.println(super.num1);
		System.out.println(this.num2);
		//System.out.println(super.num2);	//super可以调用父类的成员变量
	}
}

/*
this和super的区别
1.this可以访问自己的成员变量,也可以访问父类的成员变量(本类没有这个成员变量)
  super只能访问父类的成员变量
*/
```
### 继承中构造方法的关系
* A:案例演示
  * 子类中所有的构造方法默认都会访问父类中空参数的构造方法(隐藏了super()方法)；
* B:为什么呢?
  * 因为子类会继承父类中的数据，可能还会使用父类的数据。
  * 所以，子类初始化之前，一定要先完成父类数据的初始化。

  * 其实：
    * 每一个构造方法的第一条语句默认都是：super()在这里简单的提一句，Object类。否则有人就会针对父类的构造方法有疑问。Object在没有父类了。

### 继承中构造方法的注意事项
* A:案例演示
  * 父类没有无参构造方法,子类怎么办?
  * super解决
  * this解决
* B:注意事项
  * super(…)或者this(….)必须出现在第一条语句上

```java
class Demo4Extends {
	public static void main(String[] args) {
		//Student s = new Student("张三",23);
		Student s = new Student();
		System.out.println(s.getName() + "," + s.getAge());
	}
}

class Person {
	private String name;
	private int age;

	/*public Person() {
	
	}*/

	public Person(String name,int age) {
		this.name = name;
		this.age = age;
	}

	public void setName(String name) {
		this.name = name;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}
}

class Student extends Person {
	public Student() {
		//super("李四",24);					//构造函数中有super没this,有this没super
											//this和super语句必须放在构造函数的第一行（必须先进行初始化）
											//无论如何子类必须要访问父类的构造函数
		this("李四",24);					//调用本类其他的构造方法
	}

	public Student(String name,int age) {	//name 李四,age 24
		super(name,age);
	}

}
```

### 继承中的面试题

* A:案例演示
* 
   看程序写结果2
   	class Fu {
   		static {
   			System.out.println("静态代码块Fu");
   		}
   	
   		{
   			System.out.println("构造代码块Fu");
   		}
   	
   		public Fu() {
   			System.out.println("构造方法Fu");
   		}
   	}
   	
   	class Zi extends Fu {
   		static {
   			System.out.println("静态代码块Zi");
   		}
   	
   		{
   			System.out.println("构造代码块Zi");
   		}
   	
   		public Zi() {
   			System.out.println("构造方法Zi");
   		}
   	}
   	
   	Zi z = new Zi(); 请执行结果。

![](http://ogtmd8elu.bkt.clouddn.com/201707140801_420.png)

```java
class Fu {
	static {
		System.out.println("静态代码块Fu");
	}

	{
		System.out.println("构造代码块Fu");
	}

	public Fu() {
		System.out.println("构造方法Fu");
	}
}

class Zi extends Fu {
	static {
		System.out.println("静态代码块Zi");
	}

	{
		System.out.println("构造代码块Zi");
	}

	public Zi() {
		super();  //隐藏了这个super，因为是继承关系，所以都会调用。
		System.out.println("构造方法Zi");
	}
}


class Demo5Extends {
	public static void main(String[] args) {
		Zi z = new Zi();
		/*
		1,Demo5Extends.class加载进内存
		2,主方法进栈
		3,需要执行Zi z = new Zi();这句话,但是在内存还没有加载Zi.class
		4,Zi继承了Fu,所以Fu.class先加载进内存,同时父类的静态代码块也加载进内存
		5,Zi.class加载进内存,同时子类的静态代码块也加载进内存
		6,执行Zi类的构造方法,子类构造方法中隐藏着super,super会访问父类中的构造方法
		7,在父类构造方法执行之前,会先看是否有构造代码块,有先执行构造代码块
		8,执行父类构造方法
		9,执行子类构造方法,在执行之前会看是否有构造代码块,有先执行
		10,执行子类构造方法
		
		
		构造代码块加载的优先级高于构造方法
		静态代码块随着类的加载进内存而加载。
		out: 
		静态代码块Fu
		静态代码块Zi
		构造代码块Fu
		构造方法Fu
		构造代码块Zi
		构造方法Zi
		*/
		
	}
}
```


### 继承中成员方法关系
* A:案例演示
  * a:不同名的方法
  * b:同名的方法

```java
class Demo6Extends {
	public static void main(String[] args) {
		Zi z = new Zi();
		z.show1();
		z.show2();
	}
}

class Fu {
	public void show1() {
		System.out.println("show1");
	}
}

class Zi extends Fu {
	int x=10;             //没问题
	//  x=20; 非法的，报错，语句必须写在方法里面，for语句，while语句，都不能写在类中方法外。 
	public void show1() {						//方法的复写(重写)
		super.show1();							//语句都需要定义在方法中
		System.out.println("Zi show1");			//如果子父类方法构成重写,想访问父类的方法需要用super.
	}

	public void show2() {
		System.out.println("show2");
	}

}
```

### 方法重写概述及其应用
* A:什么是方法重写
* B:方法重写的应用：
  * 当子类需要父类的功能，而功能主体子类有自己特有内容时，可以重写父类中的方法。这样，即沿袭了父类的功能，又定义了子类特有的内容。
* C:案例演示
  * a:定义一个手机类。

```java
class Demo8Extends {
	
	
	//这个例子说明了， 当父类的方法过时的时候，通过继承实现方法的复写，完成新功能。
	
	public static void main(String[] args) {
		DayOne d = new DayOne();
		d.PaoNiu();
	}
}

class Tom {
	public void PaoNiu() {
		System.out.println("唱红歌,搞定林夕合鸟女士");
	}

}

class DayOne extends Tom {
	public void PaoNiu() {
		System.out.println("霸王硬上弓");
	}
}
```


可以用super语句调用到父类的之前拥有的功能，并且可以让子类拥有新功能，和特性。
```java
class Demo7Extends {
	public static void main(String[] args) {
		Iphone4s i = new Iphone4s();			//创建子类对象,先从子类找方法,如果没有找父类的
		i.siri();
		i.call();
	}
}

class Iphone4 {
	public void siri() {
		System.out.println("speak english");
	}

	public void call() {
		System.out.println("call");
	}

	public Iphone4(){}
}

class Iphone4s extends Iphone4 {
	public void siri() {
		super.siri();
		System.out.println("说中文");
	}

	public Iphone4s(){
		super();					//super()语句和this()语句必须放在构造方法中
	}
}
```

### 方法重写的注意事项
* A:方法重写注意事项
  * a:父类中私有方法不能被重写
    * 因为父类私有方法子类根本就无法继承
  * b:子类重写父类方法时，访问权限不能更低（为了保证功能的提升性）
    * 最好就一致
  * c:父类静态方法，子类也必须通过静态方法进行重写
    * 其实这个算不上方法重写，但是现象确实如此，至于为什么算不上方法重写，多态中我会讲解(静态只能覆盖静态)，因为相当于存在两个方法名一样的方法，java虚拟机不知道调用哪个方法。

  * 子类重写父类方法的时候，最好声明一模一样。
* B:案例演示
  * 方法重写注意事项

### 方法重写的面试题的面试题
* A:方法重写的面试题——重写和重载的区别

  * Override（重写）和Overload（重载）的区别?Overload能改变返回值类型吗?
  * overload可以改变返回值类型,只看参数列表
  * 方法重写：子类中出现了和父类中方法声明一模一样的方法。
  * 方法重载：本类中出现的方法名一样，参数列表不同的方法。与返回值无关。
  * 子类对象调用方法的时候：
    * 先找子类本身，再找父类。（如果子类没有，就找父类的）

方法的重写（override），是**子类中出现了和父类中方法声明一模一样的方法**，是因为子类需要拥有更多的功能，可以在继承父类方法的基础上，添加更多的功能，所以使用重写。

方法的重载（overload），是**本类**中出现了方法名一样，参数列表不一样的方法（顺序，个数，参数类型），与返回值无关，使用重载是因为避免了要写和调用大量的方法，节省了内存。

### 构造方法中的问题


```java
class Demo7Extends {
	public static void main(String[] args) {
		Iphone4s i = new Iphone4s();			//创建子类对象,先从子类找方法,如果没有找父类的
		i.siri();
		i.call();
	}
}

class Iphone4 {
	public void siri() {
		System.out.println("speak english");
	}

	public void call() {
		System.out.println("call");
	}

	public Iphone4(){}
}

class Iphone4s extends Iphone4 {
	public void siri() {
		super.siri();
		System.out.println("说中文");
	}

	public Iphone4s(){
		super();					//super()语句和this()语句必须放在构造方法中
	}
}
```

super()和this()方法只能放在构造方法里面，注意区分，
可以使用super.xxx();调用父类的xxx方法给子类。


### 使用继承前的学生和老师案例
* A:案例演示
  * 使用继承前的学生和老师案例

```java
/*
学生类
	属性:姓名,年龄
	行为:学习,吃饭
老师类
	属性:姓名,年龄
	行为:讲课,吃饭
*/
class Demo9Extends {
	public static void main(String[] args) {
		
	}
}

class Student {
	private String name;
	private int age;

	public Student(){}

	public Student(String name,int age) {
		this.name = name;
		this.age = age;
	}

	public void setName(String name) {
		this.name = name;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}

	public void study() {
		System.out.println("学习");
	}

	public void eat() {
		System.out.println("学生吃饭");
	}
}

class Teacher {
	private String name;
	private int age;

	public Teacher(){}

	public Teacher(String name,int age) {
		this.name = name;
		this.age = age;
	}

	public void setName(String name) {
		this.name = name;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}

	public void teach() {
		System.out.println("讲课");
	}

	public void eat() {
		System.out.println("老师吃饭");
	}
}
```

### 使用继承后的学生和老师案例
* A:案例演示
  * 使用继承后的学生和老师案例

```java
/*
学生类
	属性:姓名,年龄
	行为:学习,吃饭
老师类
	属性:姓名,年龄
	行为:讲课,吃饭

	学生和老师的共性
	姓名,年龄,吃饭
人类(Person)
	属性:姓名,年龄
	行为:吃饭
*/
class Demo10Extends {
	public static void main(String[] args) {
		System.out.println("Hello World!");
	}
}

class Person {
	private String name;
	private int age;

	public Person(){}

	public Person(String name,int age) {
		this.name = name;
		this.age = age;
	}

	public void setName(String name) {
		this.name = name;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}

	public void eat() {
		System.out.println("吃饭");
	}
}

class Student extends Person {
	public Student(){}

	public Student(String name,int age) {
		super(name,age);
	}

	public void study() {
		System.out.println("学生学习");
	}
}

class Teacher extends Person {
	public Teacher(){}

	public Teacher(String name,int age) {
		super(name,age);
	}

	public void teach() {
		System.out.println("老师讲课");
	}
}
```

### 猫狗案例分析,实现及测试
* A:猫狗案例分析
* B:案例演示
  * 猫狗案例继承版


```java
/*
* A:猫狗案例分析
* B:案例演示
	* 猫狗案例继承版

	猫类:
		属性:颜色,腿的个数
		行为:吃饭,抓老鼠
	狗类:
		属性:颜色,腿的个数
		行为:吃饭,看家
	动物类:
		相同的属性和行为
		属性:颜色,腿的个数
		行为:吃饭

	1,定义属性(私有)
	2,定义空参和有参构造(构造是没有返回值类型)
	3,setXxx和getXxx方法(根据属性)
	4,定义行为(方法)
*/
class Demo11Extends {
	public static void main(String[] args) {
		Dog d1 = new Dog();
		d1.setColor("花");
		d1.setLeg(4);

		System.out.println(d1.getColor() + "," + d1.getLeg());

		Dog d2 = new Dog("黑",2);
		System.out.println(d2.getColor() + "," + d2.getLeg());
	}
}

class Animal {
	private String color;
	private int leg;

	public Animal(){}

	public Animal(String color,int leg) {
		this.color = color;
		this.leg = leg;
	}

	public void setColor(String color) {
		this.color = color;
	}

	public void setLeg(int leg) {
		this.leg = leg;
	}

	public String getColor() {
		return color;
	}

	public int getLeg() {
		return leg;
	}

	public void eat() {
		System.out.println("吃饭");
	}
}

class Cat extends Animal {
	public Cat(){}

	public Cat(String color,int leg) {
		super(color,leg);
	}

	public void catchMouse() {
		System.out.println("抓老鼠");
	}
}

class Dog extends Animal {
	public Dog(){}

	public Dog(String color,int leg) {
		super(color,leg);
	}

	public void lookHome() {
		System.out.println("看家");
	}
}
```


### final关键字修饰类,方法以及变量的特点
* A:final概述
* B:final修饰特点
  * 修饰类，类不能被继承
  * 修饰变量，变量就变成了常量，只能被赋值一次
  * 修饰方法，方法不能被重写
* C:案例演示
  * final修饰特点

```java
/*
final是最终的
final可以修饰类,方法和变量
final修饰的类,不可以被继承(丁克家庭)
final修饰的方法,不可以被重写
final修饰的变量,又称之为常量
final修饰的变量就变成常量 名字要大写
*/
class Demo1Final {
	public static void main(String[] args) {
		Zi z = new Zi();
		z.method();
	}
}

/*final class Fu {
	public  void method() {
		System.out.println("访问底层文件");
	}
}*/

class Zi /*extends Fu*/ {
	final double PI = 3.14;
	public void method() {
		System.out.println("哈哈,被我干掉了");
	}
}
```


### final关键字修饰局部变量
* A:案例演示
  * 方法内部或者方法声明上都演示一下

  * 基本类型，是值不能被改变
  * 引用类型，是地址值不能被改变

```java
class Demo2Final {
	public static void main(String[] args) {
		final int num = 10;				//final修饰基本数据类型变量不改变其值
		System.out.println(num);

		final Person p1 = new Person("张三",23);//0x0011
		//p1 = new Person("李四",24);//0x0022
		p1.setName("李四");				//final修饰引用数据类型变量不改变其地址值
		p1.setAge(24);					//但是可以改变对象的属性值

		System.out.println(p1.getName() + "," + p1.getAge());
	}
}

class Person { 
	private String name;
	private int age;

	public Person(){}

	public Person(String name,int age) {
		this.name = name;
		this.age = age;
	}

	public void setName(String name) {
		this.name = name;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}
}
```

说白了，final修饰引用类型的数据的时候，不能再重新开辟内存空间，原内存地址不可更改，但是可以更改内部的属性。


### final修饰变量的初始化时机
* A:final修饰变量的初始化时机
  * 在对象构造完毕前即可
* B:案例演示
  * final修饰变量的初始化时机


```java
class Demo3Final {
	public static void main(String[] args) {
		Demo d = new Demo();
		//System.out.println(d.NUM);
		d.show();
	}
}
/*
final修饰的变量
	1,第一种赋值,定义时就直接赋值
	2,第二种赋值,构造函数赋值
*/
class Demo {
	
	final  int MAX = 100;  //第一种方式
	final int NUM;						
	
	public Demo() {
		NUM = 10;
	}
	public void show() {
		//NUM = 10;
		System.out.println(NUM);
	}
}
```
