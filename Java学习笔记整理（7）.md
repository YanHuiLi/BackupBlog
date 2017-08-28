---
title: Java学习笔记整理（7）
date: 2017-07-12 21:58:25
tags: [J2SE]
categories: [J2SE]
top: 7
---

---

<!--more-->



### 构造方法概述和格式
* A:构造方法概述和作用
	* 给对象的数据(属性)进行初始化
* B:构造方法格式特点
	* a:方法名与类名相同(大小也要与类名一致)
	* b:没有返回值类型，连void都没有
	* c:没有具体的返回值

### 构造方法的重载及注意事项（个数，类型，顺序）
* A:案例演示
	* 构造方法的重载
* B:构造方法注意事项
	* a:如果我们没有给出构造方法，系统将自动提供一个无参构造方法。
	* b:如果我们给出了构造方法，系统将不再提供默认的无参构造方法。
		* 注意：这个时候，如果我们还想使用无参构造方法，就必须自己给出。建议永远自己给出无参构造方法
* C:给成员变量赋值的两种方式
	* a:setXxx()方法
	* b:构造方法
```java
class ConDemo1 {
	public static void main(String[] args) {
		Student s = new Student();					//构造方法不用调用,一创建对象就执行。
		                                            //带有()就是调用方法，所以构造器会优先初始化
		System.out.println("--------------------");
		System.out.println(s.getName() + "," + s.getAge());

		Student s2 = new Student();	
		System.out.println(s2.getName() + "," + s2.getAge());

		System.out.println("--------------------");
		Student s3 = new Student("王五",25);
		System.out.println(s3.getName() + "," + s3.getAge());
	}
}

class Student {
	private String name;
	private int age;

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

	public Student() {					//构造方法:给对象的数据(属性)进行初始化
		name = "张三";                  //类在初始化，形成对象的同时，构造器就先被执行。
		age = 23              ;
		System.out.println("构造方法");
	}

	public Student(String name, int age) {
		this.name = name;
		this.age = age;
	}

	/*public Student(int age, String name) {		//与上面的构造方法是重载关系,开发不用(顺序不同，同样算重载，但是不建议使用)
		this.name = name;
		this.age = age;
	}*/

}
```

```java
class ConDemo2 {
	public static void main(String[] args) {
		Student s = new Student("张三",23);
		Test t = new Test();
		t.print();
	}
}

class Student {
	private String name;
	private int age;

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
	
	/*1.如果在一个类中没有定义构造方法,系统会默认给一个空参的构造方法
	格式是:public 类名(Student)(){}
	2,如果定义有参构造函数,没有定义空参构造函数,系统不会默认再给空参构造函数
	3,空参什么用,有参什么用?
		a 有参的构造函数就是为了给对象中的属性进行初始化用的
		b 空参在不需要给属性进行初始化,但是还要创建对象的时候使用
	4,定义了有参构造函数,要不要再定义空参的呢?
		建议把空参的加上,为了创建对象,调用成员使用
	*/
	public Student() {					//构造方法:给对象的数据(属性)进行初始化
		/*name = "张三";
		age = 23;
		System.out.println("构造方法");*/
	}

	public Student(String name, int age) {
		this.name = name;
		this.age = age;
	}

}

class Test {
	public void print() {
		System.out.println("print");
	}
}
```

### 构造方法和setXxx的区别

```java
class ConDemo3 {
	public static void main(String[] args) {
		Student s1 = new Student();
		s1.setName("张三");
		s1.setAge(23);

		System.out.println(s1.getName() + "," + s1.getAge());

		Student s2 = new Student("李四",24);
		//s2 = new Student("李五",25);			//新对象将原来对象的地址值覆盖,原对象变成垃圾
		s2.setName("李五");
		s2.setAge(25);
		System.out.println(s2.getName() + "," + s2.getAge());

		/*
		构造方法和setXxx方法的区别
		1,构造方法是用来给对象中的属性进行初始化的
		2,setXxx是用来修改属性值的,在原对象的基础上
		*/
	}
}

class Student {
	private String name;
	private int age;

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
	
	public Student() {					//构造方法:给对象的数据(属性)进行初始化
	
	}

	public Student(String name, int age) {
		this.name = name;
		this.age = age;
	}

}

```



