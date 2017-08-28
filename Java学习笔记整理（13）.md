---
title: Java学习笔记整理（13）
date: 2017-07-23 09:47:09
tags: [J2SE]
categories: [J2SE]
top: 13
---

---





<!--more-->

###  StringBuffer类的概述

- A:StringBuffer类概述
  - 通过JDK提供的API，查看StringBuffer类的说明
  - 线程安全的可变字符序列
- B:简述安全问题
  - 线程安全效率低 
- C:StringBuffer和String的区别
  - String是一个不可变的字符序列
  - StringBuffer是一个可变的字符序列 

### StringBuffer类的构造方法

- A:StringBuffer的构造方法：
  - public StringBuffer():无参构造方法
  - public StringBuffer(int capacity):指定容量的字符串缓冲区对象
  - public StringBuffer(String str):指定字符串内容的字符串缓冲区对象
- B:StringBuffer的方法：
  - public int capacity()：返回当前容量。	理论值(不掌握)
  - public int length():返回长度（字符数）。 实际值
- C:案例演示
  - 构造方法和长度方法的使用

```java
public static void demo1() {				//StringBuffer的构造函数
		StringBuffer sb = new StringBuffer();
		System.out.println(sb.capacity());		//理论值 16
		System.out.println(sb.length());		//实际值 0
		
		StringBuffer sb2 = new StringBuffer(10);
		System.out.println(sb2.capacity());		//capacity()返回的值就是底层字符数组的长度
		System.out.println(sb2.length());
		
		StringBuffer sb3 = new StringBuffer("abcdef");
		System.out.println(sb3.capacity());		//传入的字符串的长度加上16(默认的char数组长度)	
		System.out.println(sb3.length());
	}
```



###  StringBuffer的添加功能

- A:StringBuffer的添加功能
  - public StringBuffer append(String str):
    - 可以把任意类型数据添加到字符串缓冲区里面,并返回字符串缓冲区本身
  - public StringBuffer insert(int offset,String str):
    - 在指定位置把任意类型的数据插入到字符串缓冲区里面,并返回字符串缓冲区本身


```java
	public static void demo2() {					//添加,在尾部追加
		StringBuffer sb = new StringBuffer();
		StringBuffer sb2 = sb.append(true);
		StringBuffer sb3 = sb.append(123);
		StringBuffer sb4 = sb.append("大家好");	//四个引用指向的是同一个对象

   /*
    指向同一个对象，输出都相同
    true123大家好
     */
		
		System.out.println(sb);					//调用了StringBuffer的toString方法
		System.out.println(sb2);
		System.out.println(sb3);
		System.out.println(sb4);

 
    /*
    指向同一个对象。
     */

		Person p1 = new Person("张三", 23);
		Person p2 = p1;
		Person p3 = p1;
		p2.setName("李四");
		p3.setAge(24);
		
    /*
    输出都相同
    李四，24
     */
		System.out.println(p1);
		System.out.println(p2);
		System.out.println(p3);
	}
```

```java
/*
在索引是1的地方添加， 原本是索引1和之后的的值，向后移动
*/
public static void demo3() {					//添加
		StringBuffer sb = new StringBuffer("itcast");
		sb.insert(1, true);							//在指定位置添加
		System.out.println(sb);
  
  // out: itruetcast
  }
```



###  StringBuffer的删除功能

- A:StringBuffer的删除功能
  - public StringBuffer deleteCharAt(int index):
    - 删除指定位置的字符，并返回本身
  - public StringBuffer delete(int start,int end):
    - 删除从指定位置开始指定位置结束的内容，并返回本身

```java
	public static void demo4() {
		StringBuffer sb = new StringBuffer("itcast");
//		sb.deleteCharAt(1);						//删除指定索引位置的字符
//		System.out.println(sb);
		
//		sb.delete(1, 3);							//删除,包含头不包含尾
		sb.delete(0, sb.length());				//清空缓冲区(比较好)
		sb = new StringBuffer();					//重新创建对象
		System.out.println(sb);
	}
```



###  StringBuffer的替换和反转功能

