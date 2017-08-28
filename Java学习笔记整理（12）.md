---
title: Java学习笔记整理（12）
date: 2017-07-21 08:00:45
tags: [J2SE]
categories: [J2SE]
top: 12
---

---



<!--more-->

###  Scanner的概述和方法介绍

- A:Scanner的概述
- B:Scanner的构造方法原理
  - Scanner(InputStream source)
  - System类下有一个静态的字段：
    - public static final InputStream in; 标准的输入流，对应着键盘录入。
- C:一般方法
  - hasNextXxx()  判断是否还有下一个输入项,其中Xxx可以是Int,Double等。如果需要判断是否包含下一个字符串，则可以省略Xxx
  - nextXxx()  获取下一个输入项。Xxx的含义和上个方法中的Xxx相同,默认情况下，Scanner使用空格，回车等作为分隔符

```java
package cn.itcast.scanner;

import java.util.Scanner;

public class Demo1Scanner {

	/**
	 * @param args
	 * 当录入的结果的数据类型与接收的数据类型不匹配报出InputMismatchException
	 */
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);		//System.in是标准键盘输入流
		System.out.println("请输入一个整数:");
		if(sc.hasNextInt()) {						//判断是否是int数
			int x = sc.nextInt();					//将录入的结果存储在x中
			System.out.println(x);
		}else {
			System.out.println("输入错误");
		}
		
	}

}

```



###  Scanner获取数据出现的小问题及解决方案

- A:两个常用的方法：
  - public int nextInt():获取一个int类型的值
  - public String nextLine():获取一个String类型的值
- B:案例演示
  - a:先演示获取多个int值，多个String值的情况
  - b:再演示先获取int值，然后获取String值出现问题
  - c:问题解决方案
    - 第一种：先获取一个数值后，在创建一个新的键盘录入对象获取字符串。
    - 第二种：把所有的数据都先按照字符串获取，然后要什么，你就对应的转换为什么。(后面讲)

```java
package cn.itcast.scanner;

import java.util.Scanner;

public class Demo2Scanner {

	/**
	 * @param args
	 * Scanner
	 * nextInt()返回int数
	 * nextLine()返回String字符串
	 * 10 "10"
	 */
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		//键盘录入两个int数
		/*System.out.println("请输入第一个int数");
		int x = sc.nextInt();
		System.out.println("请输入第二个int数");
		int y = sc.nextInt();
		
		System.out.println("x =" + x + ",y = " + y);*/
		
		//键盘录入两个字符串
	/*	System.out.println("请输入第一个字符串");
		String line1 = sc.nextLine();
		System.out.println("请输入第二个字符串");
		String line2 = sc.nextLine();
		System.out.println("line1 =" + line1 + ",line2 = " + line2);*/
		
		//键盘录入一个整数一个字符串
		/*System.out.println("请输入第一个int数");
		int x = sc.nextInt();
		System.out.println("请输入第二个字符串");
		String line2 = sc.nextLine();
		System.out.println("x =" + x + ",line2 = " + line2);*/
		
		//键盘录入一个字符串一个整数
	/*	System.out.println("请输入第一个字符串");
		String line1 = sc.nextLine();
		System.out.println("请输入第二个int数");
		int y = sc.nextInt();
		System.out.println("line1 =" + line1 + ",y = " + y);*/
		
		//next()和nextLine()
		
//		String str1 = sc.next();				//next方法遇到空格就会结束 后续内容没有了
//		System.out.println(str1);
		
		String str2 = sc.nextLine();			//nextLine方法只有遇到回车才结束，空格后加内容不影响。
		System.out.println(str2);
		
	}

}

```

###  

### String类的概述

- A:什么是字符串
  - B:String类的概述
  - 通过JDK提供的API，查看String类的说明
  - 可以看到这样的两句话。
    - a:字符串字面值"abc"也可以看成是一个字符串对象。
    - b:字符串是常量，一旦被赋值，就不能被改变。
    - String s = "abcdef";
    - s.subString(2,4);

```java
package cn.itcast.string;

import cn.itcast.bean.Person;

public class Demo01String {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		/*
		相当于，重新开辟了另一个空间。（一旦被修改不可以在被修改，除非把它变成垃圾）
		 */
		String s = "abc";//变成了垃圾
		s = "a1b2c5";

		/*
		substring()的用法,先用4-2（endIndex-beginIndex）得到截取的位数为两位
		再从beginIndex的游标2开始，实际字符串的第三位开始截串，所以结果为b2
		多看源码注解。
		 */

		System.out.println(s.substring(2,4));			//String类重写了toString方法,返回是该对象本身

		/*
		类似创建P句柄，但是句柄可以通过set,get方法进行修改数据。
		string类型不可以。
		 */

		Person p = new Person("张三", 23);
		p = new Person("李四", 24);
	}

}

```