### 成员方法的分类及使用
* A:成员方法分类
	* a:根据返回值
		* 有明确返回值方法
		* 返回void类型的方法
	* b:根据形式参数
		* 无参方法
		* 带参方法
* B:案例演示
	* 把各种方法都演示一下
```java
class MethodDemo4 {
	/*
	* A:成员方法分类
	* a:根据返回值
		* 有明确返回值方法
		* 返回void类型的方法
	* b:根据形式参数
		* 无参方法
		* 带参方法
	*/
	public static void main(String[] args) {
		MethodDemo4 md = new MethodDemo4();
		int num = md.sum(20,40);
		System.out.println(num);

		md.add(30,40);
	}

	/*
	求两个数的和(有返回值)
	1,明确返回值类型int
	2,明确参数列表 int a,int b
	*/

	public int sum(int a,int b) {
		return a + b;
	}

	/*
	求两个数的和(无返回值)
	1,明确返回值类型void
	2,明确参数列表 int a,int b
	*/

	public void add(int a,int b) {
		System.out.println(a + b);
	}

	/*
	求两个数的最大值(有参数)
	1,明确返回值类型int
	2,明确参数列表int a,int b
	*/

	public int max(int a,int b) {
		return a > b ? a : b;
	}

	/*
	求两个数的最大值(无参数)
	1,明确返回值类型int
	2,明确参数列表
	*/

	public int getMax() {
		int a = 10;
		int b = 20;
		return a > b ? a : b;
	}
}
```

	
### 一个标准学生类的代码及测试
* A:案例演示
	* 一个标准代码的最终版。

	* 学生类：
		* 成员变量：
			* name，age
		* 构造方法：
			* 无参，带两个参
		* 成员方法：
			* getXxx()/setXxx()
			* show()：输出该类的所有成员变量值
* B:给成员变量赋值：
	* a:setXxx()方法
	* b:构造方法
	
* C:输出成员变量值的方式：
	* a:通过getXxx()分别获取然后拼接
	* b:通过调用show()方法搞定

```java
class ConDemo5 {
	public static void main(String[] args) {
		
	}
}

class Person {								//javabean类
	private String name;					//成员变量需要私有
	private int age;

	public Person(){}						//定义空参的构造方法

	public Person(String name, int age) {	//定义有参的构造方法
		this.name = name;
		this.age = age;
	}

	public void setName(String name) {		//定义set和get成员方法
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
	
### 一个标准手机类的代码及测试
* A:案例演示
	* 模仿学生类，完成手机类代码

### 创建一个对象的步骤
* A:画图演示
	* 画图说明一个对象的创建过程做了哪些事情?
	* Student s = new Student();
	* 1,Student.class加载进内存
	* 2,声明一个Student类型引用s
	* 3,在堆内存创建对象,给对象中属性默认初始化值
	* 4,构造方法进栈,对对象中的属性赋值,构造方法弹栈
	* 5,将对象的地址值赋值给s
![](http://ogtmd8elu.bkt.clouddn.com/201707121323_910.png)

	
### 变量的定义问题
* A:案例演示
	* 需求：定义一个类Demo,其中定义一个求两个数据和的方法，定义一个测试了Test，进行测试。
* B:什么时候定义成员变量
	* a:变量什么时候定义为成员变量：
		* 如果这个变量是用来描述这个类的信息的，那么，该变量就应该定义为成员变量。
	
	* b:变量到底定义在哪里好呢?
		* 变量的范围是越小越好。因为能及时的被回收。

```java
class Demo8 {
	public static void main(String[] args) {
		Demo d = new Demo();
		int num = d.add(30,40);
		System.out.println(num);
	}
}

class Demo {
	int a;
	int b;

	//方式一求和
	/*public int add() {
		int a = 10;
		int b = 20;
		return a + b;
	}*/
	
	//方式二求和
	/*public int add(int a,int b) {
		return a + b;
	}*/

	//方式三求和

	public int add(int a,int b) {
		this.a = a;
		this.b = b;

		return this.a + this.b;
	}

