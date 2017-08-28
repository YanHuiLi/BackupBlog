---
title: Java基础笔记整理（3）
date: 2017-07-05 16:41:19
tags: [逻辑运算符,位运算符,if语句,switch语句,三元运算符]
categories: J2SE
top: 3
---

---
1.逻辑运算符和位运算符的具体使用。
2.附带了常问的关于Java的异或面试题。
3.三元运算符的使用，以及和if语句的互转，及其使用的场合
4.顺序结构语句和选择结构语句的使用环境和适用场合。
5.if语句的三种形式介绍以及相关使用细节和注意事项。
6.if语句的嵌套使用。
7.if语句和switch语句的区别
<!--more-->


### 逻辑运算符的基本用法
* A:逻辑运算符有哪些
* B:案例演示
* 逻辑运算符的基本用法
* 注意事项：
  * a:逻辑运算符一般用于连接boolean类型的表达式或者值。
  * b:表达式：就是用运算符把常量或者变量连接起来的符合java语法的式子。
    * 算术表达式：a + b
    * 比较表达式：a == b(条件表达式)
* C:结论：
* &逻辑与:有false则false。
* |逻辑或:有true则true。
* ^逻辑异或:相同为false，不同为true。
* !逻辑非:非false则true，非true则false。
  * 特点：偶数个不改变本身。

  ![逻辑运算符](http://ogtmd8elu.bkt.clouddn.com/2017/07/05/3e51f1b984a28c1c0085bdb22ea0fcf0.png)

```java
class Demo1Operator {
	public static void main(String[] args) {
		//逻辑与
		int x = 10;
		//x == 5 & x == 15 
		//true & true = true
		//false & true = false
		//true & false = false
		//false & false = false
		//左右两边都为真的时候,结果才为真
		
		//逻辑或
		int y = 10;
		y > 10 | y == 10;
		//true | true = true
		//true | false = true
		//false | true = true
		//false | false = false
		//左右两边只要有一个为真,结果为真

		//逻辑异或
		//true ^ true = false
		//true ^ false = true
		//false ^ true = true
		//false ^ false = false
		//左右两边相同,结果为假,左右两边不同结果为真

		//逻辑非!
		!!true;
		!!false;
	        }
}
```

### 逻辑运算符&&和&的区别
* A:案例演示
  * &&和&的区别?(面试题)
    * a:最终结果一样。
    * b:&&具有短路效果。左边是false，右边不执行。
* B:同理||和|的区别?(学生自学)
* C:开发中常用谁?
  * &&,||,!

```java
class Demo2Operator {
	public static void main(String[] args) {
		//逻辑与和短路与
		//true & true = true
		//false & true = false
		//true & false = false
		//false & false = false
		//左右两边都为真的时候,结果才为真

		//true && true = true
		//false && true = false
		//true && false = false
		//false && false = false

		/*
		&和&&的区别(面试题)
		&无论左边是true还是false,右边都参与运算
		&&左边如果为false,右边不参与运算,左边如果为true,右边参与运算
		*/
		


		//逻辑或和短路或
		//true | true = true
		//true | false = true
		//false | true = true
		//false | false = false
		//左右两边只要有一个为真,结果为真

		//true || true = true
		//true || false = true
		//false || true = true
		//false || false = false
		/*
		|和||的区别
		|无论左边是true还是false,右边都参与运算
		||左边如果为true,右边不参与运算,左边如果为false,右边参与运算
		*/
		
	}
}
```


### 位运算符的基本用法1
* A:位运算符有哪些
* B:案例演示
  * 位运算符的基本用法1
  * &,|,^,~ 的用法
  * &:有0则0
  * |:有1则1
  * ^:相同则0，不同则1（异或）
  * ~:按位取反
    **0假1真**
    ![位运算符](http://ogtmd8elu.bkt.clouddn.com/wei-min.png)
```java
class Demo1Operator {
	public static void main(String[] args) {
		System.out.println(~6);
	}
}

/*
  110
 &011
---------
  010

  110
 |011
---------
  111

  110
 ^011
---------
  101
 ^011
---------
  110

  一个数被另一个数异或两次,结果是这个数本身

  00000000 00000000 00000000 00000110   +6的原码
  11111111 11111111 11111111 11111001   -7

  10000000 00000000 00000000 00000111   -7的原码
  11111111 11111111 11111111 11111000   -7的反码
  11111111 11111111 11111111 11111001   -7的补码

平时我们使用的手写出来的-6的二进制码称为原码，计算机在操作运算的时候，使用的是补码。
*/
```


### 位异或运算符的特点及面试题
* A:案例演示
  * 位异或运算符的特点
  * ^的特点：一个数据对另一个数据位异或两次，该数本身不变。

* B:面试题：
  * 请自己实现两个整数变量的交换
  * 注意：以后讲课的过程中，我没有明确指定数据的类型，默认int类型。
```java

//一个数被另一个数异或两次得到的就是这个数本身。

//交换两个变量中的值
//int x = 10; int y = 5;
//做法1 引入第三方变量
//做法2 不允许用第三方变量
class TestOperator {
	public static void main(String[] args) {
		int x = 10;
		int y = 5;
		/*int temp;

		temp = x;
		x = y;
		y = temp;
		System.out.println("x = " + x + ",y = " + y);

		x = x + y;		//10 + 5 = 15
		y = x - y;		//15 - 5 = 10
		x = x - y;		//15 - 10 = 5
		System.out.println("x = " + x + ",y = " + y);*/

		x = x ^ y;		// 10 ^ 5
		y = x ^ y;		// 10 ^ 5 ^ 5 = 10
		x = x ^ y;		// 10 ^ 5 ^ 10 = 5
		System.out.println("x = " + x + ",y = " + y);
	}
}
```

### 位运算符的基本用法2及面试题
* A:案例演示 >>,>>>,<<的用法:
  *  <<:左移	左边最高位丢弃，右边补齐0
    *  >>:右移最高位是0，左边补齐0;最高为是1，左边补齐1
  *  >>>:无符号右移 无论最高位是0还是1，左边补齐0
```java
class Demo2Operator {
	public static void main(String[] args) {
		System.out.println(-6 >> 1);			//向右移动几次就是除以2的几次幂
		//System.out.println(6 << 1);			//向左移动几次就是乘以2的几次幂
	/*
	>>>无符号右移和>>有符号右移的区别
	>>>无符号右移无论高位是0还是1,移动后都用0补位
	>>有符号右移高位是0就补0,高位是1就补1
	*/

	//面试题
	//请用最有效率的方式写出计算2乘以8的结果
		System.out.println(2 * 8);
		System.out.println(2 << 3);				//最有效率,直接操作的是二进制
	}
}
```
### 三元运算符的基本用法
* A:三元运算符的格式
  * 	(关系表达式) ? 表达式1 : 表达式2;
* B:三元运算符的执行流程 
* C:案例演示
  * 获取两个数中的最大值
```java
class Demo1Operator {
	public static void main(String[] args) {
		//格式(关系表达式)?表达式1：表达式2；
		int x = 10;
		int y = 5;
		int z = (x > y) ? x : y;	//如果条件为真,就把表达式1的值赋值过去,如果条件为假就把表达式2的值赋值过去

		System.out.println(z);
	}
}
```

### 三元运算符的练习
* A:案例演示
  * 比较两个整数是否相同
* B:案例演示
  * 获取三个整数中的最大值

```java
class Test1Operator {
	public static void main(String[] args) {
		/*
			A:案例演示
			比较两个整数是否相同
			B:案例演示
			获取三个整数中的最大值
		
		
		int x = 10;
		int y = 10;
		boolean b = (x == y)? true : false;
		boolean b2 = (x == y);

		System.out.println(b);*/

		int a = 10;
		int b = 20;
		int c = 30;

		int temp = (a > b) ? a : b;
		//System.out.println((temp > c) ? temp : c);
		int temp2 = (temp > c) ? temp : c;
	}
}
```


### 键盘录入的基本格式讲解 
* A:为什么要使用键盘录入数据
  * a:为了让程序的数据更符合开发的数据
  * b:让程序更灵活一下
* B:如何实现键盘录入呢
  * 先照格式来。
  * a:导包
    * 格式：
      * import java.util.Scanner; 
    * 位置：
      * 在class上面。
  * b:创建键盘录入对象
    * 格式：
      * Scanner sc = new Scanner(System.in);
    * c:通过对象获取数据
    * 格式：
      * int x = sc.nextInt();
* C:案例演示
  * 键盘录入1个整数，并输出到控制台。
  * 键盘录入2个整数，并输出到控制台。

```java

import java.util.Scanner;							//导入包

class Demo1Scanner {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);		//创建对象,键盘录入
		System.out.println("请输入第一个整数");
		int x = sc.nextInt();						//录入一个int类型的整数
		System.out.println(x);
		System.out.println("请输入第二个整数");
		int y = sc.nextInt();
		System.out.println(y);
	}
}
```


### 键盘录入的练习1
* A:案例演示
  * 键盘录入练习：键盘录入两个数据，并对这两个数据求和，输出其结果
* B:案例演示
  * 键盘录入练习：键盘录入两个数据，获取这两个数据中的最大值

```java
import java.util.Scanner;							//导入包
class Test1Scanner {
	public static void main(String[] args) {
		/*
		* A:案例演示
		* 键盘录入练习：键盘录入两个数据，并对这两个数据求和，输出其结果
		* B:案例演示
		* 键盘录入练习：键盘录入两个数据，获取这两个数据中的最大值
		*/
		Scanner sc = new Scanner(System.in);		//创建对象

		System.out.println("请输入第一个整数:");
		int x = sc.nextInt();
		System.out.println("请输入第二个整数:");
		int y = sc.nextInt();

		/*System.out.println("两个整数的和是:");
		int temp = x + y;
		System.out.println(temp);*/

		int result = (x > y) ? x : y;
		System.out.println("两个整数的最大值是:");
		System.out.println(result);
	}
}
```
### 键盘录入的练习2
* A:案例演示
  * 键盘录入练习：键盘录入两个数据，比较这两个数据是否相等
* B:案例演示
  * 键盘录入练习：键盘录入三个数据，获取这三个数据中的最大值


```java
import java.util.Scanner;							//导入包
class Test2Scanner {
	public static void main(String[] args) {
		/*
		* A:案例演示
		* 键盘录入练习：键盘录入两个数据，比较这两个数据是否相等
		* B:案例演示
		* 键盘录入练习：键盘录入三个数据，获取这三个数据中的最大值
		*/

		Scanner sc = new Scanner(System.in);		//创建对象

		System.out.println("请输入第一个整数:");
		int x = sc.nextInt();
		System.out.println("请输入第二个整数:");
		int y = sc.nextInt();

		/*boolean b = (x == y);
		System.out.println(b);*/
		System.out.println("请输入第三个整数:");
		int z = sc.nextInt();

		int temp = (x > y) ? x : y;
		int temp2 = (temp > z) ? temp : z;
		System.out.println("三个整数的最大值是:" + temp2);
	}
}
```
### 顺序结构语句
* A:什么是流程控制语句
  * 流程控制语句：可以控制程序的执行流程。
* B:流程控制语句的分类
  * 顺序结构
  * 选择结构
  * 循环结构
* C:执行流程：
  * 从上往下，依次执行。
* D:案例演示
  * 输出几句话看效果即可
```java
//顺序结构，依次执行
import java.util.Scanner;
class Demo1 {
	public static void main(String[] args) {
		System.out.println("Hello World1111111111111!");
		System.out.println("Hello World222222222!");
		System.out.println("Hello World333333333!");
		
		Scanner sc = new Scanner(System.in);
		int x = sc.nextInt();
		System.out.println(x);
		System.out.println("Hello World44444!");
	}
}
```


### 选择结构if语句格式1及其使用
* A:选择结构的分类
  * if语句
  * switch语句
* B:if语句有几种格式????
  * 格式1
  * 格式2
  * 格式3
* C:if语句的格式1
* 
   if(比较表达式) {
   		语句体;
   	}
* D:执行流程：
  * 先计算比较表达式的值，看其返回值是true还是false。
  * 如果是true，就执行语句体；
  * 如果是false，就不执行语句体；
* E:案例演示
  * 判断两个数据是否相等
```java
class Demo1If {
	public static void main(String[] args) {
		/*
		if语句第一种格式：
		if(关系表达式) {
			语句体
		}

		*/

		int age = 19;
		if(age >= 18 && age <= 80){
			//int x = 10;   声明是一句int x; 赋值是一句 x = 10;
			System.out.println("可以浏览本网站");
		
		}
		System.out.println("完了");
		
	}
}
```


### 选择结构if语句注意事项
* A:案例演示
  * a:比较表达式无论简单还是复杂，结果必须是boolean类型
  * b:if语句控制的语句体如果是一条语句，大括号可以省略；
    * 如果是多条语句，就不能省略。建议永远不要省略。
  * c:一般来说：有左大括号就没有分号，有分号就没有左大括号

### 选择结构if语句格式2及其使用
* A:if语句的格式2
* 
   if(比较表达式) {
   		语句体1;
   	}else {
   		语句体2;
   	}
* B:执行流程：
  * 首先计算比较表达式的值，看其返回值是true还是false。
  * 如果是true，就执行语句体1；
  * 如果是false，就执行语句体2；
* C:案例演示
  * a:获取两个数据中较大的值
  * b:判断一个数据是奇数还是偶数,并输出是奇数还是偶数
  * 注意事项：else后面是没有比较表达式的，只有if后面有。

```java
import java.util.Scanner;
class Test1If {
	/*
		a:获取两个数据中较大的值
	*/
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);		//创建对象
		System.out.println("请输入第一个整数:");
		int x = sc.nextInt();						//将键盘录入的整数存储在x中
		System.out.println("请输入第二个整数:");
		int y = sc.nextInt();						//将键盘录入的整数存储在y中

		int z;
		if(x > y) {
			//System.out.println(x);
			z = x;
		}else {
			//System.out.println(y);
			z = y;
		}

		int z = (x > y) ? x : y;
	}

	/*
	if else 和三元运算符号的区别
	如果仅仅是赋值,他俩没区别,建议用三元运算符
	如果想有输出语句或其他更复杂的语句,只能用if else
	*/
}
```

```java
class Test2If {
	//判断一个数据是奇数还是偶数,并输出是奇数还是偶数
	public static void main(String[] args) {
		int x = 10;
		
		if(x % 2 == 0) {
			System.out.println(x + "是一个偶数");
		}else {
			System.out.println(x + "是一个奇数");
		}
	}
}
```



### if语句的格式2和三元的相互转换问题
* A:案例演示
  * if语句和三元运算符完成同一个效果
* B:案例演示
  * if语句和三元运算符的区别

  * 三元运算符实现的，都可以采用if语句实现。反之不成立。

  * 什么时候if语句实现不能用三元改进呢?
    * 当if语句控制的操作是一个输出语句的时候就不能。
    * 为什么呢?因为三元运算符是一个运算符，运算符操作完毕就应该有一个结果，而不是一个输出。

#### if else 和三元运算符号的区别
如果仅仅是赋值,他俩没区别,建议用三元运算符.
如果想有输出语句或其他更复杂的语句,只能用if else


​		
### 选择结构if语句格式3及其使用
* A:if语句的格式3：
* 
   if(比较表达式1) {
   		语句体1;
   	}else if(比较表达式2) {
   		语句体2;
   	}else if(比较表达式3) {
   		语句体3;
   	}
   	...
   	else {
   		语句体n+1;
   	}
* B:执行流程：
  * 首先计算比较表达式1看其返回值是true还是false，
  * 如果是true，就执行语句体1，if语句结束。
  * 如果是false，接着计算比较表达式2看其返回值是true还是false，

  * 如果是true，就执行语句体2，if语句结束。
  * 如果是false，接着计算比较表达式3看其返回值是true还是false，

  * 如果都是false，就执行语句体n+1。
* C:案例演示
* 
   需求：键盘录入一个成绩，判断并输出成绩的等级。
   	90-100 优
   	80-89  良
   	70-79  中
   	60-69  及
   	0-59   差

```java
import java.util.Scanner;
class Test3If {
	/*
	需求：键盘录入一个成绩，判断并输出成绩的等级。
		90-100 优
		80-89  良
		70-79  中
		60-69  及
		0-59   差
	*/
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);		//创建对象
		
		System.out.println("请输入一个成绩,范围是0到100之间:");
		int x = sc.nextInt();
		if(x >=90 && x <= 100) {
			System.out.println("优");
		}else if(x >= 80 && x < 90) {
			System.out.println("良");
		}else if(x >= 70 && x < 80) {
			System.out.println("中");
		}else if(x >= 60 && x < 70) {
			System.out.println("及");
		}else if(x >= 0 && x < 60) {
			System.out.println("差");
		}else {
			System.out.println("输入错误");
		}
	}
}
```

* D:学生练习
  * 需求：
    * 键盘录入x的值，计算出y的并输出。

      * x>=3y = 2 * x + 1;
      * -1<x<3y = 2 * x;
      * x<=-1y = 2 * x – 1;

```java
import java.util.Scanner;
class Test4If {
	/*
	* 需求：
		* 键盘录入x的值，计算出y的并输出。
		
		* x>=3	y = 2 * x + 1;
		* -1<x<3	y = 2 * x;
		* x<=-1	y = 2 * x – 1;
	*/
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		System.out.println("请输入一个整数");
		int x = sc.nextInt();
		int y;
		if(x >= 3) {
			y = 2 * x + 1;
			System.out.println(y);
		}else if(x > -1 && x < 3) {
			y = 2 * x;
			System.out.println(y);
		}else if(x <= -1) {
			y = 2 * x - 1;
			System.out.println(y);
		}
	}
}
```

### 选择结构if语句的嵌套使用
* A:案例演示
  * 需求：获取三个数据中的最大值
  * if语句的嵌套使用。
  * 可以三元实现，也可以if嵌套实现。
```java
class Demo4If {
	/*
	* A:案例演示
	* 需求：获取三个数据中的最大值
	* if语句的嵌套使用。
	* 可以三元实现，也可以if嵌套实现。
	*/
	public static void main(String[] args) {
		int x = 30;
		int y = 20;
		int z = 10;

		/*if(x > y) {
			if(x > z) {
				System.out.println(x);
			}else {
				System.out.println(z);
			}
		}else {
			if(y > z) {
				System.out.println(y);
			}else {
				System.out.println(z);
			}
		}*/

		//int temp = (x > y) ? x : y  > z ? ((x > y) ? x : y) : z;三元运算符号可以嵌套,但是强烈不建议
	}
}
```

 


​	
### 选择结构switch语句的格式及其解释
* A:switch语句的格式
* B:switch语句的格式解释
* C:面试题
  * byte可以作为switch的表达式吗? 
  * long可以作为switch的表达式吗?
  * String可以作为switch的表达式吗?
* C:执行流程
  * 先计算表达式的值
  * 然后和case后面的匹配，如果有就执行对应的语句，否则执行default控制的语句
```java

class Demo1Switch {
	/*
	switch(表达式) {		表达式可以是byte,short,char,int,只要能自动类型提升为int类型的都可以
	      case 值1：		//jdk1.5版本可以接收枚举
			语句体1;		//jdk1.7版本可以接收字符串String
			break;
		    case 值2：
			语句体2;
			break;
		    …
		    default：	
			语句体n+1;
			break;
    }
		switch语句结束有两种
		1,遇到break
		2,程序执行到switch的末尾
	*/
	public static void main(String[] args) {
		String name = "张三";
		String gender = "妖"; //比如选择题答案

		switch(gender) {
			case "男":
				System.out.println(name + "是一位" + gender + "士,喜欢吃饭睡觉打lol");
			break;										//跳出switch语句
			case "女":
				System.out.println(name + "是一位" + gender + "士,喜欢逛街购物美容");
			break;
			default: //前面都不对，默认的
				System.out.println(name + "是一位" + gender + ",喜欢打雌性激素,维持美貌容颜");
			break;
		}//遇到break就跳出，或者是遇到右大括号
	}
}
```

### 选择结构switch语句的基本使用
* 定义固定值
* A:整数(给定一个值,输出对应星期几)
* B:字符串(根据给定串输出对应值)

```java
class Test1Switch {
	//A:整数(给定一个值,输出对应星期几)
	public static void main(String[] args) {
		int x = 0;//给定一个int值
		switch(x) {
			default :
				System.out.println("输入有误");
			break;
			case 1 :
				System.out.println("星期一");
			break;
			case 2 :
				System.out.println("星期二");
			break;
			case 3 :
				System.out.println("星期三");
			break;
			case 4 :
				System.out.println("星期四");
			break;
			case 5 :
				System.out.println("星期五");
			break;
			case 6 :
				System.out.println("星期六");
			break;
			case 7 :
				System.out.println("星期日");
			break;
			
			//break;
		}
	}
}
```

### 选择结构switch语句的注意事项
* A:案例演示
  * a:case后面只能是常量，不能是变量，而且，多个case后面的值不能出现相同的
  * b:default可以省略吗?
    * 可以省略，但是不建议，因为它的作用是对不正确的情况给出提示。
    * 特殊情况：
      * case就可以把值固定。
      * A,B,C,D
  * c:break可以省略吗?
    * 最后一个可以省略,其他最好不要省略
    * 会出现一个现象：case穿透。
    * 最终我们建议不要省略
  * d:default一定要在最后吗?
    * 不是，可以在任意位置。但是建议在最后。
  * e:switch语句的结束条件
    * a:遇到break就结束了
    * b:执行到末尾就结束了

### 选择结构switch语句练习
* A:看程序写结果：4 先找case，不匹配的话，执行default，遇到break跳出
* 
   int x = 2;
   	int y = 3;
   	switch(x){
   		default:
   			y++;
   			break;
   		case 3:
   			y++;
   		case 4:
   			y++;
   	}
   	System.out.println("y="+y);
* B:看程序写结果： 6 case穿透 执行了三个 y++
* 
   int x = 2;
   	int y = 3;
   	switch(x){
   		default:
   			y++;
   		case 3:
   			y++;
   		case 4:
   			y++;
   	}
   	System.out.println("y="+y);

### 选择结构if语句和switch语句的区别
* A:总结switch语句和if语句的各自使用场景
  * switch建议判断固定值的时候用
  * if建议判断区间或范围的时候用
* B:案例演示
  * 分别用switch语句和if语句实现下列需求：
    * 键盘录入月份，输出对应的季节


```java
import java.util.Scanner;

class Test3Switch {
	/*
	分别用switch语句和if语句实现下列需求：
	* 键盘录入月份，输出对应的季节
	3,4,5是春季,6,7,8是夏季,9,10,11是秋季,12,1,2是冬季
	*/
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		System.out.println("请输入月份:");
		int month = sc.nextInt();
		/*switch (month) {
		case 3:
		case 4:
		case 5:
			System.out.println(month +"月是春季");
		break;
		case 6:
		case 7:
		case 8:
			System.out.println(month +"月是夏季");
		break;
		case 9:
		case 10:
		case 11:
			System.out.println(month +"月是秋季");
		break;
		case 12:
		case 1:
		case 2:
			System.out.println(month +"月是冬季");
		break;
		default:
			System.out.println("输入有误");
		}*/
		if(month > 12 || month < 1) {
			System.out.println("输入有误");
		}else if(month >= 3 && month <= 5) {
			System.out.println(month +"月是春季");
		}else if (month >= 6 && month <= 8) {
			System.out.println(month +"月是夏季");
		}else if (month >= 9 && month <= 11) {
			System.out.println(month +"月是秋季");
		}else {
			System.out.println(month +"月是冬季");
		}
	}
}
```