- A:StringBuffer的替换功能
  - public StringBuffer replace(int start,int end,String str):
    - 从start开始到end用str替换
- B:StringBuffer的反转功能
  - public StringBuffer reverse():
    - 字符串反转

```java
public static void demo5() {					//替换和反转
		StringBuffer sb = new StringBuffer("画上荷花花和尚画");
		sb.replace(2, 4, "oo");					//替换 包含头不包含尾
		System.out.println(sb);
		
		sb.reverse();								//反转,将缓冲区的字符反转
		System.out.println(sb);
	}
```





###  StringBuffer的截取功能及注意事项

- A:StringBuffer的截取功能
  - public String substring(int start):
    - 从指定位置截取到末尾
  - public String substring(int start,int end):
    - 截取从指定位置开始到结束位置，包括开始位置，不包括结束位置
- B:注意事项
  - 注意:返回值类型不再是StringBuffer本身(返回值是String对象)



```java
public static void demo6() {
		StringBuffer sb = new StringBuffer("itcast");
//		String s = sb.substring(1);
		
//		System.out.println(s); //tcast
		String s = sb.substring(2, 4);				//截取字符串,包含头,不包含尾 ctrl + 1可以根据右边的方法或变量生成左边的返回值类型和对象名
		System.out.println(s);  // ca
		System.out.println(sb);// itcast 说明缓冲区仍然存在
	}
```



###  StringBuffer和String的相互转换

- A:String -- StringBuffer
  - a:通过构造方法
  - b:通过append()方法
- B:StringBuffer -- String
  - a:通过构造方法
  - b:通过toString()方法
  - c:通过subString(0,length);

```java
package cn.itcast.stringbuffer;

public class Demo2_StringBuffer {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		//demo1();
    demo2();

  }

  private static void demo2() {
    //StringBuffer ---> String
    StringBuffer sb = new StringBuffer("itcast");
    String s1 = sb.substring(0, sb.length());			//将StringBuffer对象转换成String对象
    String s2 = sb.toString();							//将StringBuffer对象转换成String对象
    String s3 = new String(sb);							//将StringBuffer对象转换成String对象
  }

  public static void demo1() {
		//String ---> StringBuffer
		StringBuffer sb = new StringBuffer("itcast");		//将String对象转换成StringBuffer对象
		
		StringBuffer sb2 = new StringBuffer();
		sb2.append("itcast");								//将String对象转换成StringBuffer对象
	}

}
```



### 把数组转成字符串

- A:案例演示

  - 需求：把数组中的数据按照指定个格式拼接成一个字符串

  - 举例：


          int[] arr = {1,2,3};    
         // 输出结果：
          "[1, 2, 3]"
    
      用StringBuffer的功能实现


```java
package cn.itcat.test;

public class Test1_StringBuffer {

	/**
	 * A:案例演示
	* 需求：把数组中的数据按照指定个格式拼接成一个字符串
	* 
			举例：
				int[] arr = {1,2,3};	
			输出结果：
				"[1, 2, 3]"
				
			用StringBuffer的功能实现

	 */
	public static void main(String[] args) {
		int[] arr = {1,2,3};
		System.out.println(arr2String(arr)); 	//alt+/也可以帮我们完成输出语句
	}

	/*
	 * 明确返回值类型,String
	 * 明确参数列表,int[] arr
	 * [1, 2, 3]
	 */
	
	/*public static String arr2String(int[] arr) {
		StringBuffer sb = new StringBuffer();				//创建一个StringBuffer对象
		sb.append("[");										//向sb对象里添加[
		
		for (int i = 0; i < arr.length; i++) {				//遍历数组
			if(i == arr.length - 1) {						//如果遍历到最后一个索引
				sb.append(arr[i]).append("]");				//将数组中最后一个元素和]添加到缓冲区
			}else {											//如果不是最后一个个索引
				sb.append(arr[i]).append(", ");				//将元素和逗号空格添加到缓冲区
			}
		}
		return sb.toString();								//将StringBuffer对象转换成String对象返回
	}*/
	
  
  // 产生了大量的垃圾，因为创建了N多的对象，数组越大垃圾越多，所以应该使用stringBuffer来做。
	public static String arr2String(int[] arr) {
		String temp = "[";
		for (int i = 0; i < arr.length; i++) {
			if(i == arr.length - 1) {
				temp = temp + arr[i] + "]";
			}else {
				temp = temp + arr[i] + ", ";	//
			}
		}
		return temp;
	}
}
```





