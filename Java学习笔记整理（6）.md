---
title: Java学习笔记整理（6）
date: 2017-07-11 14:39:45
tags: [J2SE,this关键字]
categories: J2SE
top: 6
---

---

<!--more-->

### 代码格式
```java
class Demo1Object {
	/*
	Java约定俗成
	1,类名接口名  一个单词首字母大写,多个单词每个单词首字母都大写
	2,方法名和变量名 一个单词全部小写,多个单词从第二个单词首字母大写
	建议:如果能用英语尽量用英语,实在不行用汉语拼音
	代码书写格式
	1,大括号成对写,左大括号在该行代码的最后,右大括号在该行代码的下边,并与该行代码对齐
	2,左大括号前面有空格
	3,并排语句中间都需要加空格
	4,语句块或者方法中间加空行
	*/
	public static void main(String[] args) {
		for (; ; ) {
		}
		
		while () {															
		}

		if () {
		}else {
			System.out.println();}	
			
		}

	}	
	public static void print() {
		
	}
}
```

### 面向对象思想概述(了解)
* A:面向过程思想概述
	* 第一步
	* 第二步 
* B:面向对象思想概述
	* 找对象(第一步,第二步) 
* C:举例
	* 买煎饼果子
	* 洗衣服 
* C:面向对象思想特点
	* a:是一种更符合我们思想习惯的思想
	* b:可以将复杂的事情简单化
	* c:将我们从执行者变成了指挥者
		* 角色发生了转换

面向对象的思想更为简洁方便，不用所有行为都需要自己去做（都交给对象对做），同时我也认为这也是高度解耦的一种形式。

		
### 面向对象开发,设计以及特征(了解)
* A:面向对象开发
	* 就是不断的创建对象，使用对象，指挥对象做事情。
* B:面向对象设计
	* 其实就是在管理和维护对象之间的关系。
* C:面向对象特征
	* 封装(encapsulation)
	* 继承(inheritance)
	* 多态(polymorphism)

### 类与对象概述
* A:我们学习编程是为了什么
	* 为了把我们日常生活中实物用学习语言描述出来
* B:我们如何描述现实世界事物
	* 属性	就是该事物的描述信息(事物身上的名词)
	* 行为	就是该事物能够做什么(事物身上的动词)
* C:Java中最基本的单位是类,Java中用class描述事物也是如此
	* 成员变量	就是事物的属性
	* 成员方法	就是事物的行为
* D:定义类其实就是定义类的成员(成员变量和成员方法)
	* a:成员变量	和以前定义变量是一样的，只不过位置发生了改变。在类中，方法外。
	* b:成员方法	和以前定义方法是一样的，只不过把static去掉，后面在详细讲解static的作用。
* E:类和对象的概念
	* a:类：是一组相关的属性和行为的集合
	* b:对象：是该类事物的具体体现
	* c:举例：
		* 类	 学生
		* 对象	具体的某个学生就是一个对象


		
### 学生类的定义
* A:学生事物
* B:学生类
* C:案例演示
	* 属性:姓名,年龄,性别
	* 行为:学习,睡觉

### 手机类的定义
* 模仿学生类，让学生自己完成
	* 属性:品牌(brand)价格(price)
	* 行为:打电话(call),发信息(sendMessage)玩游戏(playGame)

### 学生类的使用
* A:文件名问题
	* 在一个java文件中写两个类：一个基本的类，一个测试类。
	* 建议：文件名称和测试类名称一致。
* B:如何使用对象?
	* 创建对象并使用
	* 格式：类名 对象名 = new 类名();
* D:如何使用成员变量呢?
	* 对象名.变量名
* E:如何使用成员方法呢?
	* 对象名.方法名(...)

