---
title: Java学习笔记整理（19）
date: 2017-08-04 13:18:18
tags: [异常,异常处理]
categories: J2SE
top: 19
---

---

1. 异常类的使用
2. try..catch finally语句的使用
3. RuntimeException(运行时异常）
4. 编译时异常
5. throw 和throws的区别（面试题）
6. final,finally和finalize的区别（面试题）
7. 自定义异常的使用
8. File的使用（创建，删除，重命名）
9. File的遍历读取
10. File类的相对路径（当前项目或者是当前目录）和绝对路径（盘符开始）
11. 文件过滤器的使用
12. 递归

<!--more-->

### 异常的概述和分类

- A:异常的概述

  - 异常就是Java程序在运行过程中出现的错误。

- B:异常的分类

  - 通过API查看Throwable
  - Error
    - 服务器宕机,数据库崩溃等
  - Exception
    C:异常的继承体系
  - Throwable
    - Error	
    - Exception
      - RuntimeException（运行时异常）

  ​

###  JVM默认是如何处理异常的

- A:JVM默认是如何处理异常的
  - main函数收到这个问题时,有两种处理方式:
  - a:自己将该问题处理,然后继续运行
  - b:自己没有针对的处理方式,只有交给调用main的jvm来处理
  - jvm有一个默认的异常处理机制,就将该异常进行处理.
  - 并将该异常的名称,异常的信息.异常出现的位置打印在了控制台上,同时将程序停止运行
- B:案例演示
  - JVM默认如何处理异常



###  try...catch的方式处理异常1

- A:异常处理的两种方式
  - a:try…catch…finally
  - b:throws
- B:try...catch处理异常的基本格式
  - try…catch…finally
- C:案例演示
  - try...catch的方式处理1个异常



```java
package cn.itcast.exception;

public class Demo1_Exception {

	/**
	 * @param args
	 * * A:异常处理的两种方式
		* a:try…catch…finally
			* try是用来检测异常的
			* catch是用来捕获异常的
			* finally释放资源 
		* b:throws
			* 抛出去 
		世界是最真情的相依,是你在try我在catch,无论你发神马脾气,我都默默接受,静静处理
	 */
	public static void main(String[] args) {
		Demo d = new Demo();
		try {
			int x = d.div(10, 0);
			System.out.println(x);
		} catch (ArithmeticException e) {//ArithmeticException e = new ArithmeticException(" / by zero")
			System.out.println("除数为零了");
		}
    }

}

class Demo {
	public int div(int a,int b) {//a = 10,b = 0
		return a / b;			//除数不能为0
								//除数为0后,创建一个异常对象new ArithmeticException(" / by zero")
	}
}

```







###  try...catch的方式处理异常2

- A:案例演示
  - try...catch的方式处理多个异常

```java
public static void demo1() {
		int a = 10;
		int b = 0;
		int[] arr = {11,22,33};
		
		try {
			//System.out.println(a / b);
			System.out.println(arr[5]);
			arr = null;
			System.out.println(arr[0]);
		} catch (ArithmeticException e) {
			System.out.println("除数为零了");
		} catch (ArrayIndexOutOfBoundsException  e) {
			System.out.println("出问题了");
		}
		
		System.out.println("over");
	}
```





### JDK7针对多个异常的处理方案

- A:案例演示
  - JDK7以后处理多个异常的方式及注意事项



```java
/**
	 * @param args
	 * 安卓都是try{}catch(Exception e){}客户端
	 * EE都是从底层往上抛					服务端
     * 范围大的异常要往会面放，否则报错。
     *
	 */
	public static void main(String[] args) {
		//demo1();
		int a = 10;
		int b = 0;
		int[] arr = {11,22,33};
		
		try {
			//System.out.println(a / b);
			System.out.println(arr[5]);

		} catch (ArithmeticException | ArrayIndexOutOfBoundsException e) {//1.7版本的异常处理,注意不能子父类同时放
			System.out.println("除数为零了");
		} catch (Exception e3) {
			System.out.println("出问题了");
		}
		
		System.out.println("over");
	}
```





### 编译期异常和运行期异常的区别

- A:编译期异常和运行期异常的区别
  - Java中的异常被分为两大类：编译时异常和运行时异常。
  - 所有的RuntimeException类及其子类的实例被称为运行时异常，其他的异常就是编译时异常
  - 编译时异常
    - Java程序必须显示处理，否则程序就会发生错误，无法通过编译
  - 运行时异常
    - 无需显示处理，也可以和编译时异常一样处理
- B:案例演示
  - 编译期异常和运行期异常的区别



```java
package cn.itcast.exception;

import java.io.FileInputStream;
import java.io.FileNotFoundException;

public class Demo3_Exception {

	/**
	 * @param args
	 * 编译时异常
	 * 		未雨绸缪
	 * 		在准备要做某件事的时候,要提前想到可能会发生什么事
	 * 运行时异常
	 * 		编译时不会报错,程序运行起来如果有错误就会报异常,运行时异常都是程序员犯的错误,需要回来修改代码
	 * @throws FileNotFoundException 
	 */
	public static void main(String[] args) throws FileNotFoundException {
		/*int[] arr = {11,22,33};
		//System.out.println(arr[4]);

		System.out.println(arr[0]);*/
		FileInputStream fis = new FileInputStream("F:\\config.txt");		//编译时异常
		
	}

}

```



### Throwable的几个常见方法

- A:Throwable的几个常见方法
  - a:getMessage()
    - 获取异常信息，返回字符串。
  - b:toString()
    - 获取异常类名和异常信息，返回字符串。
  - c:printStackTrace()
    - 获取异常类名和异常信息，以及异常出现在程序中的位置。返回值void。
- B:案例演示
  - Throwable的几个常见方法的基本使用



```java
package cn.itcast.exception;

public class Demo4_Exception {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		try {
			System.out.println(1/0);		//new ArithmeticException("/ by zero");
		} catch (ArithmeticException e) {
			System.out.println(e.getMessage()); 		//获取异常信息
            System.out.println(e.toString()); 			//错误类名+异常信息
            e.printStackTrace();						//错误类名+异常信息+错误位置(行号)jvm默认处理异常使用此方法

        }
	}

}

```



### throws的方式处理异常

- A:throws的方式处理异常
  - 定义功能方法时，需要把出现的问题暴露出来让调用者去处理。
  - 那么就通过throws在方法上标识。
- B:案例演示
  - 举例分别演示编译时异常和运行时异常的抛出

```java
package cn.itcast.exception;

public class Demo5_Exception {

	/**
	 * @param args
	 * throws如果抛出的是RuntimeException及其子类,调用该方法的时候不用处理,抛出RutimeException和不抛效果一样
	 * 如果抛出的是除了RuntimeException及其子类.调用方法必须处理
	 * @throws Exception 
	 */
	public static void main(String[] args) throws Exception {
		Test t = new Test();
		int x = t.div(10, 0);
		System.out.println(x);
	}

}

class Test {
	public int div(int a,int b) throws Exception {
		return a / b;
	}
}
```





### throw的概述以及和throws的区别

- A:throw的概述
  - 在功能方法内部出现某种情况，程序不能继续运行，需要进行跳转时，就用throw把异常对象抛出。
  - B:案例演示
  - 分别演示编译时异常对象和运行时异常对象的抛出
- C:throws和throw的区别
  - a:throws
    - 用在方法声明后面，跟的是异常类名
    - 可以跟多个异常类名，用逗号隔开
    - 表示抛出异常，由该方法的调用者来处理
  - b:throw
    - 用在方法体内，跟的是异常对象名
    - 只能抛出一个异常对象名
    - 表示抛出异常，由方法体内的语句处理



```java
package cn.itcast.exception;

import cn.itcast.bean.Person;

public class Demo6_Exception {

	/**
	 * @param args
	 * @throws Exception 
	 */
	public static void main(String[] args) throws Exception {
		Person p = new Person();
		p.setAge(-18);
		p.setName("张三");
		
		System.out.println(p.getName() + "," + p.getAge());
	}

}

```



```java
package cn.itcast.bean;

public class Person {
	private String name;
	private int age;
	public Person() {
		super();
		
	}
	public Person(String name, int age) {
		super();
		this.name = name;
		this.age = age;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) throws AgeOutOfBoundsException  {
		if(age > 0 && age < 200) {
			this.age = age;
		}else {
			//System.out.println("年龄错误");
			//throw new Exception("年龄错误");
			throw new AgeOutOfBoundsException("年龄非法");
		}
	}
	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}
	
	
}

```



```java
package cn.itcast.bean;

public class AgeOutOfBoundsException extends Exception {
	public AgeOutOfBoundsException (){
		super();
	}
	
	public AgeOutOfBoundsException (String message){
		super(message);
	}
}

```



### finally关键字的特点及作用

- A:finally的特点
  - 被finally控制的语句体一定会执行
  - 特殊情况：在执行到finally之前jvm退出了(比如System.exit(0))
- B:finally的作用
  - 用于释放资源，在IO流操作和数据库操作中会见到
- C:案例演示
  - finally关键字的特点及作用



```java
package cn.itcast.exception;

public class Demo7_Finally {

	/**
	 * @param args
	 * finally最终的
	 * 释放资源,一定会执行的代码
	 * try catch finally
	 * 1,try catch				不需要释放资源
	 * 2,try catch finally		需要释放资源
	 * 3,try finally			需要释放资源,但是不能对象异常捕获,需要交给调用处理
	 */
	public static void main(String[] args) {
		try {
			System.out.println(1/0);
		} catch (Exception e) {
			System.out.println("catch执行了吗");
			//System.exit(0);	//如果先退出虚拟机就不会执行finally了
			return;				//return在将死之前,会让finally完成未完成的遗愿
		} finally {
			System.out.println("finally执行了吗");
		}

		
	}

}

```







###  finally关键字的面试题

- A:面试题1
  - final,finally和finalize的区别


- B:面试题2
  - 如果catch里面有return语句，请问finally的代码还会执行吗?如果会，请问是在return前还是return后。

```java
package cn.itcast.exception;

public class Demo8_Finally {

	/**
	  * A:面试题1
	  * final,finally和finalize的区别
     *
     * final:
	  * final可以修饰类,方法,变量(常量)
     * a.修饰类的时候，该类不可被继承。
     * b.修饰方法不能被重写
     * c.修饰变量的时候，变成常量，不可更改。
     * d.修饰引用类型的时候，类型的地址值不可更改，但属性可以更改
     * e.修饰基本类型的数据时，值不可被改变。
     *
	  * finally写在try catch语句用于释放资源(关闭数据库,关闭流)
	  *
     *
     * finalize()是一个方法,在垃圾回收的时候调用
	  * B:面试题2
	  * 如果catch里面有return语句，请问finally的代码还会执行吗?如果会，请问是在return前还是return后。
     *
     * 会执行，return前执行
     *
	 */
	public static void main(String[] args) {
		System.out.println(getNum());
	}

	public static int getNum() {
		int x = 10;
            try {
			System.out.println(10 / 0);
			return x;						//try和catch里面都应该有return语句,如果try没有检测到问题
		} catch (Exception e) {				//不会跳转到catch里面去,那么需要try里面有返回
			x = 20;
			return x;
		}finally {
			x = 30;
			System.out.println("执行了吗");
//			return x;						//finally可以写return语句,但是不要这么写,因为会将结果改变
		}									//将前面的返回值覆盖掉
	}										//finally的作用就是为了释放资源
}
```

### 自定义异常概述和基本使用

- A:为什么需要自定义异常
  - 举例：人的年龄
- B:自定义异常概述
  - 继承自Exception
  - 继承自RuntimeException
- C:案例演示
  - 自定义异常的基本使用

```java
package cn.itcast.bean;

public class Person {
	private String name;
	private int age;
	public Person() {
		super();
		
	}
	public Person(String name, int age) {
		super();
		this.name = name;
		this.age = age;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) throws AgeOutOfBoundsException  {
		if(age > 0 && age < 200) {
			this.age = age;
		}else {
			//System.out.println("年龄错误");
			//throw new Exception("年龄错误");
			throw new AgeOutOfBoundsException("年龄非法");
		}
	}
	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}
	
	
}

```





```java
package cn.itcast.bean;

public class AgeOutOfBoundsException extends Exception {
    public AgeOutOfBoundsException (){
        super();
    }

    public AgeOutOfBoundsException (String message){
        super(message);
    }
}

```

```java
public static void main(String[] args) throws AgeOutOfBoundsException {
		Person p = new Person();
		p.setAge(-18);
		p.setName("张三");
		
		System.out.println(p.getName() + "," + p.getAge());
	}
```

### 异常的注意事项及如何使用异常处理

- A:异常注意事项
  - a:子类重写父类方法时，子类的方法必须抛出相同的异常或父类异常的子类。(父亲坏了,儿子不能比父亲更坏)
  - b:如果父类抛出了多个异常,子类重写父类时,只能抛出相同的异常或者是他的子集,子类不能抛出父类没有的异常
  - c:如果被重写的方法没有异常抛出,那么子类的方法绝对不可以抛出异常,如果子类方法内有异常发生,那么子类只能try,不能throws
- B:如何使用异常处理
  - 原则:如果该功能内部可以将问题处理,用try,如果处理不了,交由调用者处理,这是用throws
  - 区别:
    - 后续程序需要继续运行就try
    - 后续程序不需要继续运行就throws
  - 如果JDK没有提供对应的异常，需要自定义异常。



```java
class Fu {
	public void print() throws Exception {
		System.out.println("Fu");
	}
}

class Zi extends Fu  {
	public void print()throws FileNotFoundException{
		
		System.out.println("Zi");
	} 
```



### 异常练习

- 键盘录入一个int类型的整数,对其求二进制表现形式
  - 如果录入的整数过大,给予提示,录入的整数过大请重新录入一个整数BigInteger
  - 如果录入的是小数,给予提示,录入的是小数,请重新录入一个整数
  - 如果录入的是其他字符,给予提示,录入的是非法字符,请重新录入一个整数


```java
package cn.itcast.test;

import java.math.BigDecimal;
import java.math.BigInteger;
import java.util.Scanner;

public class Test3 {

	/**
	 * 键盘录入一个int类型的整数,对其求二进制表现形式
	 * 如果录入的整数过大,给予提示,录入的整数过大请重新录入一个整数BigInteger
	 * 如果录入的是小数,给予提示,录入的是小数,请重新录入一个整数
	 * 如果录入的是其他字符,给予提示,录入的是非法字符,请重新录入一个整数
	 */
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);					//创建键盘录入对象
		System.out.println("请输入一个整数");
		while(true) {
			String line = sc.nextLine();							//将键盘录入的结果存储在line中
			try {
				int num = Integer.parseInt(line);					//将数字字符串转成数字
				System.out.println(Integer.toBinaryString(num));	//将数字转换成二进制打印
				break;
			} catch (Exception e) {
				try {
					new BigInteger(line);							//如果能封装到BigInteger对象中说明是一个过大的整数
					System.out.println("录入的整数过大请重新录入一个整数:");
				} catch (Exception e2) {
					try {
						new BigDecimal(line);						//如果能封装到BigDecimal对象中说明是一个小数
						System.out.println("录入的是小数,请重新录入一个整数");
					} catch (Exception e3) {
						System.out.println("录入的是非法字符,请重新录入一个整数");
					}
				}
				
			}
		}
	}

}

```






### File类的概述和构造方法

- A:File类的概述
  - File更应该叫做一个路径
    - 文件路径或者文件夹路径  
    - 路径分为绝对路径和相对路径
    - 绝对路径是一个固定的路径,从盘符开始
    - 相对路径相对于某个位置,在eclipse下是指当前项目下,在dos下指的是当前路径
  - 查看API
  - 文件和目录路径名的抽象表示形式
- B:构造方法
  - File(String pathname)：根据一个路径得到File对象
  - File(String parent, String child):根据一个目录和一个子文件/目录得到File对象
  - File(File parent, String child):根据一个父File对象和一个子文件/目录得到File对象
- C:案例演示
  - File类的构造方法

```java
package cn.itcast.file;

import java.io.File;

public class Demo1_File {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		//demo1();
		//demo2();
		File parent = new File("F:\\2015\\0302");					//父级路径的文件对象
		String child = "day19异常+File";								//子级路径
		File file = new File(parent, child);
		System.out.println(file.exists());
	}

	public static void demo2() {
		String parent = "F:\\2015\\0302";							//父级路径
		String child = "day19异常+File";								//子级路径
		File file = new File(parent, child);
		System.out.println(file.exists());
	}

	public static void demo1() {
		File file1 = new File("F:\\2015\\0302\\day19异常+File");		//绝对路径
		File file2 = new File("xxx.txt");							//相对路径,相对于当前项目下
		
		boolean b1 = file1.exists();								//判断是否存在
		System.out.println(b1);
		boolean b2 = file2.exists();
		System.out.println(b2);
	}

}
```

### File类的创建功能

- A:创建功能
  - public boolean createNewFile():创建文件 如果存在这样的文件，就不创建了
  - public boolean mkdir():创建文件夹 如果存在这样的文件夹，就不创建了
  - public boolean mkdirs():创建文件夹,如果父文件夹不存在，会帮你创建出来
- B:案例演示
  - File类的创建功能
  - 注意事项：
    - 如果你创建文件或者文件夹忘了写盘符路径，那么，默认在项目路径下。

```java
package cn.itcast.file;

import java.io.File;
import java.io.IOException;


public class Demo2_File {

	/**
	 * * A:创建功能
	* public boolean createNewFile():创建文件 如果存在这样的文件，就不创建了
	* public boolean mkdir():创建文件夹 如果存在这样的文件夹，就不创建了
	* public boolean mkdirs():创建文件夹,如果父文件夹不存在，会帮你创建出来
	* B:案例演示
		* File类的创建功能
		* 注意事项：
			* 如果你创建文件或者文件夹忘了写盘符路径，那么，默认在项目路径下。
	 * @throws IOException 
	 */
	public static void main(String[] args) throws IOException {
		//demo1();
		File dir1 = new File("aaa");
		boolean b1 = dir1.mkdir();
		System.out.println(b1);
		
		File dir2 = new File("bbb.txt");
		boolean b2 = dir2.mkdir();					//创建单级目录
		System.out.println(b2);
		
		File dir3 = new File("ccc\\ddd");
		boolean b3 = dir3.mkdirs();					//创建多级目录 使用的是mkdirs
		System.out.println(b3);
	}

	public static void demo1() throws IOException {
		File file = new File("yyy.txt");
		boolean b1 = file.createNewFile();					//创建新文件
		System.out.println(b1);
		System.out.println(file.exists());
		
		File file2 = new File("zzz");
		boolean b2 = file2.createNewFile();					//创建一个新文件,如果文件不存在就创建返回true
		System.out.println(b2);								//如果文件存在,就不创建,返回false
		System.out.println(file2.exists());
	}

}

```







### File类的删除功能

- A:删除功能
  - public boolean delete():删除文件或者文件夹
- B:案例演示
  - File类的删除功能
  - 注意事项：
    - Java中的删除不走回收站。
    - 要删除一个文件夹，请注意该文件夹内不能包含文件或者文件夹

```java
public static void demo1() {
		File file1 = new File("xxx.txt");
		boolean b1 = file1.delete();
		System.out.println(b1);
		
		File file2 = new File("bbb.txt");
		boolean b2 = file2.delete();				//删除文件夹,必须删的是空的
		System.out.println(b2);
	}
```





### File类的重命名功能

- A:重命名功能
  - public boolean renameTo(File dest):把文件重命名为指定的文件路径
- B:案例演示
  - File类的重命名功能
  - 注意事项：
    - 如果路径名相同，就是改名。
    - 如果路径名不同，就是改名并剪切。



```java
public static void main(String[] args) {
		//demo1();
		File file1 = new File("zzz.txt");
		File file2 = new File("D:\\xxx.txt");
		boolean b = file1.renameTo(file2);
		System.out.println(b);
	}
```





### File类的判断功能

- A:判断功能
  - public boolean isDirectory():判断是否是目录
  - public boolean isFile():判断是否是文件
  - public boolean exists():判断是否存在
  - public boolean canRead():判断是否可读
  - public boolean canWrite():判断是否可写
  - public boolean isHidden():判断是否隐藏
- B:案例演示
  - File类的判断功能

```java
package cn.itcast.file;

import java.io.File;

public class Demo4_File {

	/**
	 * * A:判断功能
	* public boolean isDirectory():判断是否是目录
	* public boolean isFile():判断是否是文件
	* public boolean exists():判断是否存在
	* public boolean canRead():判断是否可读
	* public boolean canWrite():判断是否可写
	* public boolean isHidden():判断是否隐藏
	* 
	 */
	public static void main(String[] args) {
		//demo1();
		File file = new File("bbb.txt");
		//file.setReadable(false);				//在windows系统下所有的文件都是可读
		//boolean b1 = file.canRead();
		//System.out.println(b1);
		
		file.setWritable(true);					//在windows系统下可以设置为不可写
		boolean b2 = file.canWrite();
		System.out.println(b2);
		
		File file2 = new File("ccc.txt");
		System.out.println(file2.isHidden());	//判断是否是隐藏文件
		System.out.println(file.isHidden());
	}

	public static void demo1() {
		File file1 = new File("aaa");
		File file2 = new File("bbb.txt");
		System.out.println(file1.isDirectory());
		System.out.println(file2.isDirectory());
		
		System.out.println("---------------------------");
		System.out.println(file1.isFile());
		System.out.println(file2.isFile());
	}

}

```







### File类的获取功能

- A:获取功能
  - public String getAbsolutePath()：获取绝对路径
  - public String getPath():获取相对路径
  - public String getName():获取名称
  - public long length():获取长度。字节数（文件大小）
  - public long lastModified():获取最后一次的修改时间，毫秒值
  - public String[] list():获取指定目录下的所有文件或者文件夹的名称数组
  - public File[] listFiles():获取指定目录下的所有文件或者文件夹的File数组 
- B:案例演示
  - File类的获取功能

```java
package cn.itcast.file;

import java.io.File;
import java.util.Date;

public class Demo5_File {

	/**
	 *A:获取功能
	* public String getAbsolutePath()：获取绝对路径
	* public String getPath():获取相对路径
	* public String getName():获取名称
	* public long length():获取长度。字节数
	* public long lastModified():获取最后一次的修改时间，毫秒值
	* public String[] list():获取指定目录下的所有文件或者文件夹的名称数组
	* public File[] listFiles():获取指定目录下的所有文件或者文件夹的File数组 
	 */
	public static void main(String[] args) {
		//demo1();
		//demo2();
		demo3();
//        demo4();
	}

    private static void demo4() {
        File dir = new File("F:\\2015\\0302\\day19异常+File");
        String[] arr1 = dir.list();					//只是为了获取名字

        for (String string : arr1) {
            System.out.println(string);
        }

        File[] subFiles = dir.listFiles();			//为了获取File对象,对其做各种操作
        for (File file : subFiles) {
            System.out.println(file);

        }
    }

    public static void demo3() {
		File file = new File("ccc.txt");
		long time = file.lastModified();		//获取文件最后的修改时间
		System.out.println(time);
		
		Date d = new Date(time);
		System.out.println(d);
	}

	public static void demo2() {
		File file = new File("F:\\2015\\0302\\day19异常+File\\day19笔记.md");
		long len = file.length();				//如果获取的是文件返回的是字节个数
		System.out.println(len);
		
		File file2 = new File("F:\\2015\\0302\\day19异常+File");
		long len2 = file2.length();				//如果直接获取文件夹的大小返回就是0
		System.out.println(len2);
	}

	public static void demo1() {
		File file1 = new File("ccc.txt");
		System.out.println(file1.getAbsolutePath());		//获取绝对路径
		System.out.println(file1.getPath());				//获取相对路径
		System.out.println(file1.getName());
		
		File file2 = new File("D:\\150302\\day19");
		System.out.println(file2.getName());	 			//获取名称(文件和文件夹)
	}

}

```



###  输出指定目录下指定后缀的文件名

- A:案例演示
  - 需求：判断E盘目录下是否有后缀名为.jpg的文件，如果有，就输出该文件名称



```java
File dir = new File("E:\\");				//将路径封装成File对象
		/*String[] arr = dir.list();					//获取这个路径下所有的文件或文件夹路径
		
		for (String s : arr) {
			if(s.endsWith(".jpg")) {
				System.out.println(s);
			}
		}*/
		File[] subFiles = dir.listFiles();
		
		for (File subFile : subFiles) {
			if(subFile.isFile() && subFile.getName().endsWith(".jpg")) {
				System.out.println(subFile.getName());
			}
		}
		
```











###  文件名称过滤器的概述及使用

- A:文件名称过滤器的概述
  - public String[] list(FilenameFilter filter)
  - public File[] listFiles(FilenameFilter filter)
- B:文件名称过滤器的使用
  - 需求：判断E盘目录下是否有后缀名为.jpg的文件，如果有，就输出该文件名称
- C:源码分析
  - 带文件名称过滤器的list()方法的源码



```java
package cn.itcast.file;

import java.io.File;
import java.io.FilenameFilter;

public class Demo1_File {

	/**
	 * * A:案例演示
	 * 需求：判断E盘目录下是否有后缀名为.jpg的文件，如果有，就输出该文件名称
	 */
	public static void main(String[] args) {
		File dir = new File("E:\\");				//将路径封装成File对象
		/*String[] arr = dir.list();					//获取这个路径下所有的文件或文件夹路径
		
		for (String s : arr) {
			if(s.endsWith(".jpg")) {
				System.out.println(s);
			}
		}*/
		/*File[] subFiles = dir.listFiles();
		
		for (File subFile : subFiles) {
			if(subFile.isFile() && subFile.getName().endsWith(".jpg")) {
				System.out.println(subFile.getName());
			}
		}*/


		//遍历所有的文件名字
		String[] arr = dir.list(new FilenameFilter() {

			@Override
			public boolean accept(File dir, String name) {
				File file = new File(dir,name);

				//满足是一个文件并且名字是以.txt结束的
				return file.isFile() && file.getName().endsWith(".txt");
//			return  true;  返回文件架里面的文件目录
 			}
		});

		//打印
		for (String s : arr) {
			System.out.println(s);
		}
		
	}

}
```

### 递归

- 5的阶乘	

  ​	

```java
package cn.itcast.file;

public class Demo2_Digui {

	/**
	 * @param args
	 * 递归
	 * 方法自己调用自己
	 * 弊端:容易栈内存溢出
	 * 好处:不用知道调用次数
	 * 5! 5 * 4 * 3 * 2 * 1
	 * 5 * fun(4)
	 * 		4 * fun(3)
	 * 				3 * fun(2)
	 * 						2 * fun(1)
	 * 构造函数不能使用递归
	 * 递归调用是否必须有返回值?
	 * 不一定(可以有也可以没有)
	 */
	public static void main(String[] args) {
		/*int result = 1;
		for(int i = 1; i <= 5; i++) {
			result = result * i;
		}
		
		System.out.println(result);*/
		System.out.println(fun(10000));//超出范围了。
	}

	public static int fun(int num) {
		if(num == 1) {
			return 1;
		}else {
			return num * fun(num - 1);
		}
	}
}



```



### 打印某个路径下的所有java文件



```java
package cn.itcast.test;

import java.io.File;
import java.util.Scanner;

public class Test1 {

	/**
	 * @param args
	 * 将一个路径下的所有.java文件打印
	 */
	public static void main(String[] args) {
		File dir = getDir();
		printJavaFile(dir);
	}

	/*
	 * 键盘获取一个文件夹路径
	 */
	public static File getDir() {
		Scanner sc = new Scanner(System.in);
		System.out.println("请输入一个文件夹路径:");
		while(true) {
			String line = sc.nextLine();
			File dir = new File(line);
			if(!dir.exists()) {
				System.out.println("您录入的路径不存在,请重新输入一个文件夹路径:");
			}else if(dir.isFile()) {
				System.out.println("您录入的是文件路径,请重新输入一个文件夹路径:");
			}else {
				return dir;
			}
		}
	}
	
	/*
	 * 打印一个文件夹路径下的所有.java文件
	 */
	public static void printJavaFile(File dir) {
		File[] subFiles = dir.listFiles();			//获取dir路径下所有的文件和文件夹
		
		for (File subFile : subFiles) {				//遍历数组
			if(subFile.isFile() && subFile.getName().endsWith(".java")) {//如果是文件并且后缀是.java
				System.out.println(subFile);		//打印.java文件
			}else if(subFile.isDirectory()){		//如果是目录
				printJavaFile(subFile);				//就继续递归调用
			}
		}
	}
}

```



