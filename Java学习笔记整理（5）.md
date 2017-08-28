---
title: Java学习笔记整理（5）
date: 2017-08-01 08:32:47
tags: [J2SE]
categories: [J2SE]
top: 5
---

---



<!--more-->



### 数组概述和定义格式说明
* A:为什么要有数组(容器)
* B:数组概念
* C:数组定义格式
  数据类型[] 数组名 = new 数据类型[数组的长度];

数组本质上是一种容器，用于装一些数据类型一致的数据 。既可以存一下基本数据类型（8种），也可以存一些引用数据类型。




### 数组的初始化动态初始化
* A:什么是数组的初始化
* B:如何对数组进行初始化
  * a:动态初始化 只指定长度，系统为赋值
    * int[] arr = new int[5]; //未赋值	
    * b:静态初始化 给出初始化值，由系统决定长度
    * int[] arr = new int[]{1,2,3,4,5}; 
    * int[] arr = {1,2,3,4,5};
* C:动态初始化的格式：
  * 数据类型[] 数组名 = new 数据类型[数组长度];
* D:案例演示
  * 对数组的解释
  * 输出数组名称和数组元素

数组的初始化就是要为数组中的数组元素分配内存空间，并为每个元素赋值

```java
class Demo1Array {
	public static void main(String[] args) {
		//声明数组
		//int[] arr1;							//数据类型[] 数组名; 推荐
		//int arr2[];							//数据类型 数组名[];

		//动态初始化,没有给数组赋具体的值
		int[] arr = new int[5];					//new是一个关键字,用来创建实体对象
												//5代表数组的长度

		arr[0] = 5;

		arr[4] = 10;
		
		//静态初始化
		int[] arr2 = new int[]{1,2,3};			//静态初始化,不用知道数组长度,长度根据元素个数

	}
}
```

### Java中的内存分配以及栈和堆的区别
* A:栈
* B:堆
* C:方法区
* D:本地方法区
* E:寄存器