```java
class Demo3Student {							//测试类,包含主函数,测试类的类名要与文件名一致
	public static void main(String[] args) {
		/*
		* 创建对象并使用
		* 格式：类名 对象名 = new 类名();
		*/

		Student s = new Student();				//创建对象
		//String name = s.name;					//获取对象的属性(成员变量)
		//int age = s.age;
		//char gender = s.gender;
		s.name = "张三";
		s.age = 23;
		s.gender = '男';

		s.study();								//获取对象的行为(成员方法)
		s.sleep();

		s.name = "李四";
		s.study();
	}
}


class Student {
	/*
	* 属性:姓名,年龄,性别(成员变量)
	* 行为:学习,睡觉(成员方法)
	*/

	String name ;
	int age ;
	char gender ;

	public void study() {
		System.out.println(name + "在学习");
	}

	public void sleep() {
		System.out.println(name + "在睡觉");
	}
}
```
	
### 手机类的使用
* A:学生自己完成
	* 模仿学生类，让学生自己完成

```java
class Demo4Phone {
	public static void main(String[] args) {
		Phone p = new Phone();						//创建对象在最后有小括号
		p.brand = "三星";							//调用属性的时候后面没有括号
		p.price = 14999;
		p.color = "天空灰";

		p.call();									//调用方法的时候后面有括号

		System.out.println("我是一部" + p.brand + "手机,价格是:" + p.price + ",颜色是:" + p.color);
	}
}

/*
 属性:品牌(brand)价格(price)
* 行为:打电话(call),发信息(sendMessage)玩游戏(playGame)
*/

class Phone {
	String brand;
	int price;
	String color;

	public void call() {
		System.out.println("打电话");
	}

	public void sendMessage() {
		System.out.println("发信息");
	}

	public void playGame() {
		System.out.println("玩游戏");
	}
}
```
		
### 一个对象的内存图
* A:画图演示
	* 一个对象
