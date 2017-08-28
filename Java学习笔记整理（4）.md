---
title: Java学习笔记整理（4）
date: 2017-07-11 08:14:36
tags: [J2SE]
categories: J2SE
top: 4
---

---





<!--more-->

### 循环结构概述和for语句的格式及其使用
* A:什么是循环结构
* B:循环结构的分类
* C:循环结构for语句的格式：
* 
   for(初始化表达式;条件表达式;增量表达式) {
   		循环体;
   	}
* D:执行流程：
  * a:执行初始化语句
  * b:执行判断条件语句,看其返回值是true还是false
    * 如果是true，就继续执行
    * 如果是false，就结束循环
  * c:执行循环体语句;
  * d:执行控制条件语句
  * e:回到B继续。
* E:案例演示
  * 在控制台输出10次"helloworld"

```java
class Demo1For {
	/*
	初始化语句：
	一条或者多条语句，这些语句完成一些初始化操作。

	判断条件语句：
	这是一个boolean 表达式，这个表达式能决定是否执行循环体。

	循环体语句：
	这个部分是循环体语句，也就是我们要多次做的事情。

	控制条件语句：
	这个部分在一次循环体结束后，下一次循环判断条件执行前执行。
	通过用于控制循环条件中的变量，使得循环在合适的时候结束。

	for(初始化语句;判断条件语句;控制条件语句) {
         循环体语句;
    }

	*/
	public static void main(String[] args) {
		//在控制台输出10次”HelloWorld”
		/*System.out.println("HelloWorld");
		System.out.println("HelloWorld");
		System.out.println("HelloWorld");
		System.out.println("HelloWorld");
		System.out.println("HelloWorld");
		System.out.println("HelloWorld");
		System.out.println("HelloWorld");
		System.out.println("HelloWorld");
		System.out.println("HelloWorld");
		System.out.println("HelloWorld");*/

		for(int x = 1; x <= 10;x++) {					//第一次执行的是初始化语句int x = 1;初始化语句只执行一次
														//第二次执行的是判断语句 x <= 10;
														//第三次执行的是循环语句 System.out.println("HelloWorld");
														//第四次执行的是控制语句 x++;
														//第五次执行判断语句(2,3,4之间不断循环,直到2返回的false)
			System.out.println("HelloWorld");
		}

		System.out.println("for循环结束");
	}
}
```

### 循环结构for语句的练习之获取数据
* A:案例演示
  * 需求：请在控制台输出数据1-10
  * 需求：请在控制台输出数据10-1
* B:注意事项
  * a:判断条件语句无论简单还是复杂结果是boolean类型。
  * b:循环体语句如果是一条语句，大括号可以省略；如果是多条语句，大括号不能省略。建议永远不要省略。
  * c:一般来说：有左大括号就没有分号，有分号就没有左大括号

```java 
class Demo2For {
	public static void main(String[] args) {
		//需求：请在控制台输出数据1-10
		/*
		1,开始从1开始,定义一个初始化变量等于1
		2,结束是到10结束,定义判断语句小于等于10
		3,控制语句每次增加1就可以
		4,循环语句直接将变量x输出就可以
		*/

		/*for (int x = 1;x <= 10 ;x++ ) {
			System.out.println(x);
		}*/

		//System.out.println(x);			x在for语句执行完,就会被释放掉,所以下面可以继续定义x变量
		//需求：请在控制台输出数据10-1
		for (int x = 10;x >= 1 ;x-- ) {
			System.out.println(x);
		}
		
		//有左大括号不要有分号
		int x = 5;
		for (x = 10;x >= 1 ;x-- );			//x-- 相当于 x= x -1
			
		{
			System.out.println(x);
		}

		System.out.println(x);

	}
}
```
### 循环结构for语句的练习之求和思想
* A:案例演示
  * 需求：求出1-10之间数据之和
* B:学生练习
  * 需求：求1-100之和
  * 需求：求出1-100之间偶数和
  * 需求：求出1-100之间奇数和