![](http://ogtmd8elu.bkt.clouddn.com/201707071636_7.png)

### 数组的内存图解1一个数组
* A:画图演示
  * 一个数组
    ![](http://ogtmd8elu.bkt.clouddn.com/201707071657_309.png)

main方法是第一个入栈的，因为它是程序最先执行的方法并且有权力去调用其它的方法，new出来的对象，数据，放在堆里面，堆会开辟出内存的空间并初始化值char类型初始化的0x0000，是因为一个char占两个字节，16位，全部初始化成了0.0x0011是地址的编号，根据地址编号找到数据。

栈内存里面存储的变量必须赋值（否则报错），堆里面的不是必须赋值，因为系统在划分内存空间的时候已经赋了初始值。
​	
### 数组的内存图解2二个数组
* A:画图演示
  * 二个不同的数组


![](http://ogtmd8elu.bkt.clouddn.com/201707311851_41.png)


### 数组的内存图解3三个数组
* A:画图演示
  * 三个数组，有两个数组的引用指向同一个地址

![](http://ogtmd8elu.bkt.clouddn.com/201707311854_241.png)



new了几次就是在堆上创建了几个内存空间。

```java
int [] arr3 =arr2; //实际上是句柄（arr3），指向了arr2存储空间，通过赋给同一个地址来找到数据
arr3=null;
arr2=null;
//当都执行arr2,arr3=null的时候，没有任何句柄（引用）指向堆内存的对象的时候，堆里面的对象就会变成垃圾被被回收。
//当程序执行完以后，主方法弹栈，不存在回收的说法，用完就结束，等待下一个程序的main方法入栈。
```



### 数组的初始化静态初始化及内存图
* A:静态初始化的格式：
  * 格式：数据类型[] 数组名 = new 数据类型[]{元素1,元素2,…};
  * 简化格式：
    * 数据类型[] 数组名 = {元素1,元素2,…};
* B:案例演示
  * 对数组的解释
  * 输出数组名称和数组元素
* C:画图演示
  * 一个数组

```java
class Demo3Array {
	public static void main(String[] args) {
		//int[] arr1 = new int[]{1,2,3,4,5};		//静态初始化,一初始化就赋值,根据值的个数决定长度
		//int[] arr2 = {1,2,3,4,5};					//静态初始化的简写形式,只能在一行定义

		int[] arr1;
		arr1 = new int[]{1,2,3,4,5};

		/*
		int[] arr2;									//这种静态初始化不允许先声明后赋值
		arr2 = {1,2,3,4,5};                         //非法的
		
		*/
		
	}
}

```





![](http://ogtmd8elu.bkt.clouddn.com/201707311907_157.png)

静态数组初始化的时候，会给一个默认值，int的就默认为0，String 默认为null，然后再进行赋值。

### 数组操作的两个常见小问题越界和空指针

* A:案例演示
  * a:ArrayIndexOutOfBoundsException:数组索引越界异常
    * 原因：你访问了不存在的索引。
  * b:NullPointerException:空指针异常
    * 原因：数组已经不在指向堆内存了。而你还用数组名去访问元素。
    * int[] arr = {1,2,3};
    * arr = null;
    * System.out.println(arr[0]);

```java
class Demo4Array {
	public static void main(String[] args) {
		int[] arr1 = {1,2,3,4,5};
		//System.out.println(arr1[-1]);			//ArrayIndexOutOfBoundsException:数组索引越界异常。
                                                //定义的组数里面不存在这个索引
		System.out.println(arr1);				//[I@1afb7ac7 [代表以一维数组，I代表int地址，1afb7ac7代表一个地址值
		arr1 = null;							//将数组赋值为null
		System.out.println(arr1[0]);			//NullPointerException:空指针异常
      //句柄和实体之间的地址值被赋为了null，但又调用了数组，就会出现空指针异常。
      //System.out.println(arr1[]); 输出为null
	}
}

```



### 数组的操作1遍历
* A:案例演示
  * 数组遍历：就是依次输出数组中的每一个元素。
  * 数组的属性:arr.length数组的长度
  * 数组的最大索引:arr.length - 1;

```java
class Demo5Array {
	public static void main(String[] args) {
		int[] arr = {1,2,3,4,5};

		System.out.println(arr.length);			//数组名.length代表数组的长度
		/*for (int x = 0;x < 5 ;x++ ) {
			System.out.print(arr[x] + " ");
		}*/

		//arr.length;

		for (int x = 0;x < arr.length ;x++ ) {	//一维数组的遍历操作
			System.out.print(arr[x] + " ");
		}
	}
}
```

### 数组的操作2获取最值

* A:案例演示
  * 数组获取最值(获取数组中的最大值最小值)



```java
class Demo6Array {
	public static void main(String[] args) {
		int[] arr = {44,55,66,33,77,11};
		int max = getMax(arr);
		System.out.println(max);
	}

	/*
	获取数组中的最大值
	1,明确返回值类型,int
	2,明确参数列表,int[] arr
	*/

	public static int getMax(int[] arr) {
		int max = arr[0];								//定义变量记录住第一个位置的值

		for (int x = 1;x < arr.length ;x++ ) {			//遍历数组,从第二个位置
			if (arr[x] > max) {							//与max中的值比较,如果比max值大
				max = arr[x];							//将max中的值替换掉
			}
		}

		return max;										//将最大值返回
	}
}
```

### 数组的操作3反转
* A:案例演示
  * 数组元素反转(就是把元素对调)

```java
class Demo7Array {
	public static void main(String[] args) {
		int[] arr = {11,22,33,44,55};					//0x0088

		/*for (int x = 0;x < arr.length ;x++ ) {			//正序遍历
			System.out.println(arr[x]);
		}

		for (int x = arr.length - 1;x >= 0 ;x-- ) {		//倒序遍历
			System.out.println(arr[x]);
		}*/

		revArray(arr);									//对数组反转操作
		print(arr);										//遍历打印数组
	}

	/*
	将数组反转
	1,返回值类型void
	2,参数列表,int[] arr
	*/

	public static void revArray(int[] arr) {			//arr = 0x0088
		for (int x = 0;x < arr.length / 2 ; x++) {
			int temp = arr[x];
			arr[x] = arr[arr.length-1-x];
			arr[arr.length-1-x] = temp;
		}
	}													//55,44,33,22,11

	/*
	将数组打印
	1,返回值类型void
	2,参数列表,int[] arr
	*/

	public static void print(int[] arr) {
		for (int x = 0;x < arr.length ;x++ ) {
			System.out.print(arr[x] + " ");
		}
	}
}

```



![](http://ogtmd8elu.bkt.clouddn.com/201707311940_10.png)







### 数组的操作4查表法

* A:案例演示
  * 数组查表法(根据键盘录入索引,查找对应星期)

```java
import java.util.Scanner;
class Demo8Array {
	/*
	数组查表法(根据键盘录入索引,查找对应星期)
	*/
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);				//创建键盘录入对象
		System.out.println("请输入对应的星期,输入的数值在1-7之间:");
		int week = sc.nextInt();
		char c = getWeek(week);
		System.out.println("今天是星期" + c);
	}

	/*
	获取星期对应的值
	1,明确返回值类型char
	2,明确参数列表int week
	*/

	public static char getWeek(int week) {					//传入想要获取的星期
		char[] arr = {' ','一','二','三','四','五','六','日'};//定义表
		return arr[week];									//返回对应的星期
	}
}

```

### 数组的操作5基本查找

* A:案例演示
  * 数组元素查找(查找指定元素第一次在数组中出现的索引)



```java
class Demo9Array {
	// 数组元素查找(查找指定元素第一次在数组中出现的索引)
	public static void main(String[] args) {
		int[] arr = {44,33,55,22,11,66};
		int index = getIndex(22,arr);
		System.out.println(index);
	}

	/*
	根据数组元素获取数组元素的索引
	1,明确返回值类型int
	2,明确参数列表,int key,int[] arr
	*/

	public static int getIndex(int key,int[] arr) {
		for (int x = 0;x < arr.length ;x++ ) {
			if (key == arr[x]) {
				return x;
			}
		}

		return -1;

	}
}

```

### 二维数组概述和格式1的讲解
* A:二维数组概述
* B:二维数组格式1
  * int[][] arr = new int[3][2]; 
* C:二维数组格式1的解释
* D:注意事项
  * a:以下格式也可以表示二维数组
    * 1:数据类型 数组名[][] = new 数据类型[m][n];
    * 2:数据类型[] 数组名[] = new 数据类型[m][n];
  * B:注意下面定义的区别

```java
    int x;
 	int y;
 	int x,y;
 	
 	int[] x; //一维数组
 	int[] y[]; //二维数组
 	
 	int[] x,y[];	//x是一维数组,y是二维数组
```
* E:案例演示
  * 定义二维数组，输出二维数组名称，一维数组名称，一个元素


```java
class Demo1Array {
	public static void main(String[] args) {
		int x = 10;
		int[] arr1 = new int[5];			//一维数组
		int[][] arr2 = new int[5][3];		//二维数组
		/*
		二维数组
		这是一个二维数组
		这个二维数组中有五个一维数组
		每个一维数组中有三个元素
		*/
		int[][][] arr3 = new int[3][4][5];
		/*
		这是一个三维数组
		这个三维数组中有3个二维数组
		每个二维数组中有4个一维数组
		每个一维数组中有5个元素
		*/

		/*
		* 1:数据类型 数组名[][] = new 数据类型[m][n];
		* 2:数据类型[] 数组名[] = new 数据类型[m][n];
		*/

		int arr4 [][] = new int[3][4];
		int[] arr5 [] = new int[5][6];
	}
}

```



```java
class Demo2Array {
	public static void main(String[] args) {
		int[][] arr1 = new int[3][2];

		System.out.println(arr1);				//代表二维数组 //存了一个地址值
		System.out.println(arr1[0]);			//代表二维数组中第一个一维数组 /存了一个地址
		System.out.println(arr1[0][0]);			//代表二维数组中第一个一维数组中的第一个元素
	}
}
```



| a00  | a01  |
| :--: | :--: |
| a10  | a11  |
| a20  | a21  |

### 二维数组格式1的内存图解
* A:画图演示
  * 画图讲解上面的二维数组名称，一维数组名称，一个元素的值的问题

一维数组，是引用数组类型。

![](http://ogtmd8elu.bkt.clouddn.com/201707312021_285.png)

**注意：**

一维数组本身就是一种引用类型。

二维数组int [3][2]的形式，相当于创建了三个元素为2的一维数组。

通过给地址的方法，给句柄赋值0x0011 找到三个一维数组的存储位置，再通过三个以为数组里面存储的地址找到定位到每个元素的值。

### 二维数组格式2的讲解及其内存图解
* A:二维数组格式2
  * int[][] arr = new int[3][];没指定一维数组长度
* B:二维数组格式2的解释
* C:案例演示
  * 讲解格式，输出数据，并画内存图

```java
class Demo3Array {
	public static void main(String[] args) {
		//int[][] arr = new int[3][2];				//第一种格式
		int[][] arr = new int[3][];					//第二种格式
		arr[0] = new int[5]; //初始化一维数组的长度，更加灵活
		arr[1] = new int[6];
		arr[2] = new int[7];
		System.out.println(arr[0]);//打印出地址值
	}
}

```

![](http://ogtmd8elu.bkt.clouddn.com/201707312051_209.png)



### 二维数组格式3的讲解及其内存图解

* A:二维数组格式3;

  ```
  int arr ={{1,2,3},{4,5},{6,7,8,9}}; 
  ```

* B:二维数组格式3的解释

* C:案例演示
  * 讲解格式，输出数据，并画内存图



![](http://ogtmd8elu.bkt.clouddn.com/201707312321_980.png)



### 二维数组练习1遍历

* A:案例演示
  * 需求：二维数组遍历
  * 外循环控制的是二维数组的长度，其实就是一维数组的个数。
  * 内循环控制的是一维数组的长度。

```java
class Demo4Array {
	public static void main(String[] args) {
		int[][] arr = {{1,2,3},{4,5},{6,7,8,9}};		//二维数组的第三种格式

		//System.out.println(arr.length);				//代表一维数组的个数
		//二维数组的遍历
		for (int x = 0;x < arr.length ;x++ ) {			//外层循环获取到每一个一维数组
			//arr[x]  arr[0] arr[1] arr[2]
			for (int y = 0;y < arr[x].length ;y++ ) {
				System.out.print(arr[x][y] + " ");
			}

			System.out.println();
		}
	}
}

```



### 二维数组练习2求和
* A:案例演示
* 
   需求：公司年销售额求和
   	某公司按照季度和月份统计的数据如下：单位(万元)
   	第一季度：22,66,44
   	第二季度：77,33,88
   	第三季度：25,45,65
   	第四季度：11,66,99



```java
class Test1Array {
	/*
	需求：公司年销售额求和
		某公司按照季度和月份统计的数据如下：单位(万元)
		第一季度：22,66,44
		第二季度：77,33,88
		第三季度：25,45,65
		第四季度：11,66,99
	*/
	public static void main(String[] args) {
		int[][] arr = {{22,66,44},{77,33,88},{25,45,65},{11,66,99}};
		
		int sum = 0;									//声明一个变量,进行累加操作
		for (int x = 0;x < arr.length ;x++ ) {
			for (int y = 0;y < arr[x].length ;y++ ) {
				sum = sum + arr[x][y];
			}
		}

		System.out.println(sum);
	}
}
```

### 思考题Java中的参数传递问题及图解

* A:案例演示
* 
   看程序写结果，并画内存图解释
   ```java
   public static void main(String[] args) {
   	int a = 10;
   	int b = 20;
   	System.out.println("a:"+a+",b:"+b);
   	change(a,b);
   	System.out.println("a:"+a+",b:"+b);

   	int[] arr = {1,2,3,4,5};
   	change(arr);
   	System.out.println(arr[1]);
   }

   public static void change(int a,int b) {
   	System.out.println("a:"+a+",b:"+b);
   	a = b;
   	b = a + b;
   	System.out.println("a:"+a+",b:"+b);
   }

   public static void change(int[] arr) {
   	for(int x=0; x<arr.length; x++) {
   		if(arr[x]%2==0) {
   			arr[x]*=2;
   		}
   	}
   }
   ```