	/*
	成员变量和局部变量
	成员变量所属于对象,要看是否是对象的特性,比如人,人的姓名,年龄等
	局部变量所属与方法,要看方法执行的时候,是否需要,如果需要就定义,好处是方法运行后就消失
	*/
}
```
		
### 长方形案例练习 
* A:案例演示
	* 需求：
		* 定义一个长方形类,定义 求周长和面积的方法，
		* 然后定义一个测试了Test2，进行测试。

```java
class Test1 {
	public static void main(String[] args) {
		RectangleTest rt = new RectangleTest(10,5);
		System.out.println(rt.perimeter());
		System.out.println(rt.area());
	}
}

/*
* A:案例演示
	* 需求：
		* 定义一个长方形类,定义 求周长和面积的方法，
		* 然后定义一个测试了Test2，进行测试。

	矩形(Rectangle)
	长(length)
	宽(width)
	周长(perimeter)
	面积(area)
*/

class RectangleTest {
	private int length;
	private int width;

	public RectangleTest(){}					//空参的构造方法

	public RectangleTest(int length, int width) {
		this.length = length;
		this.width = width;
	}

	public void setLength(int length) {
		this.length = length;
	}

	public void setWidth(int width) {
		this.width = width;
	}

	public int perimeter() {						//求周长
		return 2 * (length + width);
	}

	public int area() {								//求面积
		return length * width;
	}
}
```
		
### 员工类案例练习
* A:案例演示
	* 需求：定义一个员工类Employee
	* 自己分析出几个成员，然后给出成员变量
		* 姓名,工号,工资,职位 
	* 构造方法，
		* 空参和有参的
	* getXxx()setXxx()方法，
	* 以及一个显示所有成员信息的方法。并测试。
		* work 
```java
class EmpolyeeDemo9 {
	public static void main(String[] args) {
		Employee e = new Employee("邦德","007",20000.00);
		e.work();
	}
}
/*
* A:案例演示
	* 需求：定义一个员工类Employee
	* 自己分析出几个成员，然后给出成员变量
		* 姓名,工号,工资
	* 构造方法，
		* 空参和有参的
	* getXxx()setXxx()方法，
	* 以及一个显示所有成员信息的方法。并测试。
		* work 
*/

class Employee {
	private String name;
	private String id;
	private double pay;

	public Employee() {}			//空参的构造方法

	public Employee(String name, String id, double pay) {
		this.name = name;
		this.id = id;
		this.pay = pay;
	}

	public void setName(String name) {
		this.name = name;
	}

	public void setId(String id) {
		this.id = id;
	}

	public void setPay(double pay) {
		this.pay = pay;
	}

	public String getName() {
		return name;
	}

	public String getId() {
		return id;
	}

	public double getPay() {
		return pay;
	}

	public void work() {
		System.out.println("我的姓名是:" + name + ",我的工号是:" + id + "我的薪水是:" + pay);
	}
}
```
### static关键字的引入
* A:案例演示
	* 通过一个案例引入static关键字。
	* 人类：Person。每个人都有国籍，中国。



### static关键字的特点
* A:static关键字的特点
	* a:随着类的加载而加载
	* b:优先于对象存在
	* c:被类的所有对象共享
		* 举例：咱们班级的学生应该共用同一个班级编号。
		* 其实这个特点也是在告诉我们什么时候使用静态?
			* 如果某个成员变量是被所有对象共享的，那么它就应该定义为静态的。
		* 举例：
			* 饮水机(用静态修饰)
			* 水杯(不能用静态修饰)
	* d:可以通过类名调用
		* 其实它本身也可以通过对象名调用。
		* 推荐使用类名调用。
		* 静态修饰的内容一般我们称其为：与类相关的，类成员
* B:案例演示
	* static关键字的特点


```java
class Demo1Static {
	public static void main(String[] args) {
		//Person p1 = new Person();
		//p1.name = "苍老师";
		//p1.country = "日本";					//对象调用静态
		

		//Person p2 = new Person();
		//p2.name = "小泽老师";					//非静态的只能用对象调用
		Person.country = "日本";				//类名调用静态

		//p1.speak();
		//p2.speak();

		System.out.println(Person.country);
	}
}