```java
class Demo3For {
	public static void main(String[] args) {
		// 需求：求出1-10之间数据之和
		/*
		分析:
		1,应该有一个变量,记录住每次相加的和,初始化值应该是0,变量sum
		2,定义循环,再定义一个变量,让其从1变化到10,变量名x
		3,进行累加操作
		  0 + 1 = 1
				  1 + 2 = 3
				          3 + 3 = 6

		累加思想
		*/

		int sum = 0;
		for (int x = 1; x <= 10;x++ ) {
			sum = sum + x;					//sum += x;
		}

		System.out.println(sum);
	}
}
```
```java
class Test1For {
	public static void main(String[] args) {
		//需求：求1-100之和
		/*int sum = 0;
		for (int x = 1;x <= 100 ;x++ ) {
			sum = sum + x;
		}
		System.out.println(sum);*/
		
		//需求：求出1-100之间偶数和
		/*
		分析
		1,定义一个变量,记录住最后运算的总和
		2,定义一个循环,再定义一个变量自增,初始化值为2,每次增2
		3,累加运算
		*/
		/*int sum = 0;
		for (int x = 2;x <= 100 ;x += 2 ) {
			sum = sum + x;
		}

		for (int x = 1;x <= 100 ;x++ ) {
			if (x % 2 == 0) {
				sum = sum + x;
			}
		}
		System.out.println(sum);*/
		//需求：求出1-100之间奇数和

		int sum = 0;
		/*for (int x = 1;x <= 100 ;x += 2 ) {
			sum = sum + x;
		}*/

		for (int x = 1;x <= 100 ;x++ ) {
			if (x % 2 == 1) {
				sum = sum + x;
			}
		}
		System.out.println(sum);
	}
}
```

### 循环结构for语句的练习之水仙花
* A:案例演示
  * 需求：在控制台输出所有的”水仙花数”

  * 所谓的水仙花数是指一个三位数，其各位数字的立方和等于该数本身。
  * 举例：153就是一个水仙花数。
  * 153 = 1*1*1 + 5*5*5 + 3*3*3 = 1 + 125 + 27 = 153

```java
class Demo4For {
	/*
	 需求：在控制台输出所有的”水仙花数”

	* 所谓的水仙花数是指一个三位数，其各位数字的立方和等于该数本身。
	* 举例：153就是一个水仙花数。
	* 153 = 1*1*1 + 5*5*5 + 3*3*3 = 1 + 125 + 27 = 153

	分析:
	1,是三位数,三位数的范围是100到999
	2,获取每个位上的数字
	3,求每个位上数字立方和并且与变化的变量进行判断,如果相等打印这个数
	*/
	public static void main(String[] args) {
		for (int x = 100;x < 1000 ;x++ ) {
		
		//取模拿到各个位数上的数字
			int ge = x % 10;			//153 % 10 = 3 //10^0次方
			int shi = x / 10 % 10;		//153 / 10 = 15 % 10 = 5
			int bai = x / 10 / 10 % 10;//153 / 10 = 15 / 10 = 1 % 10 = 1

			if (ge * ge * ge + shi * shi * shi + bai * bai * bai == x ) {
				System.out.println(x);
			}
		}
	}
}
```

### 循环结构for语句的练习之统计思想
* A:案例演示
  * 需求：统计”水仙花数”共有多少个

```java 
class Demo5For {
	//需求：统计”水仙花数”共有多少个
	/*
	计数器思想(统计思想)
	分析:
	1,定义一个变量,记录水仙花数出现的个数
	*/
	public static void main(String[] args) {
		int count = 0;
		for (int x = 100;x < 1000 ;x++ ) {
			int ge = x % 10;			//153 % 10 = 3
			int shi = x / 10 % 10;		//153 / 10 = 15 % 10 = 5
			int bai = x / 10 / 10 % 10;//153 / 10 = 15 / 10 = 1 % 10 = 1

			if (ge * ge * ge + shi * shi * shi + bai * bai * bai == x ) {
				count++;				//count = count + 1;
			}
		}

		System.out.println(count);
	}
}
```