###  字符串反转

- A:案例演示

- 需求：把字符串反转

```
   举例：键盘录入"abc"
    输出结果："cba"
    用StringBuffer的功能实现  
```

```java
package cn.itcat.test;

import java.util.Scanner;

public class Test2_StringBuffer {

 /**
  *  A:案例演示
* 
  需求：把字符串反转
   举例：键盘录入"abc"  
   输出结果："cba"
   
  用StringBuffer的功能实现 
  */
 public static void main(String[] args) {
  Scanner sc = new Scanner(System.in);
  System.out.println("请输入一个字符串");
  String line = sc.nextLine();    //将键盘录入的字符串存储在line中
  
  StringBuffer sb = new StringBuffer(line);  //将字符串转换成StringBuffer对象
  sb.reverse();         //反转
  
  line = sb.toString();       //转换成字符串
  System.out.println(line);
 }

}
```


###  StringBuffer和StringBuilder的区别（面试题）

- A:StringBuilder的概述
  - 通过查看API了解一下StringBuilder类
- B:面试题
  - String,StringBuffer,StringBuilder的区别
  - StringBuffer和StringBuilder的区别
    1. StringBuffer是jdk1.0版本的,是线程安全的,效率低
    2. StringBuilder是jdk1.5版本的,是线程不安全的,效率高（单线程run的时候，不考虑线程安全的情况下，优先选择StringBuilder）
  - String和StringBuffer,StringBuilder的区别
    1. String是一个不可变的字符序列
    2. StringBuffer,StringBuilder是可变的字符序列

### String和StringBuffer分别作为参数传递

- A:形式参数问题
  - String作为参数传递
  - StringBuffer作为参数传递 
- B:案例演示
  - String和StringBuffer分别作为参数传递问题

```java
package cn.itcast.stringbuffer;

public class Demo3_StringBuffer {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		String s1 = "abc";
		StringBuffer sb = new StringBuffer();
		sb.append("itcast");
		
		change(s1);
		System.out.println(s1);

//		change(sb);
//		System.out.println(sb);
	}
	
	public static void change(String s) {		//String不会改变原有对象的值,一旦被初始化就不会被改变
		s = s + "def";							//s = "abcdef";
                              //s和s1 指向的是不同的两个引用 
    System.out.println(s);
  }
	
	public static void change(StringBuffer sb) {//StringBuffer会改变原有对象的值
		sb.append("heima");
	}

}

```



###  数组高级冒泡排序原理图解

- A:画图演示

- 需求：

  ```
        数组元素：{24, 69, 80, 57, 13}
        请对数组元素进行排序。
        
        冒泡排序
            相邻元素两两比较，大的往后放，第一次完毕，最大值出现在了最大索引处
  ```