###  String类的构造方法

- A:常见构造方法
  - public String():空构造
  - public String(byte[] bytes):把字节数组转成字符串
  - public String(byte[] bytes,int index,int length):把字节数组的一部分转成字符串
  - public String(char[] value):把字符数组转成字符串
  - public String(char[] value,int index,int count):把字符数组的一部分转成字符串
  - public String(String original):把字符串常量值转成字符串
  - B:案例演示
  - 演示String类的常见构造方法
  - public int length()：返回此字符串的长度。

```java
package cn.itcast.string;

public class Demo02String {

	/**
	 * @param args
	 * 字符串的构造方法
	 */
	public static void main(String[] args) {
		demo1();
		//demo2();
		//demo3();
		String s = new String("abc");				//返回和参数一模一样的字符串
		System.out.println(s);
	}

	public static void demo3() {
		char[] arr = {'a','b','c'};
		String s1 = new String(arr);				//将字符数组转换为字符串
		System.out.println(s1);
		//String(char[] value, int offset, int count)将char数组转换为String字符串,从offset开始,转换count个
		char[] arr2 = {'a','b','c','d','e','f','g','h','i'};
		String s2 = new String(arr2, 3, 3);
		System.out.println(s2); // def
	}

	public static void demo2() {
		byte[] arr = {97,98,99};
		String s1 = new String(arr);				//解码,将计算机能看懂的变成我们能看懂的
		System.out.println(s1);
		
		byte[] arr2 = {97,98,99,100,101,102};
		// public String(byte[] bytes,int index,int length)将byte数组解码,从index开始,解码length个
		String s2 = new String(arr2, 2, 3);			
		System.out.println(s2); // out : cde
	}

	public static void demo1() {
		String s1 = new String();					//创建一个空的字符串
		System.out.println(s1);
		
		System.out.println("------------------");
		String s2 = "";								//空的字符串
		System.out.println(s2);
		
		String s3 = null;
		System.out.println(s3); 					//打印对象的引用如果是null就返回null
		/*											//如果不是null就返回对象的toString方法
		 * null 和""的区别
		 * ""是一个String类的对象,可以调用String类中所有方法
		 * null是一个空值,不能调用任何方法,因为调用就会报空指针异常
		 */
	}

}

```



###  String类的常见面试题

- 1.判断定义为String类型的s1和s2是否相等
  - String s1 = "abc";
  - String s2 = "abc";
    - System.out.println(s1 == s2); 				
      - System.out.println(s1.equals(s2)); 