### 循环结构while语句的格式和基本使用
* A:循环结构while语句的格式：
* 
   while循环的基本格式：
   	while(判断条件语句) {
   		循环体语句;
   	}
   	
   	完整格式：
   	
   	初始化语句;
   	while(判断条件语句) {
   		 循环体语句;
   		 控制条件语句;
   	}
* B:执行流程：
  * a:执行初始化语句
  * b:执行判断条件语句,看其返回值是true还是false
    * 如果是true，就继续执行
    * 如果是false，就结束循环
  * c:执行循环体语句;
  * d:执行控制条件语句
  * e:回到B继续。
* C:案例演示
  * 需求：请在控制台输出数据1-10
```java
class Demo1While {
	/*
	A:循环结构while语句的格式：
* 
		while循环的基本格式：
		while(判断条件语句) {
			循环体语句;
		}
		
		完整格式：
		
		初始化语句;
	    while(判断条件语句) {
			 循环体语句;
			 控制条件语句;
		}
	*/
	public static void main(String[] args) {
		/*for (int x = 1;x <= 3 ;x++ ) {
			System.out.println("Hello World!");
		}

		System.out.println("=======================================================");
		
		
		int x = 1;
		while (x <= 3) {
			System.out.println("Hello World!");
			x++;
		}*/

		int y = 1;
		while (y <= 10) {
			System.out.println(y);
			y++;
		}
		
	}
}
```

### 循环结构while语句的练习
* A:求和思想
  * 求1-100之和
* B:统计思想
  * 统计”水仙花数”共有多少个

```java
class Demo2While {
	public static void main(String[] args) {
		//求1-100之和
		/*int x = 1;
		int sum = 0;
		while (x <= 100) {
			sum = sum + x;
			x++;
		}

		System.out.println(sum);*/

		//求水仙花数的个数
		int count = 0;
		int x = 100;
		while (x < 1000) {
			int ge = x % 10;
			int shi = x / 10 % 10;
			int bai = x / 100;

			if (ge * ge * ge + shi * shi * shi + bai * bai * bai == x) {
				count++;
			}

			x++;
		}

		System.out.println(count);
	}
}
```


### 循环结构do...while语句的格式和基本使用)
* A:循环结构do...while语句的格式：
* 
   do {
   		循环体语句;
   	}while(判断条件语句);
   	
   	完整格式；
   	初始化语句;
   	do {
   		循环体语句;
   		控制条件语句;
   	}while(判断条件语句);
* B:执行流程：
  * a:执行初始化语句
  * b:执行循环体语句;
  * c:执行控制条件语句
  * d:执行判断条件语句,看其返回值是true还是false
    * 如果是true，就继续执行
    * 如果是false，就结束循环
  * e:回到b继续。
* C:案例演示
  * 需求：在控制台输出10次"helloworld"
  * 需求：请在控制台输出数据1-10

```java 
class Demo1DoWhile {
	/*
	A:循环结构do...while语句的格式：
* 
		do {
			循环体语句;
		}while(判断条件语句);
		
		完整格式；
		初始化语句;
		do {
			循环体语句;
			控制条件语句;
		}while(判断条件语句);
	*/
	public static void main(String[] args) {
		/*for (int x = 1;x <= 10 ;x++ ) {
			System.out.println(x);
		}
		
		System.out.println("---------------------------------------");*/

		int y = 1;
		while (y < 0) {
			System.out.println(y);
			y++;
		}

		/*
		建议:
		当循环增量,只为了循环而定义的,建议for
		当循环增量,不只为了循环,循环结束之后,继续使用,建议while
		*/
		System.out.println("---------------------------------------");
		
		int x = 1;
		do {
			System.out.println(x);
			x++;
		}
		while (x < 0);

		/*
		while和do while的区别
		while如果条件不满足,不执行循环体
		do while 无论条件是否满足,至少会执行一次循环体
		*/
	}
	/*
	三种循环的区别
	for while do while
	
	*/
}
```