class Person {
	String name;								//共性用静态,特性用非静态
	static String country;						//国家

	public void speak() {
		System.out.println(name + "," +  country);
	}
}
```

### static的内存图解
* A:画图演示
	* 带有static的内存图
![](http://ogtmd8elu.bkt.clouddn.com/201707121519_142.png)

static 相当于是一块独立的代码块，speak方法必须要用对象才可以调用。解压缩文件和压缩文件（创建对象的过程相当于解压缩的过程）的区别。

### static的注意事项
* A:static的注意事项
	* a:在静态方法中是没有this关键字的
		* 如何理解呢?
			* 静态是随着类的加载而加载，this是随着对象的创建而存在。
			* 静态比对象先存在。
	* b:静态方法只能访问静态的成员变量和静态的成员方法
		* 静态方法：
			* 成员变量：只能访问静态变量
			* 成员方法：只能访问静态成员方法
		* 非静态方法：
			* 成员变量：可以是静态的，也可以是非静态的
			* 成员方法：可是是静态的成员方法，也可以是非静态的成员方法。
		* 简单记：
			* 静态只能访问静态。(归根就底说，因为静态的变量先于对象存在，所以无法访问到后面形成的非静态的成员)
* B:案例演示
	* static的注意事项

```java
class Demo2Static {
	public static void main(String[] args) {
		//Test t = new Test();
		//t.print1();

		Test.print2();
		//method();								//method方法是静态的,直接能调用,是因为系统会默认加上类名.
		Demo2Static.method();
	}

	public static void method() {
		System.out.println();
	}
}

class Test {
	int num1 = 10;
	static int num2 = 20;

	/*public void print1() {					//非静态的成员方法可以访问静态的成员
		System.out.println(num1);
		System.out.println(num2);
	}*/

	public static void print2() {				//静态方法不能访问非静态的成员
		//System.out.println(this.num1);
		System.out.println(num2);				//静态只能访问静态
	}
}
```

	
### 静态变量和成员变量的区别（面试题）
* 静态变量也叫类变量  成员变量也叫对象变量
* A:所属不同
	* 静态变量属于类，所以也称为为类变量
	* 成员变量属于对象，所以也称为实例变量(对象变量)
* B:内存中位置不同
	* 静态变量存储于方法区的静态区
	* 成员变量存储于堆内存
* C:内存出现时间不同
	* 静态变量随着类的加载而加载，随着类的消失而消失（字节码文件）
	* 成员变量随着对象的创建而存在，随着对象的消失而消失
* D:调用不同
	* 静态变量可以通过类名调用，也可以通过对象调用
	* 成员变量只能通过对象名调用

### main方法的格式详细解释
* A:格式
	* public static void main(String[] args) {}
* B:针对格式的解释
	* public 被jvm调用，访问权限足够大。
	* static 被jvm调用，不用创建对象，直接类名访问
	* void被jvm调用，不需要给jvm返回值
	* main 一个通用的名称，虽然不是关键字，但是被jvm识别
	* String[] args 以前用于接收键盘录入的
* C:演示案例
	* 通过args接收键盘例如数据

```java
class Demo3Static {
	public static void main(String[] args) {		//new String[] {11,22,33};
		for (int x = 0;x < args.length ;x++ ) {
			System.out.print(args[x] + " ");
		}
		
	}

	public static void main() {						//这个方法和主方法构成重载	
		System.out.println("1111111111111");
	}
}
```
	
### 工具类中使用静态
* A:制作一个工具类
	* ArrayTool
	* 1,获取最大值
	* 2,数组的遍历
	* 3,数组的反转

```java
/*
	* A:制作一个工具类
	* ArrayTool
	* 1,获取最大值
	* 2,数组的遍历
	* 3,数组的反转

	通过javadoc命令生成说明书
	* @author(提取作者内容)
	* @version(提取版本内容)
	* javadoc -d 指定的文件目录 -author -version ArrayTool.java
	* @param 参数名称//形式参数的变量名称@return 函数运行完返回的数据
	*/