![](http://ogtmd8elu.bkt.clouddn.com/201707231148_976.png)

###  数组高级冒泡排序代码实现

- A:案例演示
  - 数组高级冒泡排序代码

```java
package cn.itcast.arrays;

public class Demo1_BubbleSort {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		int[] arr = {33,11,44,22,55,66,123,-1,-34};//11,22,33,44,55
//		bubbleSort(arr);
//		System.out.println(arr2String(arr));

    ArrayLee(arr);
    System.out.println(arr2String(arr));
  }

	/*
	 * 冒泡排序
	 * 返回值类型void
	 * 参数列表int[] arr
	 */
	
	public static void bubbleSort(int[] arr) {			//arr.length = 6
		for (int i = 0; i < arr.length - 1; i++) {
			for (int j = 0; j < arr.length - 1 - i; j++) {//-1防止索引越界
				if(arr[j] > arr[j+1]) {					//-i是为了提高效率
					/*int temp = arr[j];
					arr[j] = arr[j+1];
					arr[j+1] = temp;*/
					swap(arr,j,j+1);					//交换两个索引对应的元素值
				}
			}
		}
	}
	
	/*
	 * 打印数组
	 * 1,返回值类型String
	 * 2,参数列表int[] arr
	 */
	public static String arr2String(int[] arr) {
		StringBuilder sb = new StringBuilder();
		sb.append("[");
		for (int i = 0; i < arr.length; i++) {
			if(i == arr.length - 1) {
				sb.append(arr[i]).append("]");
			}else {
				sb.append(arr[i]).append(", ");
			}
		}
		
		return sb.toString();
	}
	/*
	 * 交换数组中的位置
	 * 1,返回值类型void
	 * 2,参数列表int[] arr,int i,int j
	 */
	
	public static void swap(int[] arr,int i,int j) {
		int temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp;
	}

	 public  static void  ArrayLee(int[] arr){
     for (int i = 0; i < arr.length-1; i++) {//六个数据，比五次就可以了。

       for (int j = 0; j < arr.length-1-i; j++) { //防止数组越界
           int temp;
         if (arr[j]>arr[j+1]) {
           temp=arr[j];
               arr[j]=arr[j+1];
           arr[j+1]=temp;

         }

       }

     }
   }
}

```



###  数组高级选择排序原理图解

- A:画图演示
  - 需求：
    - 数组元素：{24, 69, 80, 57, 13}
    - 请对数组元素进行排序。
    - 选择排序
      - 从0索引开始，依次和后面元素比较，小的往前放，第一次完毕，最小值出现在了最小索引处

![](http://ogtmd8elu.bkt.clouddn.com/201707231356_739.png)

###  数组高级选择排序代码实现

- A:案例演示
  - 数组高级选择排序代码

```java
package cn.itcast.arrays;

public class Demo2_SelectSort {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		int[] arr = {66,55,44,33,22,11};
		selectSort(arr);										//升序排列
		System.out.println(Demo1_BubbleSort.arr2String(arr));	//将数组转换成字符串并打印
	}
	
	/*
	 * 选择排序
	 * 1,void
	 * 2,int[] arr
	 * 	第一次:arr[0]分别与arr[1-5]比较5次
		第二次:arr[1]分别与arr[2-5]比较4次
		第三次:arr[2]分别与arr[3-5]比较3次
		第四次:arr[3]分别与arr[4-5]比较2次
		第五次:arr[4]与arr[5]比较1次
	 */
	public static void selectSort(int[] arr) {				//arr.length = 6
		for (int i = 0; i < arr.length - 1; i++) {
			for (int j = i + 1; j < arr.length; j++) {
				if(arr[i] > arr[j]) {
					/*int temp = arr[i];
					arr[i] = arr[j];
					arr[j] = temp;*/
					Demo1_BubbleSo rt.swap(arr, i, j);		//调用Demo1_BubbleSort类静态的换位方法
				}
			}
		}
	}
}

```



```java
public static void swap(int[] arr,int i,int j) {
		int temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp;
	}
```





###  数组高级二分查找原理图解

- A:画图演示
  - 二分查找  
  - 前提：数组元素有序

![](http://ogtmd8elu.bkt.clouddn.com/201707231732_491.png)

###  数组高级二分查找代码实现及注意事项

- A:案例演示
  - 数组高级二分查找代码
- B:注意事项
  - 如果数组无序，就不能使用二分查找。
    - 因为如果你排序了，但是你排序的时候已经改变了我最原始的元素索引。



````java
package cn.itcast.arrays;

public class Demo3_HalfSearch {

	/**
	 * @param args
	 * 二分查找法
	 */
	public static void main(String[] args) {
		int[] arr = {11,22,33,44,55,66,77};
		System.out.println(halfSearch(arr, 22));
		System.out.println(halfSearch(arr, 66));
		System.out.println(halfSearch(arr, 88));
	}

	/*
	 * 二分查找法
	 * 1,返回值类型int
	 * 2,参数列表int[] arr,int key
	 */
	
	public static int halfSearch(int[] arr,int key) {
		int min = 0;						//记录最小索引的值
		int max = arr.length - 1;			//记录最大索引的值
		int mid = (min + max) >> 1;			//等效于(min + max) / 2
		while(arr[mid] != key) {
			if(key < arr[mid]) {			//如果要找的数小于了中间索引对应的值
				max = mid - 1;				//最大值变
			}else if(key > arr[mid]) {		//如果要找的数大于了中间索引对应的值
				min = mid + 1;				//最小值变
			}
			
			if(min > max) {
				return -1;
			}
			
			mid = (min + max) >> 1;			//无论最大值变还是最小值变,中间的索引都要重新求值
		}
		return mid;
	}
}

````



###  Arrays类的概述和方法使用

- A:Arrays类概述
  - 针对数组进行操作的工具类。
  - 提供了排序，查找等功能。
- B:成员方法
  - public static String toString(int[] a)
  - public static void sort(int[] a)
  - public static int binarySearch(int[] a,int key)

```java
package cn.itcast.arrays;

import java.util.Arrays;

public class Demo4_Arrays {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		//int[] arr = {};
		//Arrays.sort(arr);
		//System.out.println(Arrays.toString(arr));
		
		int[] arr2 = {11,22,33,44,55,66,77};
		System.out.println(Arrays.binarySearch(arr2, 22));
		System.out.println(Arrays.binarySearch(arr2, 66));
		System.out.println(Arrays.binarySearch(arr2, 88));
		System.out.println(Arrays.binarySearch(arr2, 9));
		System.out.println(Arrays.binarySearch(arr2, 24));
	}
	
	/*
	 * public static String toString(int[] a) {	//{1,2,3}
        if (a == null)							//a = null
            return "null";						//直接返回null
        int iMax = a.length - 1;				//记录住最大索引值
        if (iMax == -1)							//如果数组为空
            return "[]";						//返回[]

        StringBuilder b = new StringBuilder();	//创建StringBuilder对象
        b.append('[');							//添加[
        for (int i = 0; ; i++) {				//[1, 2, 3]
            b.append(a[i]);
            if (i == iMax)
                return b.append(']').toString();
            b.append(", ");
        }
    }
	 */
}

```





###  基本类型包装类的概述

- A:为什么会有基本类型包装类

  - 将基本数据类型封装成对象的好处在于可以在对象中定义更多的功能方法操作该数据。

- B:常用操作

  - 常用的操作之一：用于基本数据类型与字符串之间的转换。

- C:基本类型和包装类的对应

  - byte 		Byte

  ```
    short           Short
    int             Integer
    long            Long
    float           Float
    double          Double
    char            Character
    boolean         Boolean
  ```


  

###  Integer类的概述和构造方法

- A:Integer类概述
  - 通过JDK提供的API，查看Integer类的说明
  - Integer 类在对象中包装了一个基本类型 int 的值,
  - 该类提供了多个方法，能在 int 类型和 String 类型之间互相转换，
  - 还提供了处理 int 类型时非常有用的其他一些常量和方法
- B:构造方法
  - public Integer(int value)
  - public Integer(String s)
- C:案例演示
  - 使用构造方法创建对象

```java
public static void demo2() {
		Integer i = new Integer(100);			//将一个int数转换成Integer的对象
		Integer i2 = new Integer("100");		//将数字字符串转换成Integer对象
	}

	public static void demo1() {
		Scanner sc = new Scanner(System.in);
		System.out.println("请输入:");
		//int x = sc.nextInt();								//如果录入的是非int数,会抛异常
		String line = sc.nextLine();						//录入的所有元素都能存储在line中
															//可以对line进行操作
		int x = Integer.parseInt(line);						//将一个数字字符串转换成数字
		
		System.out.println(x);
	}
```





###  String和int类型的相互转换

- A:int -- String
  - a:和""进行拼接
  - b:public static String valueOf(int i)
  - c:int -- Integer -- String(Integer类的toString方法())
  - d:public static String toString(int i)(Integer类的静态方法)
- B:String -- int
  - a:String -- Integer -- int
    - public static int parseInt(String s)

```java
public static void demo4() {
		String s = "100";
		byte b = Byte.parseByte(s);
		short sh = Short.parseShort(s);
		int i = Integer.parseInt(s);
		long l = Long.parseLong(s);
		float f = Float.parseFloat(s);
		double d = Double.parseDouble(s);
		
		String s2 = "true";
		boolean b2 = Boolean.parseBoolean(s2);
		String s3 = "我不爱北京";					//只有Character中没有parseXxx方法,因为字符串是由多个字符组成
												//转换的时候char只能接收一个字符,字符串转成字符有toCharArray()方法
	}

public static void demo3() {
		//int ---> String
		int num = 100;
		String x = num + "";					//任何数据与字符串用+连接都会产生新的字符串
		String y = String.valueOf(num);			//valueOf可以将任何数据转换成字符串
	}
```





###  JDK5的新特性自动装箱和拆箱

- A:JDK5的新特性
  - 自动装箱：把基本类型转换为包装类类型
  - 自动拆箱：把包装类类型转换为基本类型
- B:案例演示
  - JDK5的新特性自动装箱和拆箱
  - Integer ii = 100;
  - ii += 200;
- C:注意事项
  - 在使用时，Integer  x = null;代码就会出现NullPointerException。
  - 建议先判断是否为null，然后再使用。

```java
private static void demo5() {
    int x = 10;
    Integer i = new Integer(x);				//1.5版本之前将x变成对象

    Integer i2 = x;							//自动装箱,把基本数据类型变成对象
    x = i2.intValue();						//手动将Integer对象转换成基本数据类型
    int y = i2 + 20;						//自动拆箱,把对象变成基本数据类型
  }
```



###  Integer的面试题

- A:Integer的面试题

  - 看程序写结果
  ```
    Integer i1 = new Integer(97);
    Integer i2 = new Integer(97);
    System.out.println(i1 == i2);
    System.out.println(i1.equals(i2));
    System.out.println("-----------");
    
    Integer i3 = new Integer(197);
    Integer i4 = new Integer(197);
    System.out.println(i3 == i4);
    System.out.println(i3.equals(i4));
    System.out.println("-----------");
    
    Integer i5 = 97;
    Integer i6 = 97;
    System.out.println(i5 == i6);
    System.out.println(i5.equals(i6));
    System.out.println("-----------");
    
    Integer i7 = 197;
    Integer i8 = 197;
    System.out.println(i7 == i8);
    System.out.println(i7.equals(i8));
  ```






```java
package cn.itcat.test;

public class Test3_Integer {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		Integer i1 = new Integer(97);
		Integer i2 = new Integer(97);
		System.out.println(i1 == i2);			//false
		System.out.println(i1.equals(i2));		//true
		System.out.println("-----------");
	
		Integer i3 = new Integer(197);
		Integer i4 = new Integer(197);
		System.out.println(i3 == i4);			//false
		System.out.println(i3.equals(i4));		//true
		System.out.println("-----------");
	
		Integer i5 = -128;
		Integer i6 = -128;
		System.out.println(i5 == i6);			//?  true
		System.out.println(i5.equals(i6));		//true
		System.out.println("-----------");
	
		Integer i7 = 128;
		Integer i8 = 128;
		System.out.println(i7 == i8);			//? false
		System.out.println(i7.equals(i8));		//true
		
		/*
		 * 当Integer直接赋值的范围是在-128到127之间,也就是一个byte的取值范围,不会创建多个对象
		 * 第一次Integer = -128,会在底层有一个数组存储-128到127之间的Integer对象值,直接从数组中获取Integer对象
		 * 第二次Integer = -128,不会重新创建对象,继续从数组中获取
		 * 
		 * == 表示两个引用句柄指向同一个对象，地址相同
		 * 
		 */
		
		Integer[] arr = {new Integer(-128),127,-126};	//数组中可以存储引用数据类型
	}

}

```



### 作业

- String s = "80 100 60 40 30 10 20";
- 要求对字符串中的int数排序,最后在转换成字符串
- split(" ")切割