### 循环结构三种循环语句的区别
* A:案例演示
  * 三种循环语句的区别:
  * do...while循环至少执行一次循环体。
  * 而for,while循环必须先判断条件是否成立，然后决定是否执行循环体语句。
* B:案例演示
  * for循环和while循环的区别：
    * A:如果你想在循环结束后，继续使用控制条件的那个变量，用while循环，否则用for循环。不知道用for循环。因为变量及早的从内存中消失，可以提高内存的使用效率。
    * B:建议:
      * 如果是一个范围的，用for循环非常明确。
      * 如果是不明确要做多少次，用while循环较为合适。
        * 举例：吃葡萄。
        * while(x != 0)


#### 小总结

for语句比while语句效率高一点，因为for语句执行完毕以后，变量X会被立马释放，而while语句会保留X变量一边接下来使用。看程序具体的操作，如果只是为了控制循环的话，建议使用for语句。do while语句的特点是，循环必然会被执行一次。
​								
### 环结构注意事项之死循环
* A:一定要注意控制条件语句控制的那个变量的问题，不要弄丢了，否则就容易死循环。
* B:两种最简单的死循环格式
  * while(true){...}
  * for(;;){...}

```java
class Demo2 {
	public static void main(String[] args) {
		int x = 1;
		/*while (true) {				//无限循环,如果在内部不做任何的跳转语句,无限循环的下面是不能定义语句的
										//因为永远执行不到
			if (x == 10) {
				break;
			}
			x++;
		}*/
		for (; ; ) {				
		}

		System.out.println("22222222222222");
	}
}
```
### 循环结构循环嵌套输出4行5列的星星
* A:案例演示
  * 需求：请输出一个4行5列的星星(*)图案。
  * 
     如图：
     		*****
     		*****
     		*****
     		*****
     		
     	注意：
     		System.out.println("*");和System.out.print("*");的区别
* B:结论：
  * 外循环控制行数，内循环控制列数
```java
class Demo1ForFor {
	public static void main(String[] args) {
		
		/*for (int x = 1;x <= 3 ;x++ ) {			//外层循环

			//将下面所有的代码当作外层循环的循环体
			System.out.println("x = " + x);
			for (int y = 1;y <= 3 ;y++ ) {			//内层循环
				System.out.println("y = " + y);
			}
		}*/

		//需求：请输出一个4行5列的星星(*)图案。

		for (int x = 1;x <= 4 ;x++ ) {
			for (int y = 1;y <= 5 ;y++ ) {
				System.out.print("*");
			}
			System.out.println();
		}
	}
}

/*
*****
*****
*****
*****

*/
```

### 循环结构循环嵌套输出正三角形 
* A:案例演示
* 
   需求：请输出下列的形状
   	*
   	**
   	***
   	****
   	*****

```java
class Test1ForFor {
	/*
	 A:案例演示
* 
		需求：请输出下列的形状
		*
		**
		***
		****
		*****

		*****
		****
		***
		**
		*
	分析:
	1,这个图形有行有列,想到for的嵌套循环,外层循环决定行数,内存循环决定列数
	2,外层循环是五次不变,内层循环从1变化到5
	*/
	public static void main(String[] args) {
		/*for (int x = 1;x <= 5 ;x++ ) {
			for (int y = 1;y <= x ;y++ ) {			//x 从1变化到5
				System.out.print("*");
			}
			System.out.println();
		}*/

		for (int x = 1;x <= 5 ;x++ ) {
			for (int y = x;y <= 5 ;y++ ) {			//x 从1变化到5
				System.out.print("*");
			}
			System.out.println();
		}
	}
	/*
	*****
	****
	***
	**
	*
	*/
}
```

### 循环结构九九乘法表
* A:案例演示
  * 需求：在控制台输出九九乘法表。
* B:代码优化
* 
   注意：
   	'\x' x表示任意，这种做法叫转移字符。

   	'\t'	tab键的位置
   	'\r'	回车
   	'\n'	换行