![](http://ogtmd8elu.bkt.clouddn.com/201707211010_604.png)

````java
	    String s1 = "abc";						//String s1 = "abc"会进常量池
		String s2 = "abc";						//先看常量池是否有"abc"对象,如果没有创建,如果有直接记录住
		System.out.println(s1 == s2); 			//已有对象"abc"的地址值		 true
		System.out.println(s1.equals(s2)); 		//s1和s2指向的是同一个对象      
````



- 2.下面这句话在内存中创建了几个对象?

  - String s1 = new String("abc");	

    ![](http://ogtmd8elu.bkt.clouddn.com/201707211013_56.png)

    ​

    ```java
    String s1 = new String("abc");			//这句话在内存中创建几个对象
    		//创建了两个对象,一个在常量池里,一个在堆里(是常量池的副本)
    ```

    ​		

- 3.判断定义为String类型的s1和s2是否相等

  - String s1 = new String("abc");			
  - String s2 = "abc";
    - System.out.println(s1 == s2); ?	 false
    - System.out.println(s1.equals(s2)); ?  true

```java
        String s1 = new String("abc") ;			//s1记录的是堆内存的对象地址值			
		String s2 = "abc";						//s2记录的是常量池的对象地址值
		System.out.println(s1 == s2);	  //false
		System.out.println(s1.equals(s2)); //true
```

​    

- 4.判断定义为String类型的s1和s2是否相等
  - String s1 = "a" + "b" + "c";
  - String s2 = "abc";
    - System.out.println(s1 == s2);   	
    - System.out.println(s1.equals(s2)); ?

```java
String s1 = "a" + "b" + "c";    //java有常量优化机制,在编译时已经是"abc"字符串了
String s2 = "abc";
System.out.println(s1 == s2);   //true
System.out.println(s1.equals(s2));	//true		
```

- 5.判断定义为String类型的s1和s2是否相等

  - String s1 = "ab";
  - String s2 = "abc";
  - String s3 = s1 + "c";
  - System.out.println(s3 == s2);
  - System.out.println(s3.equals(s2)); 

  ![](http://ogtmd8elu.bkt.clouddn.com/201707211023_401.png)

  ```java
  String s1 = "ab";						//在常量池中创建"ab"
  String s2 = "abc";						//在常量池中创建"abc"
  String s3 = s1 + "c";					//当字符串与对象用+连接的时候,底层会调用StringBuffer的append方法
  System.out.println(s2 == s3);			//对字符串进行添加,然后将StringBuffer对象转换为String对象  flase
  System.out.println(s3.equals(s2)); 		//并赋值给s3,s3记录的是堆内存对象的地址值                  true
  ```

  ​

###  String类的判断功能

- A:String类的判断功能
  - boolean equals(Object obj):比较字符串的内容是否相同,区分大小写
  - boolean equalsIgnoreCase(String str):比较字符串的内容是否相同,忽略大小写
  - boolean contains(String str):判断是否包含传入的字符串
  - boolean startsWith(String str):判断字符串是否以某个指定的字符串开头
  - boolean endsWith(String str):判断字符串是否以某个指定的字符串结尾
  - boolean isEmpty():判断字符串是否为空。

```java
        String s1 = "abc";
		String s2 = "abc";
		String s3 = "AbC";
		System.out.println(s1.equals(s3));	 		//相同字符序列返回true
		System.out.println(s1.equalsIgnoreCase(s3));//比较字符串的内容是否相同,忽略大小写
```

```java
        String s1 = "javaitcastjava";
		String s2 = "itcast";
		String s3 = "heima";
		
		System.out.println(s1.contains(s2));		//包含传入的字符串返回true
		System.out.println(s1.contains(s3));		//不包含返回false
```

```java
	    String s1 = "aaaitcastbbbitcastccc";
		String s2 = "aaa";
		String s3 = "ccc";
		
		System.out.println(s1.startsWith(s2));		//判断字符串是否以某个指定的字符串开头 true
		System.out.println(s1.startsWith(s3));                                    //    flase
		System.out.println("-------------------------");
		System.out.println(s1.endsWith(s2));		//判断字符串是否以某个指定的字符串结尾 flase
		System.out.println(s1.endsWith(s3));                                       //true
```

```java
        String s1 = "";
		String s2 = "itcast";
		String s3 = null;
		
		System.out.println(s1.isEmpty()); //true
		System.out.println(s2.isEmpty());  //false
		//System.out.println(s3.isEmpty());			//空指针异常
```



###  模拟用户登录

- A:案例演示
  - 需求：模拟登录,给三次机会,并提示还有几次。
  - 用户名和密码都是admin

```java
package cn.itcast.test;

import java.util.Scanner;

public class Test1String {

	/**
	 * @param args
	 * 需求：模拟登录,给三次机会,并提示还有几次。
	 * 1,键盘录入用户名和密码(需要Scanner中nextLine()方法)
	 * 2,需要录入三次(需要for循环语句)
	 * 3,需要判断用户密码是否正确(需要if,else语句如果正确跳出循环,如果不正确提示有几次)
	 */
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);			//创建键盘录入对象
		
		for(int i = 0; i < 3; i++) {					//需要三次机会
			System.out.println("请输入用户名:");
			String user = sc.nextLine();					//将键盘录入的字符串用户名存储在user里
			System.out.println("请输入密码:");
			String password = sc.nextLine();				//将键盘录入的字符串密码存储在password里
			if("admin".equals(user) && "admin".equals(password)) {//如果用户名和密码都正确
				System.out.println("欢迎" + user + "登录");
				break;									//跳出循环
			}else {
				if(i == 2) {
					System.out.println("你的错误次数已到,请明天再来");
				}else {
					System.out.println("您的用户名或密码错误,还有" + (2 - i) + "次机会");
				}
			}
		}
	}

}

```



###  String类的获取功能

- A:String类的获取功能
  - int length():获取字符串的长度。
  - char charAt(int index):获取指定索引位置的字符
  - int indexOf(int ch):返回指定字符在此字符串中第一次出现处的索引。
  - int indexOf(String str):返回指定字符串在此字符串中第一次出现处的索引。
  - int indexOf(int ch,int fromIndex):返回指定字符在此字符串中从指定位置后第一次出现处的索引。
  - int indexOf(String str,int fromIndex):返回指定字符串在此字符串中从指定位置后第一次出现处的索引。
  - lastIndexOf
  - String substring(int start):从指定位置开始截取字符串,默认到末尾。
  - String substring(int start,int end):从指定位置开始到指定位置结束截取字符串。（包含头不包含尾）

```java
package cn.itcast.string;

public class Demo05_Method2 {

	/**
	 * String类的获取功能
	* int length():获取字符串的长度。
	* char charAt(int index):获取指定索引位置的字符
	* int indexOf(int ch):返回指定字符在此字符串中第一次出现处的索引。
	* int indexOf(String str):返回指定字符串在此字符串中第一次出现处的索引。
	* int indexOf(int ch,int fromIndex):返回指定字符在此字符串中从指定位置后第一次出现处的索引。
	* int indexOf(String str,int fromIndex):返回指定字符串在此字符串中从指定位置后第一次出现处的索引。
	* lastIndexOf
	* String substring(int start):从指定位置开始截取字符串,默认到末尾。
	* String substring(int start,int end):从指定位置开始到指定位置结束截取字符串。
	 */
	public static void main(String[] args) {
		//demo1();
		//demo2();
		//demo3();
		//demo4();
		String s1 = "abcitcastabc";
		//String s2 = s1.substring(3);		//从指定位置开始截取字符串,默认到末尾。产生新的字符串
		
		//System.out.println(s2);
		String s3 = s1.substring(3, s1.length());//包含头,不包含尾(左闭右开)
		System.out.println(s3);
	}

	public static void demo4() {
		String s1 = "abcitcastabc";
		int index1 = s1.lastIndexOf('a');//从后向前找,将字符对应的索引返回
		//System.out.println(index1);
		
		int index2 = s1.lastIndexOf("ab");
		//System.out.println(index2);
		
		int index3 = s1.lastIndexOf('a', 8);	//从指定的位置向前找,第一次字符出现的索引
		System.out.println(index3);
	}

	public static void demo3() {
		String s1 = "abcabcitcast";
		int index1 = s1.indexOf('c', 3);//返回指定字符在此字符串中从指定位置后第一次出现处的索引。
		//System.out.println(index1);
		
		int index2 = s1.indexOf("ca", 4);
		System.out.println(index2);
	}

	public static void demo2() {
		String s1 = "abcdefc";
		int index1 = s1.indexOf('c');	//返回指定字符在此字符串中第一次出现处的索引。
		//System.out.println(index1);
		
		int index2 = s1.indexOf("ce");	//如果查找的字符串没有,返回-1
		System.out.println(index2);
	}

	public static void demo1() {
		String s1 = "abc";
		String s2 = "我是中国人";
		
		System.out.println(s1.length());			//获取字符串中字符的个数
		System.out.println(s2.length());       //长度是5 空格也算一个字符
		
		char c1 = s1.charAt(1);						//通过索引获取字符
		System.out.println(c1);
		char c2 = s2.charAt(3);
		System.out.println(c2);
		
		
	}

}

```



###  字符串的遍历

- A:案例演示
  - 需求：遍历字符串

```java
for(int i = 0; i < s2.length(); i++) {		//字符串的遍历方式
			System.out.println(s2.charAt(i));
		}

```



###  统计不同类型字符个数

- A:案例演示
  - 需求：统计一个字符串中大写字母字符，小写字母字符，数字字符出现的次数,其他字符出现的次数。

```java
package cn.itcast.test;

public class Test2String {

	/**
	 * @param args
	 * 需求：统计一个字符串中大写字母字符，小写字母字符，数字字符出现的次数,其他字符出现的次数。
	 */
	public static void main(String[] args) {
		String s = "Aabc4deBCD123567!@#$%^";
		int big = 0;						//记录大小字母字符的个数
		int small = 0;						//记录小写字母字符的个数
		int num = 0;						//记录数字字符的个数
		int other = 0;						//记录其他字符的个数
		
		for(int i = 0; i < s.length(); i++) {//对字符串中所有的字符遍历
			char temp = s.charAt(i);		//记录住字符串中的每一个字符
			if(temp >= 'A' && temp <= 'Z') {
				big++;
			}else if(temp >= 'a' && temp <= 'z') {
				small++;
			}else if(temp >= '0' && temp <= '9') {
				num++;
			}else {
				other++;
			}
		}
		
		System.out.println("字符s中有大写字母" + big + "个,有小写字母" + small + "个,有数字" + 
				num + "个,其他字符" + other + "个");
	}

}

```





###  String类的转换功能

- A:String的转换功能：
  - byte[] getBytes():把字符串转换为字节数组。
  - char[] toCharArray():把字符串转换为字符数组。
  - static String valueOf(char[] chs):把字符数组转成字符串。
  - static String valueOf(int i):把int类型的数据转成字符串。
    - 注意：String类的valueOf方法可以把任意类型的数据转成字符串。

```java
  * String toLowerCase():把字符串转成小写。
  * String toUpperCase():把字符串转成大写。
  * String concat(String str):把字符串拼接。
```



```java
package cn.itcast.string;

import java.io.UnsupportedEncodingException;

public class Demo06_Method3 {

	/**
	 * A:String的转换功能：
	* byte[] getBytes():把字符串转换为字节数组。
	* char[] toCharArray():把字符串转换为字符数组。
	* static String valueOf(char[] chs):把字符数组转成字符串。
	* static String valueOf(int i):把int类型的数据转成字符串。
		* 注意：String类的valueOf方法可以把任意类型的数据转成字符串。
	* String toLowerCase():把字符串转成小写。
	* String toUpperCase():把字符串转成大写。
	* String concat(String str):把字符串拼接。
	 * @throws UnsupportedEncodingException 
	
	 */
	public static void main(String[] args) throws UnsupportedEncodingException {
//		demo1();
//		demo2();
//		demo3();
		demo4(); // 抽取方法的快捷键 ctrl + alt + M
	}

	private static void demo4() {
		String s1 = "abcDEFgh";
		System.out.println(s1.toLowerCase());		//将所有的字符转换成小写
		System.out.println(s1.toUpperCase());		//将所有的字符转换成大写
		System.out.println(s1);

		String s2 = "abc";
		String s3 = "def";
		String s4 = s2.concat(s3);					//将两个字符串连接
		System.out.println(s4);
	}

	public static void demo3() {
		char[] arr = {'1','2','3'};
		String s1 = String.valueOf(arr);			//将字符数组转换成对应的字符串
		System.out.println(s1);
		
		String s2 = String.valueOf(10);
		System.out.println(s2);
		
		System.out.println(10 + "");	 			//也可以将10转换成对应的字符串
	}

	private static void demo2() {
		String s = "itcast加油";
		char[] arr = s.toCharArray();				//将字符串转换为字符数组
		for (char anArr : arr) {
			System.out.print(anArr + " ");
		}


	}

	public static void demo1() throws UnsupportedEncodingException {
		String s1 = "abc";
		/*byte[] arr = s1.getBytes();					//编码,将我们看的懂的,变成计算机看的懂的
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + " ");
		}*/
		
		String s2 = "你好你好"; // GBK 一个中文占两个字节 4个占八个字节，
//		byte[] arr2 = s2.getBytes("uTf-8");
		byte[] arr2 = s2.getBytes("GBK");
		for (byte anArr2 : arr2) {
			System.out.print(anArr2 + " ");
		}
		//用什么码表编就用什么码表解
		//-60 -29 -70 -61 -60 -29 -70 -61 通过String的构造函数解码
		//-28 -67 -96 -27 -91 -67 -28 -67 -96 -27 -91 -67 utf-8编码
		byte[] arr3 = {-28,-67,-96,-27,-91,-67,-28,-67,-96,-27,-91,-67};
		String s3 = new String(arr3,"uTF-8");//Utf-8 一个中文占三个字节。


		byte[] arr4 = {-60 ,-29 ,-70 ,-61, -60, -29 ,-70 ,-61};
		String s4 = new String(arr4,"GBK");



		System.out.println(s3);
		System.out.println(s4);
	}

}

```





###  按要求转换字符

- A:案例演示
  - 需求：把一个字符串的首字母转成大写，其余为小写。(只考虑英文大小写字母字符)

```java
package cn.itcast.test;

public class Test3String {

	/**
	 * @param args
	 * 需求：把一个字符串的首字母转成大写，其余为小写。(只考虑英文大小写字母字符)
	 */
	public static void main(String[] args) {
		String s = "abcItcastHeima";
		System.out.println(s.substring(0, 1).toUpperCase().concat(s.substring(1).toLowerCase()));	//链式编程
	}

}

```



###  把数组转成字符串

- A:案例演示

  - 需求：把数组中的数据按照指定的格式拼接成一个字符串
    - 举例：
      - int[] arr = {1,2,3};	
    - 输出结果：
      - "[1, 2, 3]"

  ```java
  package cn.itcast.test;

  public class Test4String {

  	/**
  	 * 需求：把数组中的数据按照指定个格式拼接成一个字符串
  		* 举例：
  			* int[] arr = {1,2,3};	
  		* 输出结果：
  			* "[1, 2, 3]"
  	 */
  	public static void main(String[] args) {
  		int[] arr = {1,2,3};
  		
  		String temp = "[";
  		for (int i = 0; i < arr.length; i++) {		//遍历数组,获取每一个元素
  			//temp = temp + arr[i] + ", ";			//[1, 2, 3]
  			/*if(i == arr.length -1) {
  				temp = temp + arr[i] + "]";			//[1, 2, 3]
  			}else {
  				temp = temp + arr[i] + ", ";		//[1, 2, 
  			}*/
  			temp = temp + ((i == arr.length -1) ?  arr[i] + "]" : arr[i] + ", ");
  			
  		}
  		
  		System.out.println(temp);
  	}

  }

  ```

  ​

###  String类的其他功能

- A:String的替换功能及案例演示
  - String replace(char old,char new)
  - String replace(String old,String new)
- B:String的去除字符串两空格及案例演示
  - String trim()
- C:String的按字典顺序比较两个字符串及案例演示
  - int compareTo(String str)(暂时不用掌握)
  - int compareToIgnoreCase(String str)

```java
package cn.itcast.string;

public class Demo07_Method4 {

	/**
	 * * A:String的替换功能及案例演示
	* String replace(char old,char new)
	* String replace(String old,String new)
	* B:String的去除字符串两空格及案例演示
		* String trim()
	* C:String的按字典顺序比较两个字符串及案例演示
		* int compareTo(String str)
	* int compareToIgnoreCase(String str)
	* 
	* itcast 
	* itcast
	 */
	public static void main(String[] args) {
		//demo1();
//		demo2();
		//demo3();
//		demo4();
	}

	private static void demo4() {
		String s1 = "A";
		String s2 = "a";
		System.out.println(s1.compareToIgnoreCase(s2));	//比较的时候不区分大小写
	}

	public static void demo3() {
		String s1 = "a";
		String s2 = "aaaa";					//如果两个字符串中的字符一样,个数不一样比较长度
		int x = s1.compareTo(s2);			//按照码表值比较两个字符串的大小
		System.out.println(x);
	}

	public static void demo2() {
		String s = "    itcast    ";
		String s2 = s.trim();					//去除前后空格
		System.out.println(s2);
	}

	public static void demo1() {
		String s = "itcast";
		String s2 = s.replace('z', 'o');		//替换,将已有字符替换,如果没有被替换的字符,打印原字符串
		System.out.println(s2);
		
		String s3 = s.replace("cas", "ooo");
		System.out.println(s3);
	}

}

```





###  字符串反转

- A:案例演示
  - 需求：把字符串反转
    - 举例：键盘录入"abc"		
    - 输出结果："cba"

```java
package cn.itcast.test;

import java.util.Scanner;

public class Test5String {

	/**
	 * @param args
	 * * A:案例演示
	* 需求：把字符串反转
		* 举例：键盘录入"abc"		
		* 输出结果："cba"
		* 1,键盘录入字符串(Scanner)
		* 2,将字符串转换成字符串数组(String toCharArray())
	 */
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		System.out.println("请数一个字符串:");
		String line = sc.nextLine();			//将键盘录入的字符串存储在line中
		char[] arr = line.toCharArray();		//将字符串转换成字符数组
		//line = abc
		//"" + cba
		String temp = "";
		for(int i = arr.length - 1; i >= 0; i--) {//倒着遍历字符数组
			temp = temp + arr[i];				//将每一个字符与字符串连接
		}
		
		System.out.println(temp);
		
	}

}

```



### 在大串中查找小串出现的次数思路图解

- A:画图演示
  - 需求：统计大串中小串出现的次数
  - 这里的大串和小串可以自己根据情况给出

### 在大串中查找小串出现的次数代码实现

- A:案例演示	
  - 统计大串中小串出现的次数


- ​