![](http://ogtmd8elu.bkt.clouddn.com/201707110908_530.png)
所有代码进入方法区，换成xxx.class字节码文件，等待调用，同时main方法入栈，开始调用方法区的class字节码文件并写入内存，如果是new出一个对象（所有new出来的对象，都存储在栈上，并有一个地址，通过地址访问），就在栈里面开辟空间并初始化赋值。

输出结束以后，方法进行弹栈，对象会被不定时的回收。
### 二个对象的内存图
* A:画图演示
	* 二个不同的对象
![](http://ogtmd8elu.bkt.clouddn.com/201707110920_960.png)
运行程序以后，java文件从硬盘上加载到方法区，转换成计算机可执行的字节码（类似一个代码仓库，等待调用），同时main方法入栈，new出来对象存储在栈上，并存储成员变量，并把地址传给person，进行访问。当方法执行完毕以后，进行弹栈。

每new一次就会产生一个新的对象，内存充足的时候，并不会被回收覆盖。

### 三个对象的内存图
* A:画图演示
	* 三个引用，有两个对象的引用指向同一个地址
![](http://ogtmd8elu.bkt.clouddn.com/201707110931_302.png)



### 成员变量和局部变量的区别
* A:在类中的位置不同
	* 成员变量：在类中方法外
	* 局部变量：在方法定义中或者方法声明上
* B:在内存中的位置不同
	* 成员变量：在堆内存(成员变量属于对象,对象进堆内存)
	* 局部变量：在栈内存(局部变量属于方法,方法进栈内存)
* C:生命周期不同
	* 成员变量：随着对象的创建而存在，随着对象的消失而消失
	* 局部变量：随着方法的调用而存在，随着方法的调用完毕而消失
* D:初始化值不同
	* 成员变量：有默认初始化值
	* 局部变量：没有默认初始化值，必须定义，赋值，然后才能使用。
	
* 注意事项：
	* 局部变量名称可以和成员变量名称一样，在方法中使用的时候，采用的是就近原则,自己有就不用其他的了。

![](http://ogtmd8elu.bkt.clouddn.com/201707110944_8.png)

```java
class Demo1Person {
	public static void main(String[] args) {
		Person p = new Person();			//创建对象
		p.name = "张三";					//给对象属性赋值
		p.age = 23;

		p.speak(20);
	}
}

class Person {
	String name;							//定义在类中,方法外,成员变量
	int age;

	public void speak(int y) {				//定义在方法上,局部变量
		int x = 10;							//定义在方法内,局部变量
		int age = 30;						//java中有就近原则
		System.out.println(name+ "," + age);

		System.out.println(x + "," + y);
	}
}
```
### 方法的形式参数或返回值是类名的时候如何调用
* A:方法的参数是类名public void print(Student s){}//print(new Student());
	* 如果你看到了一个方法的形式参数是一个类类型(引用类型)，这里其实需要的是该类的对象。
* B:方法的返回值是类名public Student print(){return s}
	* 如果你看到了一个方法的返回值是类名,这里其实返回的是该类对象

```java
class Demo2Student {
	public static void main(String[] args) {
		Test1 t = new Test1();
		//Student stu = new Student();				//s = 0x0011
		//t.run(stu);

		Student stu = t.aaa();						//s = 0x0022
		int x = t.xxx();
	}
}

class Student {
	String name;
	int age;

	public void speak() {
		System.out.println(name + "," +  age);
	}
}

class Test1 {
	public void run(Student s) {			//参数是引用数据类型,调用这个方法时传递的是该类的对象
		System.out.println("run");
	}

	public Student aaa() {					//返回值类型是引用数据类型,返回的就是该类对象
		Student s = new Student();			//s = 0x0022
		return s;
	}

	public int xxx() {
		int x = 10;
		return x;
	}

	public String bbb() {
		return "aaa";
	}
}
```
#### 方法的形式参数是类名的调用过程
![](http://ogtmd8elu.bkt.clouddn.com/201707111041_440.png)
#### 返回值是类名的调用过程
![](http://ogtmd8elu.bkt.clouddn.com/201707111045_139.png)

本质上是通过分配的地址找到对象，以及访问到其中的数据。

### 匿名对象的概述和应用
* A:什么是匿名对象
* B:匿名对象应用场景
	* a:调用方法，仅仅只调用一次的时候。
		* 那么，这种匿名调用有什么好处吗?
			* 节省代码 
		* 注意：调用多次的时候，不适合。匿名对象调用完毕就是垃圾。可以被垃圾回收器回收。
	* b:匿名对象可以作为实际参数传递
* C:案例演示
	* 匿名对象应用场景

```java 
class Demo1Car {
	public static void main(String[] args) {
		Car c1 = new Car();					//c1是对象的名字
		//new Car();							//匿名对象

		c1.color = "red";
		c1.num = 8;
		c1.run();

		new Car().color = "blue";			//匿名对象是否可以调用属性?可以但是没有意义
		new Car().num = 4;
		new Car().run();					//匿名对象是否可以调用方法?可以

		Car c2 = new Car();
		c2.print();
		c2.print();
		new Car().print();					//匿名对象调用方法的好处,只是节省了代码
		new Car().print();					//如果对同一个方法多次调用,必须用有名字的对象
		new Car().print();					//因为匿名对象会创建多个对象,浪费空间
	}
}

class Car {
	String color;
	int num;

	public void run() {
		System.out.println(color + "," + num);
	}

	public void print() {
		System.out.println("11111111111111111111111111");
	}
}
```

![](http://ogtmd8elu.bkt.clouddn.com/201707111118_143.png)


```java
class Demo2Car {
	public static void main(String[] args) {
		//Car c1 = new Car();
		/*c1.color = "black";
		c1.num = 4;
		c1.run();*/
		//method(c1);

		method(new Car());					//匿名对象可以当作参数传递

		Car c2 = new Car();
		/*c2.color = "black";
		c2.num = 4;
		c2.run();*/
		method(c2);
	}

	public static void method(Car cc) {		//Car cc = new Car();
		cc.color = "black";
		cc.num = 4;
		cc.run();
	}
}

class Car {
	String color;
	int num;
	public void run() {
		System.out.println(color + "," + num);
	}
}
```
	
### 封装的概述
* A:封装概述
	* 是指隐藏对象的属性和实现细节，仅对外提供公共访问方式。
* B:封装好处
	* 隐藏实现细节，提供公共的访问方式
	* 提高了代码的复用性
	* 提高安全性。
* C:封装原则
	* 将不需要对外提供的内容都隐藏起来。
	* 把属性隐藏，提供公共方法对其访问。

```java

/*
封装的作用。
 
*/

class Demo1Person {
	public static void main(String[] args) {
		Person p = new Person();
		p.name = "dayone";
		//p.age = -17;
		p.setAge(-17);
		System.out.println(p.getAge());
	}
}

class Person {
	String name;
	private int age;							//private私有,只能在本类访问

	public void speak() {
		System.out.println(name + "," +  age);
	}

	/*
	对外提供公共的访问方法,分别是setXxx()和getXxx()
	可以写if else，对程序进行控制，保证数据的安全性。
	*/
	public void setAge(int a) {
		if (a > 0 && a < 200) {
			age = a;
		}else {
			System.out.println("回火星吧");
		}
		
	}

	public int getAge() {
		return age;
	}
}
```
	
### private关键字的概述和特点
* A:人类赋值年龄的问题
* B:private关键字特点
	* a:是一个权限修饰符
	* b:可以修饰成员变量和成员方法
	* c:被其修饰的成员只能在本类中被访问
* C:案例演示
	* 封装和private的应用：
	* A:把成员变量用private修饰
	* B:提高对应的getXxx()和setXxx()方法
	* private仅仅是封装的一种体现形式,不能说封装就是私有

### this关键字的概述和应用
* A:this关键字特点
* B:案例演示
	* this的应用场景

![](http://ogtmd8elu.bkt.clouddn.com/201707111403_715.png)

```java
class Demo1This {
	public static void main(String[] args) {
		Person p = new Person();
		p.setName("王学兵");
		p.setAge(44);

		//System.out.println(p.getName() + "年龄是:" + p.getAge() + "又进去了");

		p.print();
		System.out.println(p);

		System.out.println("=========================");
		Person p2 = new Person();
		p2.print();
		System.out.println(p2);
	}
}

class Person {
	private String name;
	private int age;

	//变量名要求见名只意
	public void setName(String name) {	//name = "王学兵"
		this.name = name;				//this代表当前对象的引用
	}									//局部变量绝对不能用对象.调用

	public String getName() {
		return name;
	}

	public void setAge(int a) {
		age = a;
	}

	public int getAge() {
		return age;
	}

	public void print() {
		System.out.println("print方法中的:" + this);
		method();
	}

	public void method() {
		System.out.println("method");
	}
	/*
	this可以用来区分成员变量和局部变量重名,用this.调用的是成员变量
	this可以调用成员方法
	*/
}
```
### 标准的手机类代码及其测试
* A:学生练习
	* 请把手机类写成一个标准类，然后创建对象测试功能。
```java
class Demo2This {
	public static void main(String[] args) {
		Phone p = new Phone();
		p.setBrand("锤子");
		p.setPrice(2480);
		p.setColor("白色");

		System.out.println("罗永浩生产了" + p.getBrand() + "手机,价格是:" + p.getPrice() + ",颜色是:" + p.getColor());

		p.call();
		p.sendMessage();
		p.playGame();
	}
}
/*
请把手机类写成一个标准类，然后创建对象测试功能。
*/

class Phone {
	private String brand;			//品牌
	private int price;				//价格
	private String color;			//颜色

	public void setBrand(String brand) {		//setXxx方法,修改值
		this.brand = brand;
	}

	public void setPrice(int price) {
		this.price = price;
	}

	public void setColor(String color) {
		this.color = color;
	}

	public String getBrand() {					//getXxx方法,获取值
		return brand;
	}

	public int getPrice() {
		return price;
	}

	public String getColor() {
		return color;
	}

	public void call() {
		System.out.println("打电话");
	}

	public void sendMessage() {
		System.out.println("发信息");
	}

	public void playGame() {
		System.out.println("玩游戏");
	}
}
```