/**
这是一个数组工具类,里面提供了获取最大值,数组反转,数组遍历方法
@author 
@version v1.0
*/
public class ArrayTool {
	/**
	这是一个空参数的构造方法
	*/
	private ArrayTool(){}

	/**
	这是获取数组最大值的方法
	@param arr 传递一个int类型的数组
	@return 返回一个int数
	*/
	public static int getMax(int[] arr) {
		int max = arr[0];						//记录住零角标位置的值

		for (int x = 1;x < arr.length ;x++ ) {	//从数组的第二个位置开始遍历
			if (max < arr[x]) {
				max = arr[x];					//记录住较大的
			}
		}

		return max;
	}
	
	/**
	这是遍历数组的方法
	@param arr 传递一个int类型的数组
	*/
	public static void print(int[] arr) {
		for (int x = 0;x < arr.length ;x++ ) {	//遍历每一个元素
			System.out.print(arr[x] + " ");
		}
	}
	
	/**
	这是将数组反转的方法
	@param arr 传递一个int类型的数组
	*/
	public static void revArray(int[] arr) {
		for (int x = 0;x < arr.length / 2 ;x++ ) {
			int temp = arr[x];
			arr[x] = arr[arr.length-1-x];
			arr[arr.length-1-x] = temp;
		}
	}
}
```
```java
class Demo4Static {
	public static void main(String[] args) {
		//ArrayTool at = new ArrayTool();
		int[] arr = {66,44,33,11,22,55,77};

		//int max = at.getMax(arr);
		//System.out.println(max);

		int max = ArrayTool.getMax(arr);	//当一个类中所有的方法都是静态的,需要再多做一步
		System.out.println(max);			//将构造函数私有,目的是不让其他类创建本类对象
	}
}

```

### 说明书的制作过程(了解)
* A:对工具类加入文档注释
* B:通过javadoc命令生成说明书
	* @author(提取作者内容)
	* @version(提取版本内容)
	* javadoc -d 指定的文件目录 -author -version ArrayTool.java
	* @param 参数名称//形式参数的变量名称@return 函数运行完返回的数据

### 如何使用JDK提供的帮助文档
* A:找到文档，打开文档
* B:点击显示，找到索引，出现输入框
* C:你应该知道你找谁?举例：Scanner
* D:看这个类的结构(需不需要导包)
	* 成员变量	字段
	* 构造方法	构造方法
	* 成员方法	方法
* E:看这个类的说明
* F:看构造方法
* G:看成员方法
* H:然后使用

### 学习Math类的随机数功能
* 打开JDK提供的帮助文档学习
* A:Math类概述
	* 类包含用于执行基本数学运算的方法
* B:Math类特点
	* 由于Math类在java.lang包下，所以不需要导包。
	* 没有构造方法，因为它的成员全部是静态的。
* C:获取随机数的方法
	* public static double random():返回带正号的 double 值，该值大于等于 0.0 且小于 1.0。
* D:我要获取一个1-100之间的随机数，肿么办?
	* int number = (int)(Math.random()*100)+1;

```java
class Demo5Math {
	public static void main(String[] args) {
		//double d = Math.random();		//生成随机数大于等于0.0小于1.0

		//System.out.println(d);

		for (int x = 1;x <= 100 ;x++ ) {
			int num = (int)(Math.random() * 100) + 1;	//在1到100生成随机整数
			System.out.println(num);
		}
	}
}
```
		
### 猜数字小游戏案例
* A:案例演示
	需求：猜数字小游戏(数据在1-100之间)

```java
import java.util.Scanner;
class Demo6GuessNum {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);				//创建键盘录入对象
		int result = (int)(Math.random() * 100) + 1;		//生成的随机数,在1到100范围内
		System.out.println("请输入一个1到100的数字:");

		while(true) {										//不清楚会猜几次,所以用无限循环
			int guessNum = sc.nextInt();					//获取键盘录入的数字
			if (guessNum > result) {
				System.out.println("大啦");
			}else if (guessNum < result) {
				System.out.println("小啦");
			}else {
				System.out.println("中啦");
				break;
			}
		}
	}
}
```