```java 
class Test2For99 {
	/*
	1 * 1 = 1
	1 * 2 = 2 2 * 2 = 4
	1 * 3 = 3 2 * 3 = 6 3 * 3 = 9

	分析:
	1,99乘法表,有行有列,应该用for的嵌套循环,外层循环决定行数,内层循环决定列数

	*/
	public static void main(String[] args) {
		for (int x = 1;x <= 9 ;x++ ) {
			for (int y = 1;y <= x ;y++ ) {
				System.out.print(y  + "*" + x + "=" + (x * y)+ "\t");
			}

			System.out.print("\n");
		}
	}
}
```

### 控制跳转语句break语句
* A:什么是控制跳转语句
* B:控制跳转语句的分类
* C:break的使用场景
* D:案例演示
  * a:跳出单层循环
  * b:跳出多层循环

```java
class Demo1Break {
	public static void main(String[] args) {
		int y = 4;
		for (int x = 1;x <= 10 ;x++ ) {
			if (x == y) {
				break;							//跳出循环,满足某种条件,再跳出
			}
			System.out.println("x = " + x);
		}
	}
}
```

```java
class Demo2Break {
	public static void main(String[] args) {
		//标号,其实也是合法的标识符
		a :for (int x = 1;x <= 5 ;x++ ) {
			System.out.println("x = " + x);

			b :for (int y = 1;y <= 5 ;y++ ) {
				if (y == 3) {
					break a;						//标号可以跳出指定的循环
				}
				System.out.println("y = " + y);
			}
		}

		System.out.println("大家好");
		http://Yanhuili.github.io;						//能够正常运行，网址其实是单行注释,http其实是标号
		System.out.println("才是真的好");
	}
}
```


### 控制跳转语句continue语句
* A:continue的使用场景
* B:案例演示
  * a:跳出单层循环一次
  * b:跳出多层循环多次?????
* C:练习题
* 
   for(int x=1; x<=10; x++) {
   		if(x%3==0) {
   			//在此处填写代码
   	        
   		}
   		System.out.println(“Java基础班”);
   	}
   	
   	我想在控制台输出2次:“Java基础班“   answer:break;
   	我想在控制台输出7次:“Java基础班“   answer:continue;
   	我想在控制台输出13次:“Java基础班“	

```java
class Demo3Continue {
	public static void main(String[] args) {
		/*for (int x = 1;x <= 10 ;x++ ) {
			if (x == 4) {
				continue;				//终止本次循环,继续下次循环
				//break;
			}

			System.out.println("x = " + x);
		}*/

		int x = 10;
		if (x > 4) {
			System.out.println("x = " + x);
			//break;这是条件语句，不能运用在条件判断语句中
		}

		/*
		break和continue的应用场景
		break可以应用在switch语句还可以应用在循环里
		continue只能用在循环里
		*/
	}
}
```

```java
class Test1 {
	public static void main(String[] args) {
		for(int x=1; x<=10; x++) {
			if(x%3==0) {
				//在此处填写代码
				//break;
				//continue;
				System.out.println("Java基础班");
			}
			System.out.println("Java基础班");
		}
		
		//我想在控制台输出2次:“Java基础班“
		//我想在控制台输出7次:“Java基础班“
		//我想在控制台输出13次:“Java基础班“	
	}
}
```

### 控制跳转语句return语句
* A:return的作用
  * 返回
  * 其实它的作用不是结束循环的，而是结束方法的。
* B:案例演示
  * return和break以及continue的区别?

```java
class Demo4Return {
	//return是用来返回并结束方法的
	public static void main(String[] args) {
		for (int x = 1;x <= 5 ;x++ ) {
			if (x == 4) {
				return;				//return是将整个方法结束
				//break;			//break是将循环结束
			}

			System.out.println("x = " + x);
		}

		System.out.println("循环结束");


	}
}
```

### 方法概述和格式说明
* A:为什么要有方法
* B:什么是方法
* C:方法的格式
* D:方法的格式说明
* E:画图演示
  * 把刚才的的推荐调用方式画图解释

