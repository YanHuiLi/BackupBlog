---
title: Java学习笔记整理（23）
date: 2017-08-16 06:58:08
tags: [J2SE,多态,集合]
categories: J2SE
top: 23
---

---

对之前学习课程进行一个复习。

1. 多态
2. Collection
3. List
4. set
5. map

<!--more-->



### 多态

* 访问成员变量

![](http://ogtmd8elu.bkt.clouddn.com/201708160728_236.png)

当访问成员变量的时候，会去父类的区域去找，所以输出10.

* 访问成员方法



![](http://ogtmd8elu.bkt.clouddn.com/201708160734_265.png)

当访问成员方法的时候，存在一个动态绑定的过程，找的虽然编译的是父类的print方法，但是实际运行的是子类的print方法。



* 访问静态方法

静态方法不存在动态绑定，访问到的是父类的数据。

```java
package cn.itcast.oop;

public class Demo1_DuoTai {

	/**
	 * @param args
	 * 多态:多种形态,子类的变化
	 * 多态中的成员变量,编译和运行都看父类(左边)
	 * 静态方法不存在重写(覆盖)
	 * 
	 *
	 * 1,继承或实现
	 * 2,重写（壳不变，核心变）
	 * 3,父类引用指向子类对象
	 */
	public static void main(String[] args) {
		/*
		输出10，很奇怪吧，其实是因为用的父类，在堆上创建new zi()的时候，预留了fu的super的部分。
		所以在找成员变量的时候，实在父类的区域去找的。
		所以输出的f.num=10；
		 */

		Fu f = new Zi();
		System.out.println(f.num);// out：10

		/*Zi z = (Zi)f;					//向下转型
		System.out.println(z.num);*/
		//f.print();
//		Fu.run();  //out: fu static
	}

}

class Fu {
	int num = 10;
	
	public void print() {
		System.out.println("Fu");
	}
	
	public static void run() {
		System.out.println("Fu static");
	}
}

class Zi extends Fu {
	int num = 20;
	
	public void print() {
		System.out.println("Zi");
	}
	
	public static void run() {
		System.out.println("Zi static");
	}
}

```



### 多态的三个实现条件

* 多态:多种形态,子类的变化
* 多态中的成员变量,编译和运行都看父类(左边)
* 静态方法不存在重写(覆盖)




### 多态的利弊

* 好处

  解耦，减少的代码的冗余性

  是面向对向的基石。

* 弊端

  拿不到子类的特性。如果需要，则要进行向下转型。

  `Person p = (Person)obj`

  ​

### 多态实现equals方法比较对象内容

```java
//这个方法不完整，一般使用alt+ins 自动复写equals 和 hashcode方法

public boolean equals(Object obj) {
   Person p = (Person)obj;
   return this.name.equals(p.name) && this.age == p.age;
}
```



### Collection（List）集合

* 集合的作用

  集合本质上一种容器，用来存储对象


* 数据和集合的区别
  1. 数组的长度不可变，集合可以改变。
  2. 数组可以存储基本数据类型和引用数据类型，集合都能存在引用数组类型。（地址值）
* 适用范围
  1. 当长度确定的话，用数组。数组的效率高，但是长度固定。
  2. 不确定就用集合，集合效率低，长度不用管，会自动扩大。
* ArratList
  * 初始默认size为10，每次扩容1.5倍。

### List集合的特点

* 存取有序，有index，可以重复
* ArrayList
  * 数组实现，查询修改块，增删慢
* LinkedList
  * 链表实现 增删快，查询修改慢
* Vector
  * 数组实现

### Set集合的特点

* 存取无序，没有索引，不可以重复



### ArratList和Vector的区别

* ArrayList是线程不安全的，效率高 JDK 1.2，
* Vector是线程安全的，效率低，JDK 1.0。

### ArrayList和LinkedList的区别

* 都是线程不安全的



* ArrayList 数组实现，查询修改块，增删慢
* LinkedList 链表实现 增删快，查询修改慢



### List的迭代

1. 迭代器Iterator ，hasNext()和Next()方法
2. 增强for循环
3. 普通for循环 size() get()
4. Vector , Emueration hasMoreEelements(),NextElement



### List迭代删除的问题

1. 迭代器，只能用自身的remove方法，用集合的方法会出现，并发修改异常
2. 普通for循环，可以删除，索引要--
3. 增强for循环不能删除



### List集合的具体使用

1. 如果元素重复，使用List
2. 如果查找多就用ArrayList
3. 如果增删多就用LinkedList
4. 都多就用ArrayList

### Collection（set和Map）

* HashSet
* TreeSet

### HashSet如何保证元素唯一？

需要重写equals和hashcode方法。

当向一个hashset里面加入一个元素的时候，会先调用的HashCode方法，当HashCode的返回值相同的时候，才会调用equals方法，进行比较，equals为true就不存，说明两个对象内容相等，就不存。equals方法返回false就存进去。

确保相同的hashcode值返回值必须相同。所以在重新hashcode方法的时候，要尽量满足返回的hashcode值唯一。



### TreeSet如何保证元素的唯一性？

* 两种比较方式（优先使用比较器）

1. 自然排序，实现comparable接口，重写compareTo方法，根据返回值，进行排序
   * 正数存右边，负数存左边，0不存
2. 比较器排序，在treeset的集合的构造中传入比较器，比较器是comparator的子类对象，重新comepare方法进行排序
   * 正数存右边，负数存左边，0不存



### Set集合的迭代

1. 迭代器
2. 增强for循环



### Set集合具体使用哪个？

 如果元素不能重复，用Set集合

如果元素不需要排序就用hashSet

如果排序就用treeSet

* 如果集合中有重复的元素，但是还是要求排序和保留重复的当compare方法返回是0的时候，返回一个非零的数字。

```java
TreeSet<String> ts = new TreeSet<>(new Comparator<String>() {

			@Override
			public int compare(String s1, String s2) {
				int num = s1.compareTo(s2);
				return num == 0 ? 1 : num;//存储重复的
			}
		});
		ts.add("c");
		ts.add("c");
		ts.add("c");
		ts.add("a");
		ts.add("b");
		ts.add("b");
		ts.add("b");
		
		System.out.println(ts);
```

开发用的最多的是HashSet。



### Map

* HashMap
* TreeMap

Map中键都是唯一的，如果存储键值对中，已经有该键，则加入的值把原来的值覆盖。



### HashMap

* HashSet依赖于HashMap
* 唯一性保证：还是通过重写hashCode和equals方法。

### TreeMap

* TreeSet依赖于TreeMap



* 保值键的唯一
* 自然排序，实现comparable接口，重写compareTo方法，根据返回值，进行排序
  - 正数存右边，负数存左边，0不存
* 比较器排序，在treeset的集合的构造中传入比较器，比较器是comparator的子类对象，重新comepare方法进行排序
  - 正数存右边，负数存左边，0不存



### Map集合的迭代

没有迭代器。

1. keyset  获取所有的键值对，根据键获取值
2. entrySet  获取所有的键值对对象，根据键值对封装的对象获取对应的值（getKey，getvalue）
3. foreach语句遍历。

```java
package cn.itcast.collection;

import java.util.ArrayList;
import java.util.Comparator;
import java.util.Enumeration;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map.Entry;
import java.util.TreeSet;
import java.util.Vector;

public class Demo1_Collection {

	/**
	 * @param args
	 * 集合:容器,存储对象
	 * 集合和数组的区别?
	 * 1,数组的长度是固定的
	 *   集合的长度是可变的,可以随着元素的增加而增长,可以随着元素减少而减少
	 * 2,数组既可以基本数据类型,也可以存储引用数据类型
	 *   集合只能存储引用数据类型
	 *   
	 * 什么时候用数组,什么时候用集合,为什么?
	 * 固定用数组,数组的效率高,但是长度固定
	 * 变化用集合,集合的效率低,但是变化不用我们操心
	 * 
	 * Collection
	 * 		List
	 * 			存取有序,有索引,可以重复
	 * 			ArrayList
	 * 				数组实现,查询快,修改快,增和删慢
	 * 			Vector
	 * 				数组实现
	 * 			LinkedList
	 * 				链表实现,增和删快,查询和修改慢
	 * 		Set
	 * 			存取无序,没有索引,不可以重复
	 * 			HashSet
	 * 				如何保证元素唯一
	 * 				需要重写hashCode()和equals方法
	 * 			TreeSet
	 * 				如何保证元素唯一
	 * 				比较方式两种
	 * 				1,自然排序,实现Comparable,重写compareTo()方法,根据compareTo()方法,返回值,正数,负数和零
	 * 				正数存右边,负数存左边,0不存
	 * 				2,比较器排序,在TreeSet集合的构造中传入比较器,比较器是Comparator的子类对象,重写compare()方法
	 * 				根据compare()方法,返回值,正数,负数和零,正数存右边,负数存左边,0不存
	 * 
	 * 		ArrayList是线程不安全的,效率高(JDK1.2)
	 * 		Vector是线程安全的,效率低(JDK1.0)
	 * List集合的迭代
	 * 		1,迭代器Iterator,hasNext()和next()
	 * 		2,增强for循环
	 * 		3,普通for循环,size(),get()
	 * 		4,Vector, Emumeration hasMoreElements()nextElement()
	 * List集合三种迭代删除的问题
	 * 		1,迭代器,只能用迭代器自身remove方法,如果集合中的删除方法,会出现并发修改异常
	 * 		2,普通for可以删除,但是索引要--
	 * 		3,增强for循环不能删除
	 * List集合具体用哪个
	 * 		如果元素重复,就用List集合
	 * 		如果查找多就用ArrayList
	 * 		如果增删多就用LinkedList
	 * 		如果都多ArrayList
	 * Set集合的迭代
	 * 		1,迭代器
	 * 		2,增强for循环
	 * Set集合具体用哪个?
	 * 		如果元素不能重复,用Set集合
	 * 		如果元素不需要排序,HashSet,效率高(不考虑顺序)
	 * 		如果排序用TreeSet
	 * 			如果集合中有重复的元素,但是还需要排序要求保留重复的(针对的是java给我们提供的类),只能用比较器,当compare()方法返回是0的时候,给他返回非
	 * 			0的数字
	 * 		开发用的最多是HashSet	
	 * Map的键都是唯一的,如果存储键值对的时候,集合中已有该键,新加入的值将已有的值覆盖
	 * 		HashMap
	 * 			HashSet底层依赖于HashMap
	 * 				如何保证键的唯一
	 * 				重写hashCode()和equals()方法
	 * 		TreeMap
	 * 			TreeSet底层依赖于TreeMap
	 * 				如何保证键唯一
	 * 				1,自然排序,实现Comparable,重写compareTo()方法,根据compareTo()方法,返回值,正数,负数和零
	 * 				正数存右边,负数存左边,0不存
	 * 				2,比较器排序,在TreeSet集合的构造中传入比较器,比较器是Comparator的子类对象,重写compare()方法
	 * 				根据compare()方法,返回值,正数,负数和零,正数存右边,负数存左边,0不存
	 * Map集合的迭代
	 * 1,keySet()获取所有的键,根据键获取值get(key)
	 * 2,entrySet()获取所有的键值对对象,根据键值对对象,获取键getKey()和值getValue()
	 */
	public static void main(String[] args) {
		ArrayList<String> list = new ArrayList<>();
		list.add("a");
		list.add("b");
		list.add("c");
		list.add("d");
		list.add("e");
		
		/*Iterator<String> it = list.iterator();
		while(it.hasNext()) {
			String str = it.next();
			System.out.println(str);
		}*/
		
		/*for(String s : list) {
			System.out.println(s);
		}*/
		
		/*for(int i = 0; i < list.size() ; i++) {		//如果需要用到索引的时候
			System.out.println(list.get(i));
		}*/
		
		Vector<Integer> v = new Vector<>();
		v.add(100);
		v.add(111);
		v.add(112);
		v.add(113);
		
		Enumeration<Integer> en = v.elements();
		while(en.hasMoreElements()) {
			System.out.println(en.nextElement());
		}
		
		TreeSet<String> ts = new TreeSet<>(new Comparator<String>() {

			@Override
			public int compare(String s1, String s2) {
				int num = s1.compareTo(s2);
				return num == 0 ? 1 : num;//存储重复的
			}
		});
		ts.add("c");
		ts.add("c");
		ts.add("c");
		ts.add("a");
		ts.add("b");
		ts.add("b");
		ts.add("b");
		
		System.out.println(ts);
		
		HashMap<String, Integer> hm = new HashMap<>();
		hm.put("张三", 23);
		hm.put("李四", 24);
		hm.put("王五", 25);
		hm.put("赵六", 26);
		
		/*for(String key : hm.keySet()) {
			System.out.println(key + "=" + hm.get(key));
		}*/
		
		for(Entry<String, Integer> e : hm.entrySet()) {
			System.out.println(e.getKey() + "=" + e.getValue());
		}
	}

}

```



### JDK1.5新特性



```java
package cn.itcast.collection;

public class Demo2_JDK5 {

   /**
    * @param args
    * JDK1.5的新特性
    * 1,自动拆装箱
    *        方便运算
    *        Integer x = 10;       //自动装箱
    *        int y = x + 20;       //自动拆箱
    * 2,泛型
    *        将运行期的错误转换编译期
    *        省去强转的麻烦
    *        一种安全机制
    * 3,增强for循环
    *        简化遍历代码
    * 4,静态导入
    *        开发不用
    * 5,可变参数
    *        就是可以变化的数组
    */
   public static void main(String[] args) {
      
   }

}
```

### Exception



```java
package cn.itcast.collection;

public class Demo3_Exception {

	/**
	 * @param args
	 * 异常:程序运行中的错误
	 * Throwable
			Error
			Exception 
				编译时异常
					除了RuntimeException以及他所有的子类
					编译时必须处理,要么try要么throws
				运行时异常
					RuntimeException以及他所有的子类
					编译不报错,运行时报错
					属于程序员犯的错误,需要回来修改代码
					例如,空指针异常,索引越界异常,并发修改异常,除0异常,类型转换异常
				什么时候try什么时候throws
					当后续代码需要执行用try
					当后续代码不需要执行throws
				如何自定义异常?
					继承Exception或RuntimeException
					构造方法中用super交给父类完成
				为什么要自定义异常呢?
					主要想见名知意
				try catch finally分别有什么作用?
				try
					检测代码
				catch
					捕获异常
				finally
				   	释放资源(关闭流,关闭数据库等)
				   	
				1,try catch
				2,try finally
				3,try catch finally
				finally一定会执行吗?
				会的,除非在他前面退出jvm虚拟机System.exit(0)
				
				throw和throws的区别?
				throws放在方法上,后面跟异常类名,如果是多个,用逗号隔开
				throw放在方法内,后面跟异常对象,只能跟一个
				
				如果在方法内抛出的异常对象是一个编译时异常,在方法上必须用throws声明异常信息
				如果在方法内抛出的异常对象是一个运行时异常,在方法上不用抛出
	 */
	public static void main(String[] args) {
		
	}

}

class Demo {
	public int div(int a,int b){
		if(b <= 0) {
			throw new RuntimeException("除数为负数和零了");
		}else {
			return a / b;
		}
	}
}

```





### 字节流

```java
package cn.itcast.collection;

import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class Demo4_IO {

	/**
	 * @param args
	 * IO流按流向分:输入流和输出流
	 * 按照操作的数据分:字节流和字符流
	 * 字节流的抽象基类,可以操作的是任意类型的数据
	 * 		InputStream
	 * 		OutputStream
	 * 字符流,可以操作的是纯文本的数据,获取的字节,将字节转换为字符,写出的时候,先写出的是字符,然后转换成字节
	 * 		Reader
	 * 		Writer
	 * 字节输入流
	 * 		FileInputStream读取一个文件,文件必须存在,如果不在,会抛出FileNotFoundException
	 * 		读取文件为什么返回是int而不是byte
	 * 字节输出流
	 * 		FileOutputStream写出文件时,文件不用必须存在(路径存在),如果不存在就创建一个文件出来,如果想续写,用构造方法
	 * 		在第二个参数传一个true
	 * 四种拷贝:
	 * 		1,逐个字节拷贝(效率太低)
	 * 		2,定义一个大数组,文件多少个字节,数组就多大(容易内存溢出)
	 * 		3,定义一个小数组,是1024的整数倍
	 * 		4,带Buffer的拷贝
	 * 两种异常处理方式(面试可能会问，两种异常的处理)
	 * 		1,1.7版本以前的
	 * 		2,1.7版本的
	 * @throws IOException 
	 * @throws IOException 
	 */
	public static void main(String[] args) throws IOException {
		//demo1();
		//demo2();
		//demo3();
		//demo4();
		//demo5();
		try(
			FileInputStream fis = new FileInputStream("a.txt");
			FileOutputStream fos = new FileOutputStream("b.txt");
		){
			int b;
			while((b = fis.read()) != -1) {
				fos.write(b);
			}
		}
	}

	public static void demo5() throws FileNotFoundException, IOException {
		FileInputStream fis = null;					//局部变量在使用之前必须赋值
		FileOutputStream fos = null;
		try {
			fis = new FileInputStream("a.txt");
			fos = new FileOutputStream("b.txt");
			
			int b;
			while((b = fis.read()) != -1) {
				fos.write(b);
			}
			
			
		} finally {
			try {
				if(fis != null)
					fis.close();
			} finally {
				if(fos != null)
					fos.close();
			}
			
		}
	}

	public static void demo4() throws FileNotFoundException, IOException {
		FileInputStream fis = new FileInputStream("a.txt");
		FileOutputStream fos = new FileOutputStream("b.txt");
		BufferedInputStream bis = new BufferedInputStream(fis);
		BufferedOutputStream bos = new BufferedOutputStream(fos);
		
		int b;
		while((b = bis.read()) != -1) {
			bos.write(b);
		}
		
		bis.close();
		bos.close();
	}

	public static void demo3() throws FileNotFoundException, IOException {
		FileInputStream fis = new FileInputStream("a.txt");
		FileOutputStream fos = new FileOutputStream("b.txt");
		
		byte[] arr = new byte[8192];
		int len;
		while((len = fis.read(arr)) != -1) {
			fos.write(arr,0,len);
		}
		
		fis.close();
		fos.close();
	}

	public static void demo2() throws FileNotFoundException, IOException {
		FileInputStream fis = new FileInputStream("a.txt");
		FileOutputStream fos = new FileOutputStream("b.txt");
		byte[] arr = new byte[fis.available()];
		fis.read(arr);
		fos.write(arr);
		
		fis.close();
		fos.close();
	}

	public static void demo1() throws FileNotFoundException, IOException {
		FileInputStream fis = new FileInputStream("a.txt");
		FileOutputStream fos = new FileOutputStream("b.txt");
		
		int b;
		while((b = fis.read()) != -1) {
			fos.write(b);
		}
		
		fis.close();
		fos.close();
	}

}

```



### 字符流



```java
package cn.itcast.collection;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.ByteArrayOutputStream;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.OutputStreamWriter;
import java.io.UnsupportedEncodingException;
import java.util.Scanner;

import cn.itcast.bean.Person;

public class Demo5_CharIO {

   /**
    * @param args
    *     字符流:
    *        可以读写字符的IO流,读取的时候先将字节转换成字符(编码表),写出的时候将字符转换成字节写出(编码表)
    * FileReader
    * FileWriter
    *        拷贝的纯文本文件用字节流好还是字符流好?
    *        字节流,不用转换
    *        字符流是否可以拷贝非纯文本的文件
    *        不可以,读的时候会将字节转换字符,转换过程中有可能没有找到对应的码表值,会用?代替,写出的时候将字符转换为字节,就会将
    *        ?写出,导致乱码
    * BufferedReader
    *        readLine();
    * BufferedWriter
    *        newLine();写出换行,是跨平台的
    *        \r\n只支持的是windows系统
    * 
    * 转换流?
    * BufferedReader br = 
            new BufferedReader(new InputStreamReader(new FileInputStream("a.txt"), "UTF-8"));
      BufferedWriter bw = 
            new BufferedWriter(new OutputStreamWriter(new FileOutputStream("b.txt"),"GBK"));
      InputStreamReader 字节通向字符的桥梁
      OutputStreamWriter 字符通向字节的桥梁
      内存输出流
         ByteArrayOutputStream把内存当作缓冲区
      打印流,操作数据目的
      PrintStream
      PrintWriter
         write
         print
         println(可以自动刷出)
         print和println可以自动调用对象的toString()方法
      ObjectOutputStream对象输出流,写出对象序列化
         写出对象时,该对象所属的类必须实现Serializable
         是否必须加id号?
         不是必须加
      ObjectInputStream对象输入流,反序列化
      
      
      1,输出第八个数
      1,1,2,3,5,8,13斐波那契数列(数组或递归)
      2,1000!末尾有多少个零
    * @throws IOException 
    * @throws UnsupportedEncodingException 
    * @throws ClassNotFoundException 
    */
   public static void main(String[] args) throws UnsupportedEncodingException, IOException, ClassNotFoundException {
      /*BufferedReader br = 
            new BufferedReader(new InputStreamReader(new FileInputStream("a.txt"), "UTF-8"));
      BufferedWriter bw = 
            new BufferedWriter(new OutputStreamWriter(new FileOutputStream("b.txt"),"GBK"));*/
      
      /*Scanner sc = new Scanner(System.in);
      String line = sc.nextLine();
      
      BufferedReader br =
            new BufferedReader(new InputStreamReader(System.in));
      String line2 = br.readLine();*/
      /*FileInputStream fis = new FileInputStream("a.txt");
      ByteArrayOutputStream baos = new ByteArrayOutputStream();
      
      int b;
      while((b = fis.read()) != -1) {
         baos.write(b);
      }
      byte[] arr = baos.toByteArray();
      System.out.println(new String(arr));
      System.out.println(baos);
      
      fis.close();*/
      /*Person p = new Person("张三", 23);
      ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("a.txt"));
      oos.writeObject(p);
      
      oos.close();*/
      
      /*ObjectInputStream ois = new ObjectInputStream(new FileInputStream("a.txt"));
      Person p = (Person)ois.readObject();
      System.out.println(p);
      ois.close();*/
      
      int[] arr = new int[8];
      arr[0] = 1;
      arr[1] = 1;
      
      for(int i = 2; i < arr.length; i++) {
         arr[i] = arr[i-1] + arr[i-2];
      }
      
      System.out.println(arr[arr.length-1]);
      System.out.println(getNum(8));
   }

   public static int getNum(int num) {
      if(num == 1 || num == 2) {
         return 1;
      }else {
         return getNum(num - 1) + getNum(num - 2);
      }
   }
   
   public static int getNum2(int num) {
      if(num < 5) {
         return 0;
      }else {
         return num / 5 + getNum2(num / 5);
      }
   }
}
```



### 递归



```java
package cn.itcast.collection;

import java.io.File;
import java.util.Scanner;

public class Demo6_DiGui {

   /**
    * @param args
    * 递归
    *        方法自己调用自己
    * 递归的好处
    *        不用知道调用次数
    * 递归的坏处
    *        容易导致栈内存溢出
    * 递归是否必须有返回值?
    *        不一定有返回值
    * 构造函数可否用递归调用
    *        不可以
    * 
    * 打印层级目录
    */
   public static void main(String[] args) {
      File dir = getDir();
      printLevFile(dir,0);
   }

   /*
    * 键盘录入获取文件夹路径
    * 1,File
    * 2,参数列表无
    */
   public static File getDir() {
      Scanner sc = new Scanner(System.in);
      System.out.println("请输入文件夹路径:");
      while(true) {
         String line = sc.nextLine();
         File dir = new File(line);
         if(!dir.exists()) {
            System.out.println("重输");
         }else if (dir.isFile()) {
            System.out.println("重输");
         }else {
            return dir;
         }
      }
   }
   
   /*
    * 打印文件夹路径下的所有文件和文件夹的层级目录
    * 1,void
    * 2,参数列表(File dir,int lev)
    */
   public static void printLevFile(File dir,int lev) {
      File[] subFiles = dir.listFiles();
      
      for (File subFile : subFiles) {
         for(int i = 0; i <= lev; i++) {
            System.out.print('\t');
         }
         System.out.println(subFile);
         
         if(subFile.isDirectory()) {
            printLevFile(subFile,lev + 1);
         }
      }
   }
}
```





![](http://ogtmd8elu.bkt.clouddn.com/201708161238_747.png)

![](http://ogtmd8elu.bkt.clouddn.com/201708161238_338.png)



### 斐波那契数列

```java
package cn.itcast.collection;

public class Demo7_Test {

   /**
    * @param args
    * 斐波那契
    * 1,1,2,3,5,8,13,21
    * fun(1) = 1
    * fun(2) = 1
    * fun(3) = fun(1) + fun(2)
    */
   public static void main(String[] args) {
      //数组
   /* int[] arr = new int[10];
      arr[0] = 1;
      arr[1] = 1;
      
      for(int i = 2; i < arr.length; i++) {
         arr[i] = arr[i - 2] + arr[i - 1];
      }
      
      System.out.println(arr[arr.length - 1]);*/
      //递归
      
      System.out.println(fun(10));
   }

   /*
    * 递归算斐波那契数列
    * 1,返回值int
    * 2,int num
    */
   public static int fun(int num) {
      if(num == 1 || num == 2) {
         return 1;
      }else {
         return fun(num - 1) + fun(num - 2);
      }
   }
}
```

### 1000！计算

```java
package cn.itcast.collection;

import java.math.BigInteger;

public class Demo8_Test {

	/**
	 * @param args
	 * 1000!
	 */
	public static void main(String[] args) {
		/*int result = 1;
		for(int i = 1; i <= 1000; i++) {
			result = result * i;
		}
		
		System.out.println(result);*/
		BigInteger bi1 = new BigInteger("1");
		for(int i = 1; i <= 1000; i++) {
			BigInteger bi2 = new BigInteger(i + "");
			bi1 = bi1.multiply(bi2);
		}
		//算出1000!中所有的0
		/*String str = bi1.toString();
		int count = 0;									//计数器
		for(int i = 0; i < str.length(); i++) {
			if(str.charAt(i) == '0') {
				count++;
			}
		}
		
		System.out.println(count);*/
		//算出1000!中尾部有多少个零
		String str = bi1.toString();
		str = new StringBuilder(str).reverse().toString();		//链式编程,将字符串反转
		int count = 0;
		for(int i = 0; i < str.length(); i++) {
			if(str.charAt(i) != '0') {
				break;
			}else {
				count++;
			}
		}
		
		System.out.println(count);
	}

}

```

```java
package cn.itcast.collection;

public class Demo9_Test {

	/**
	 * @param args
	 * 100!尾部有多少个零
	 * 5 10 15 20 25 30 35 40 45 50 55 60 65 70 75 80 85 90 95 100  20个 100 / 5 = 20
	 * 25 50 75 100 20 / 5 = 4
	 * 5 * 5 * 5 * 1 = 125有3个5的一共有8个
	 * 5 * 5 * 5 * 2 = 250
	 * 5 * 5 * 5 * 3 = 375
	 * 5 * 5 * 5 * 4 = 500
	 * 
	 * 5 * 5 * 5 * 5 = 625有4个5的一共有1个
	 * 1000!尾部有多少个零
	 * 1000 / 5 = 200
	 * 200 / 5 = 40
	 * 40 / 5 = 8
	 * 8 / 5 = 1
	 */
	public static void main(String[] args) {
		System.out.println(getNum(1000));
	}

	public static int getNum(int num) {
		if(num < 5) {
			return 0;
		}else {
			return num / 5 + getNum(num / 5);
		}
	}
}

```

