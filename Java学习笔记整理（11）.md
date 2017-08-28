---
title: Java学习笔记整理（11）
date: 2017-07-20 09:58:49
tags: [J2SE]
categories: J2SE
top: 11
---

---

1. object类的概述
2. hashCode()和getclass()方法的使用
3. toString()方法的重写
4. equals()方法的重写
5. ==和equals()的区别

<!--more-->

### 面试题

```java
class X {
	
	Y b = new Y();
	X() {
		System.out.print("X");
	}
}
class Y {
	Y() {
		System.out.print("Y");
	}
}
public class Z extends X {
	Y y = new Y();
	Z() {
		super();
		System.out.print("Z");
	}
	public static void main(String[] args) {
		new Z(); 
	}
}
//输出是多少。
```

![](http://ogtmd8elu.bkt.clouddn.com/201707201655_728.png)


###  常见对象(API概述)

- A:API(Application Programming Interface) 
  - 应用程序编程接口
- B:Java API
  - 就是Java提供给我们使用的类，这些类将底层的实现封装了起来，
  - 我们不需要关心这些类是如何实现的，只需要学习这些类如何使用。

###  Object类的概述

- A:Object类概述
  - 类层次结构的根类（最顶层的类）
  - 所有类都直接或者间接的继承自该类
- B:构造方法
  - public Object()
  - 回想面向对象中为什么说：
  - 子类的构造方法默认访问的是父类的无参构造方法

### Object类的hashCode()方法

- A:案例演示
  - public int hashCode()
  - a:返回该对象的哈希码值。默认情况下，该方法会根据对象的地址来计算。
  - b:不同对象的，hashCode()一般来说不会相同。但是，同一个对象的hashCode()值肯定相同。
  - c:不是对象的实际地址值，可以理解为逻辑地址值。
    - 举例：物体和编号。


```java
package cn.itcast.object;

public class Demo1Hashcode {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		Object obj = new Object();
		int x = obj.hashCode();
		System.out.println(x);
	}

}

```





### Object类的getClass()方法

- A:案例演示
  - public final Class getClass()
  - a:返回此 Object 的运行时类。
    - b:可以通过Class类中的一个方法，获取对象的真实类的全名称。
    - public String getName()

  ```java
  package com.archer.hashcode;

  /**
   * Created by Archer on 2017/7/20.
   */
  public class hashCode {

      public static void main(String[] args) {
          Object o=new Object();
          int x=o.hashCode();
          System.out.println(x);
          Class clzz=o.getClass();
          String name = clzz.getName();
          System.out.println(name);
      }

  }

  ```

  ​

### Object类的toString()方法

- A:案例演示

  - public String toString()
  - a:返回该对象的字符串表示。

  ```java
    public Stirng toString() {
  ```


  	return name + "," + age;
  }
  ```

  - b:它的值等于： 
    - getClass().getName() + '@' + Integer.toHexString(hashCode()) 
  - c:由于默认情况下的数据对我们来说没有意义，一般建议重写该方法。

- B:最终版

  - 自动生成

​````java
package cn.itcast.bean;

public class Student {
	private String name;
	private int age;
	public Student() {
		super();//object类，其实什么也没有
		
	}
	public Student(String name, int age) {
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
	public void setAge(int age) {
		this.age = age;
	}
	
	public void eat() {
		System.out.println("学生吃饭");
	}
	
	public void sleep(){
		System.out.println("学生睡觉");
	}
	
	/*
	//告诉一些属性值，重写了tostring方法
	 *  public String toString() {
        	return getClass().getName() + "@" + Integer.toHexString(hashCode());
    	}
	 */
	
	public String toString() {
		return "我的姓名是:" +  name  + ",我的年龄是:" +  age;
	}
}
​````



​```java
package cn.itcast.object;

import cn.itcast.bean.Student;

public class Demo3ToString {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		Student s = new Student("张三", 23);
		System.out.println("我的姓名是:" + s.getName() + ",我的年龄是:" + s.getAge());
		
		System.out.println(s);
	}
	/*
	 *  public String toString() {
        	return getClass().getName() + "@" + Integer.toHexString(hashCode());
    	}
	 * */
}

  ```



### Object类的equals()方法

- A:案例演示
  - a:指示其他某个对象是否与此对象“相等”。 
  - b:默认情况下比较的是对象的引用是否相同。
  - c:由于比较对象的引用没有意义，一般建议重写该方法。
  - d:==和equals()的区别。(面试题)

```java
package cn.itcast.bean;

public class Worker {
	private String name;
	private int age;
	public Worker() {
		super();
		
	}
	public Worker(String name, int age) {
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
	public void setAge(int age) {
		this.age = age;
	}


	//重写toString方法
	public String toString() {
		return name + "," + age;
	}


	/*
	原始的equals方法比较的保存内容的地址，实际上没有多大意义。
	  public boolean equals(Object obj) {
        	return (this == obj);
    	}
	*/

	//重写equals方法，主要是比较内容里面的区别（仍不晚上）
	public boolean equals(Object obj) {	//Object obj = new Worker();
		Worker w = (Worker)obj;  //向下转型得到Worker的实例，才可以拿到数据进行比较。
		return this.name.equals(w.name) && this.age == w.age;
	}
}

```





```java
package cn.itcast.object;

import cn.itcast.bean.Worker;

public class Demo4Equals {

	/**2
	 * @param args
	 * alt + ctrl + 下键 向下复制一行
	 */
	public static void main(String[] args) {
		Worker w1 = new Worker("张三", 23);
		Worker w2 = new Worker("张三", 23);
		boolean b = w2.equals(w1);				//Object obj = w2;向上转型
		System.out.println(b);
		//Worker w3 = w2;
		//System.out.println(w2.equals(w3));
	}
	/*
	 *  public boolean equals(Object obj) {
        	return (this == obj);
    	}
	 */
}

```



### ==号和equals方法的区别(面试题)

- ==是一个比较运算符号,既可以比较基本数据类型,也可以比较引用数据类型,基本数据类型比较的是值,引用数据类型比较的是地址值
- equals方法是一个方法,只能比较引用数据类型,所有的对象都会继承Object类中的方法,如果没有重写Object类中的equals方法,equals方法和==号比较引用数据类型无区别,重写后的equals方法比较的是对象中的属性