```java
class Demo1Method {
	/*
	修饰符 返回值类型 方法名(参数类型 参数名1，参数类型 参数名2…) {
			函数体;
			return 返回值;
    }

	*/
	public static void main(String[] args) {
		/*for (int x = 1;x <= 5 ;x++ ) {
			for (int y = 1;y <= 5 ;y++ ) {
				System.out.print("*");
			}
			System.out.println();
		}

		System.out.println("-------------------------------");

		for (int x = 1;x <= 6 ;x++ ) {
			for (int y = 1;y <= 6 ;y++ ) {
				System.out.print("*");
			}
			System.out.println();
		}*/

		print(4);
		print(5);
		print(6);

	}

	public static void print(int i) {				//i = 4
		for (int x = 1;x <= i ;x++ ) {
			for (int y = 1;y <= i ;y++ ) {
				System.out.print("*");
			}
			System.out.println();
		}
	}
}
```

### 方法之求和案例及其调用
* A:如何写一个方法
* B:案例演示
  * 需求：求两个数据之和的案例

![](http://ogtmd8elu.bkt.clouddn.com/201707062227_889.png)

```java
class Demo2Method {
	public static void main(String[] args) {
		//int sum = getSum(4,5);			//赋值调用
		//System.out.println(sum);
		//炒菜(瘦肉精猪肉,烂白菜,铬大米,地沟油);
		//有具体返回值
		//getSum(4,5);						//单独调用不推荐
		System.out.println(getSum(4,5));	//输出调用,暂时不推荐,怕你看不懂

		//return;							//如果方法的返回值类型是void,有return语句是这样写的return;
											//这个return可以省略,省略之后jvm会帮我加上
	}

	/*
	求两个整数的和
	1,明确返回值类型
	2,明确参数列表
	*/

	public static int getSum(int x,int y) {//int x = 4,int y = 5
		int sum = x + y;					// sum = 9

		return sum;
	}

	/*
	盘子 炒菜(肉,菜,米,油) {
		炒菜的动作;
		return 一盘菜;
	}
	*/
}
```
### 方法的注意事项
* A:方法调用
  * a:单独调用,一般来说没有意义，所以不推荐。
  * b:输出调用,但是不够好。因为我们可能需要针对结果进行进一步的操作。
  * c:赋值调用,推荐方案。
* B:案例演示
  * a:方法不调用不执行
  * b:方法与方法是平级关系，不能嵌套定义
  * c:方法定义的时候参数之间用逗号隔开
  * d:方法调用的时候不用在传递数据类型
  * e:如果方法有明确的返回值，一定要有return带回一个值

### 方法的练习
* A:案例演示
  * 需求：键盘录入两个数据，返回两个数中的较大值
* B:案例演示
  * 需求：键盘录入两个数据，比较两个数是否相等 


```java
import java.util.Scanner;
class Demo3Method {
	/*
	* A:案例演示
	* 需求：键盘录入两个数据，返回两个数中的较大值	
	* B:案例演示
	* 需求：键盘录入两个数据，比较两个数是否相等
	*/
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);		//创建对象
		System.out.println("请输入第一个整数");
		int x = sc.nextInt();
		System.out.println("请输入第二个整数");
		int y = sc.nextInt();

		//int max = getMax(x,y);
		//System.out.println("两个整数中的最大值是 :" + max);

		boolean b = isEquals(x,y);
		System.out.println(b);
	}

	/*
	获取两个整数的最大值
	1,明确返回值类型,int
	2,明确参数列表,int a,int b
	*/

	public static int getMax(int a,int b) {
		return a > b ? a : b;
	}

	/*
	判断两个整数是否相等
	1,明确返回值类型,boolean
	2,明确参数列表,int a,int b
	*/

	public static boolean isEquals(int a,int b) {
		return a == b;
	}
}
```

​	


### 方法之输出星形及其调用
* A:案例演示
  * 需求：根据键盘录入的行数和列数，在控制台输出星形
* B:方法调用：
  * 单独调用
  * 输出调用(错误)
  * 赋值调用(错误)
```java
import java.util.Scanner;
class Demo4Method {
	/*
	* A:案例演示
	* 需求：根据键盘录入的行数和列数，在控制台输出星形
	*/
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);		//创建对象
		System.out.println("请输入第一个整数");
		int x = sc.nextInt();
		System.out.println("请输入第二个整数");
		int y = sc.nextInt();

		//print(x,y);									//如果返回值是void,可以直接调用
		//System.out.println(print(x,y));				//返回值是void的方法是不能打印输出
	}

	/*
	定义方法打印矩形
	1,明确返回值类型,void,无法明确具体返回值类型
	2,明确参数列表,int a,int b
	*/

	public static void print(int a,int b) {
		for (int x = 1;x <= a ;x++ ) {
			for (int y = 1;y <= b ;y++ ) {
				System.out.print("*");
			}
			System.out.println();
		}
	}
}
```


​	
### 方法的练习
* A:案例演示
  * 需求：根据键盘录入的数据输出对应的乘法表

```java
import java.util.Scanner;
class Demo5Method {
	//需求：根据键盘录入的数据输出对应的乘法表
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);		//创建对象
		System.out.println("请输入一个整数");
		int x = sc.nextInt();

		print99(x);
	}

	/*
	打印99乘法表
	1,明确返回值类型void
	2,明确参数列表int a
	*/

	public static void print99(int a) {
		for (int x = 1;x <= a ;x++ ) {
			for (int y = 1;y <= x ;y++ ) {
				System.out.print(y +"*" + x + "=" + (y * x) + "\t");
			}

			System.out.println();
		}
	}
}
```

### 方法重载概述和基本使用
* A:方法重载概述
  * 求和案例
    * 2个整数
    * 3个整数
    * 4个整数
* B:方法重载：
  * 在同一个类中，方法名相同，参数列表不同。与返回值类型无关。

  * 参数列表不同：
    * A:参数个数不同
    * B:参数类型不同
    * C:参数的顺序不同(算重载,但是在开发中不用)
```java
class Demo6Method {
	//方法重载,只看参数列表,不看返回值类型
	/*
	1,参数个数不同
	2,参数类型不同
	3,参数顺序不同(开发时不建议)
	*/
	public static void main(String[] args) {
		double sum = getSum(3,4.0);
		System.out.println(sum);
	}

	//求两个数的和
	/*
	1,int
	2,int a,int b
	*/

	public static double getSum(int a,double b) {
		return a + b;
	}

	//求三个数的和
	/*
	1,int
	2,int a,int b,int c
	*/

	public static double getSum(double a,int b) {
		return a + b;
	}

}
```


​		

### 方法重载练习比较数据是否相等
* A :案例演示
  * 需求：比较两个数据是否相等。参数类型分别为两个byte类型，
  * 两个short类型，两个int类型，两个long类型，并在main方法中进行测试


```java
class Test1Method {

	/*
	* A:案例演示
	* 需求：比较两个数据是否相等。参数类型分别为两个byte类型，
	* 两个short类型，两个int类型，两个long类型，并在main方法中进行测试
	*/
	public static void main(String[] args) {
		/*byte b1 = 3;
		byte b2 = 4;
		boolean b3 = isEquals(b1,b2);
		System.out.println(b3);*/

		int x = 1;
		while (x <= 10000);						//屌丝被女神拒绝
		
		{
			System.out.println("I Love You!!!!");
			x++;
		}
	}

	//比较两个byte类型
	public static boolean isEquals(byte a,byte b) {
		System.out.println("byte");
		return a == b;
	}

	//比较两个short类型
	public static boolean isEquals(short a,short b) {
		System.out.println("short");
		return a == b;
	}

	//比较两个int类型
	public static boolean isEquals(int a,int b) {
		System.out.println("int");
		return a == b;
	}

	//比较两个long类型
	public static boolean isEquals(long a,long b) {
		System.out.println("long");
		return a == b;
	}
}
```

