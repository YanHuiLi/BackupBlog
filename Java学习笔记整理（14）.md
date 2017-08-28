---
title: Java学习笔记整理（14）
date: 2017-07-24 10:26:42
tags: [J2SE,正则表达式]
categories: J2SE
top: 14
---

---

1. 正则表达式的使用
2. Math类的使用
3. Random随机数
4. System类的使用
5. BigInterger类
6. BigDecimal类
7. Date类的使用
8. DateFormat，simpleDateFormat类使用
9. Calendar类的使用

<!--more-->

### 正则表达式的概述和简单使用
* A:正则表达式
  * 是指一个用来描述或者匹配一系列符合某个语法规则的字符串的单个字符串。其实就是一种规则。有自己特殊的应用。
* B:案例演示
  * 需求：校验qq号码.
    * 1:要求必须是5-15位数字
    * 2:0不能开头
    * 3:必须都是数字
  * a:非正则表达式实现
  * b:正则表达式实现



```java
package cn.itcast.regex;

public class Demo1_Regex {

	/**
	 * * 需求：校验qq号码.
		* 1:要求必须是5-15位数字
		* 2:0不能开头
		* 3:必须都是数字
	 */
	public static void main(String[] args) {
    demo1();
	}

  private static void demo1() {
    String regex = "[1-9][\\d]{4,14}";
    System.out.println("abcdef".matches(regex));
    System.out.println("2553868".matches(regex));
    System.out.println("012345".matches(regex));
    System.out.println("1234567890987654".matches(regex));
  }

  /*
   * 校验qq号码
   * 1,返回值类型boolean
   * 2,参数列表String qq
   */
	public static boolean qq(String qq) {
		boolean flag = true;						//标记,不符合标准就改为false
		if(qq.length() >= 5 && qq.length() <= 15) {
			if(!qq.startsWith("0")) {
				char[] arr = qq.toCharArray();		//将字符串转换成字符数组
        for (char temp : arr) {
          /*if(temp >= '0' && temp <= '9') {
						
					}else {
						flag = false;
						break;
					}*/
          if (!Character.isDigit(temp)) {  //判断该字符是否是数字
            flag = false;
            break;
          }
        }
			}else {
				flag = false;
			}
		}else {
			flag = false;
		}
		return flag;
	}
}
```

###字符类演示
* A:字符类
  * [abc] a、b 或 c（简单类） 
  * [^abc] 任何字符，除了 a、b 或 c（否定） 
  * [a-zA-Z] a到 z 或 A到 Z，两头的字母包括在内（范围） 
  * [0-9] 0到9的字符都包括
```java
package cn.itcast.regex;

public class Demo2_Regex {

	/**
	 * 字符类 
		[abc] a、b 或 c（简单类） 
		[^abc] 任何字符，除了 a、b 或 c（否定） 
		[a-zA-Z] a 到 z 或 A 到 Z，两头的字母包括在内（范围） 
		[a-d[m-p]] a 到 d 或 m 到 p：[a-dm-p]（并集） 
		[a-z&&[def]] d、e 或 f（交集） 
		[a-z&&[^bc]] a 到 z，除了 b 和 c：[ad-z]（减去） 
		[a-z&&[^m-p]] a 到 z，而非 m 到 p：[a-lq-z]（减去 

	 */
	public static void main(String[] args) {
//		demo1();
//		demo2();
//		demo3();
//		demo4();
//		demo5();
//		demo6();
    demo7();
	}

  private static void demo7() {
    String regex = "[a-z&&[^m-p]]";
    System.out.println("m".matches(regex));
    System.out.println("p".matches(regex));
    System.out.println("n".matches(regex));
    System.out.println("a".matches(regex));
  }

  public static void demo6() {
		String regex = "[a-z&&[^bc]]"; //a-z的范围里面除了bc
		System.out.println("b".matches(regex));
		System.out.println("c".matches(regex));
		System.out.println("a".matches(regex));
		System.out.println("z".matches(regex));
	}

	public static void demo5() {
		String regex = "[a-z&&[def]]";	//a-z和def的字符，取交集 单个字符
		System.out.println("d".matches(regex));
		System.out.println("a".matches(regex));
		System.out.println("de".matches(regex));
	}

	public static void demo4() {
		String regex = "[a-d[m-p]]";// a-d 和 m-p 的并集的单个字符
		System.out.println("c".matches(regex));
		System.out.println("e".matches(regex));
	}

	public static void demo3() {
		String regex = "[a-zA-Z]"; //A-Z a-z的所有单个字符
		System.out.println("a".matches(regex));
		System.out.println("z".matches(regex));
		System.out.println("A".matches(regex));
		System.out.println("Z".matches(regex));
		System.out.println("d".matches(regex));
		System.out.println("M".matches(regex));
		System.out.println("ab".matches(regex));
		System.out.println("1".matches(regex));
	}

	public static void demo2() {
		String regex = "[^abc]";				//除了a,b,c的单个字符
		System.out.println("a".matches(regex));//false含有abc
		System.out.println("10".matches(regex));//不是字符 false 两个字符
		System.out.println("d".matches(regex));// true
	}

	public static void demo1() {
		String regex = "[abc]";					//中括号中代表的是单个字符
		System.out.println("a".matches(regex));//true
		System.out.println("b".matches(regex));//true
		System.out.println("c".matches(regex));// true
		System.out.println("ab".matches(regex));// false
		System.out.println("ac".matches(regex));//false
		System.out.println("d".matches(regex));//false
		System.out.println("1".matches(regex));//false
	}

}

```



###预定义字符类演示
* A:预定义字符类
  * . 任何字符。
  * \d 数字：[0-9]
  * \w 单词字符：[a-zA-Z_0-9]


```java
package cn.itcast.regex;

public class Demo3_Regex {

	/**
	 * 	. 任何字符 
		\d 数字：[0-9] 
		\D 非数字： [^0-9] 
		\s 空白字符：[ \t\n\x0B\f\r] 
		\S 非空白字符：[^\s] 
		\w 单词字符：[a-zA-Z_0-9] 
		\W 非单词字符：[^\w] 
	 */
	public static void main(String[] args) {
//		demo1();
//		demo2();
//		demo3();
//		demo4();
//		demo5();
    demo6();
	}

  private static void demo6() {
    String regex = "\\W";//非单词字符
    System.out.println(" ".matches(regex));
    System.out.println("a".matches(regex));
    System.out.println("9".matches(regex));
  }

  public static void demo5() {
		String regex = "\\S"; //单个字符
		System.out.println("a".matches(regex)); //true
		System.out.println(" ".matches(regex));  //false
		System.out.println("    ".matches(regex));//false
	}

	public static void demo4() {
		String regex = "\\s";//一个字符
		System.out.println(" ".matches(regex)); // true
		System.out.println("    ".matches(regex));		//四个空格  false
		System.out.println("	".matches(regex));		//tab键  true
		System.out.println("a".matches(regex));// true
	}

	public static void demo3() {
		String regex = "\\D"; //[^0-9] 除了0-9都返回true
		System.out.println("a".matches(regex));
		System.out.println("5".matches(regex));
	}

	public static void demo2() {
		String regex = "\\d";					//在字符串中\是转义字符  \\d = \d 代表0-9的单个字符
		System.out.println("10".matches(regex));
		System.out.println("9".matches(regex));
	}

	public static void demo1() {
		String regex = "..";					//一个点就代表任意的一个字符
		System.out.println("a".matches(regex));
		System.out.println("aa".matches(regex));// 两个点就是两个字符。
		System.out.println("%%".matches(regex));
	}

}

```



### 数量词
* A:Greedy 数量词 
  * X? X，一次或一次也没有
  * X* X，零次或多次
  * X+ X，一次或多次
  * X{n} X，恰好 n 次 
  * X{n,} X，至少 n 次 
  * X{n,m} X，至少 n 次，但是不超过 m 次 



```java
package cn.itcast.regex;

public class Demo4_Regex {

	/**
	 * 	Greedy 数量词 
		X? X，一次或一次也没有 
		X* X，零次或多次  //闭包
		X+ X，一次或多次 
		X{n} X，恰好 n 次 
		X{n,} X，至少 n 次 
		X{n,m} X，至少 n 次，但是不超过 m 次 

	 */
	public static void main(String[] args) {
//		demo1();
//		demo2();
//		demo3();
//		demo4();
//		demo5();
		demo6();

	}

	public static void demo6() {
		String regex = "[abc]{5,15}";//abc三个字符出现5到15次
		System.out.println("abcb".matches(regex));
		System.out.println("abcbaaaaaaaaaaa".matches(regex));
		System.out.println("aaaa".matches(regex));
		System.out.println("aaaaaaaaaaaaaaaaa".matches(regex));
	}

	public static void demo5() {
		String regex = "[abc]{5,}";//[]里面的内容出现至少N次
		System.out.println("abcba".matches(regex));
		System.out.println("abcbabbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb".matches(regex));
	}

	public static void demo4() {
		String regex = "[abc]{5}"; //  abc 三个字符恰好出现了N次
		System.out.println("aaaa".matches(regex));
		System.out.println("aaaaa".matches(regex));
		System.out.println("aaaaaa".matches(regex));
		System.out.println("abcba".matches(regex));
	}

	public static void demo3() {
		String regex = "[abc]+";
		System.out.println("".matches(regex));
		System.out.println("aaaaaaaaaaaaaaaa".matches(regex));
		System.out.println("aaaabbbbccccccccccc".matches(regex));
		System.out.println("d".matches(regex));
	}

	public static void demo2() {
		String regex = "[abc]*";
		System.out.println("aaaaaaaaaaaaaaaaaaaaaaaaaa".matches(regex));
		System.out.println("aaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbb".matches(regex));
		System.out.println("dddddd".matches(regex));
		System.out.println("abcabcabc".matches(regex));
		System.out.println("d".matches(regex));
	}

	public static void demo1() {
		String regex = "[abc]?";					//abc要不就是一个也没出现，要不就只出现一个字符
		System.out.println("a".matches(regex));
		System.out.println("d".matches(regex));
		System.out.println("".matches(regex));
		System.out.println("ab".matches(regex));
	}

}

```



### 正则表达式的分割功能
* A:正则表达式的分割功能
  * String类的功能：public String[] split(String regex)
* B:案例演示
  * 正则表达式的分割功能

```java
package cn.itcast.regex;

public class Demo5_Split {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		String s = "我..爱....java";
		String regex = "\\.+";					//.是代表任意字符,不能直接用点切割,需要转义
		String[] arr = s.split(regex);			//切割后返回的是字符串数组
		for (int i = 0; i < arr.length; i++) {
			System.out.println(arr[i]);
		}
	}

}

```





### 把给定字符串中的数字排序
* A:案例演示
  * 需求：我有如下一个字符串:”91 27 46 38 50”，请写代码实现最终输出结果是：”27 38 46 50 91”



```java
package cn.itcast.test;

import java.util.Arrays;

public class Test1_Split {

	/**
	 * A:案例演示
	* 需求：我有如下一个字符串:"91 27 46 38 50"，请写代码实现最终输出结果是："27 38 46 50 91"
	* 1,将字符串切割成字符串数组
	* 2,定义一个与String数组长度相同的数组,将String数组中的字符串转换后存储在该数组中
	* 3,对int数组排序
	* 4,将int数组转换成字符串
	 */
	public static void main(String[] args) {
		String s = "91 27 46 38 50";
		String[] arr = s.split(" ");			//将字符串切割成字符串数组
		int[] iArr = new int[arr.length];		//定义int类型的数组,长度与String数组相同
		
		for (int i = 0; i < arr.length; i++) {	//遍历字符串数组
			iArr[i] = Integer.parseInt(arr[i]); //将字符串中的每一个数字字符串转成数字
		}
		
		Arrays.sort(iArr);						//排序
		
		System.out.println(arrToString(iArr));	//将int数组转换成字符串
		
	}

	public static String arrToString(int[] arr) {
		StringBuilder sb = new StringBuilder();
		//sb.append("[");
		for (int i = 0; i < arr.length; i++) {
			if(i == arr.length - 1) {
				sb.append(arr[i]);
			}else {
				sb.append(arr[i]).append(", ");
			}
		}
		
		return sb.toString();
	}
}

```



### 正则表达式的替换功能
* A:正则表达式的替换功能
  * String类的功能：public String replaceAll(String regex,String replacement)
* B:案例演示
  * 正则表达式的替换功能



```java
public static void demo1() { // out: itoost
		String s = "itcast";  // 字符串s和regex进行比较，一旦找到匹配的字符就用o代替
		String regex = "[abc]";
		String s2 = s.replaceAll(regex, "o");
		System.out.println(s2);
	}
```



### 正则表达式的分组功能
* A:正则表达式的分组功能
  * 捕获组可以通过从左到右计算其开括号来编号。例如，在表达式 ((A)(B(C))) 中，存在四个这样的组： 

* 
        1     ((A)(B(C))) 
        2     (A 
        3     (B(C)) 
        4     (C) 
        
        组零始终代表整个表达式。
       B:案例演示

    a:切割

​       需求：请按照叠词切割： "sdqqfgkkkhjppppkl";(.)\\1+


```java
package cn.itcast.test;

public class Test2_Split {

	/**
	 * @param args
	 * a:切割
		需求：请按照叠词切割： "sdqqfgkkkhjppppkl";
	 */
	public static void main(String[] args) {
		String regex = "(.)\\1+";
		String s = "sdqqfgkkkhjppppkl";
		String[] arr = s.split(regex);
		for (int i = 0; i < arr.length; i++) {
			System.out.println(arr[i]);
		}
	}

}
```




   b:替换

    ​```
    需求：我我....我...我.要...要要...要学....学学..学.编..编编.编.程.程.程..程
    将字符串还原成:“我要学编程”。
    ​```




```java
package cn.itcast.test;

public class Test3_Replace {

	/**
	 * @param args
	 * b:替换
		需求：我我....我...我.要...要要...要学....学学..学.编..编编.编.程.程.程..程
		将字符串还原成:“我要学编程”。
	 */
	public static void main(String[] args) {
		String s = "我我....我...我.要...要要...要学....学学..学.编..编编.编.程.程.程..程";
		String s2 = s.replaceAll("\\.+", "");
		String s3 = s2.replaceAll("(.)\\1+", "$1");		//$是获取对应组中的字符，固定写法
		System.out.println(s3);
	}

}

```



###Pattern和Matcher的概述

* A:Pattern和Matcher的概述
* B:模式和匹配器的典型调用顺序
  * 通过JDK提供的API，查看Pattern类的说明

  * 典型的调用顺序是 
  * Pattern p = Pattern.compile("a*b");
  * Matcher m = p.matcher("aaaaab");
  * boolean b = m.matches();

###正则表达式的获取功能
* A:正则表达式的获取功能
  * Pattern和Matcher的结合使用
* B:案例演示
  * 需求：把一个字符串中的手机号码获取出来


```java
package cn.itcast.regex;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Demo7_Pattern {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		//demo1();
		String str = "我的电话号码是18511866260,曾经用过13887654321,还用过18912345678";
		String regex = "1[34578]\\d{9}";
		Pattern p = Pattern.compile(regex);			//获取正则对象
		Matcher m = p.matcher(str);					//获取匹配器
		
		/*boolean b = m.find();
		System.out.println(b);
		String s = m.group();
		System.out.println(s);*/
		while(m.find())								//当while语句只控制一句语句的时候,大括号可以省略
			System.out.println(m.group());
	}

	public static void demo1() {
		Pattern p = Pattern.compile("a*b");			//获取正则对象
		Matcher m = p.matcher("aaaaabc");			//获取匹配器
		boolean b = m.matches();
		
		System.out.println(b);
	}

}

```





### Math类概述和方法使用
* A:Math类概述
  * Math 类包含用于执行基本数学运算的方法，如初等指数、对数、平方根和三角函数。 
* B:成员方法
  * public static int abs(int a)
  * public static double ceil(double a)
  * public static double floor(double a)
  * public static int max(int a,int b) min自学
  * public static double pow(double a,double b)
  * public static double random()
  * public static int round(float a) 参数为double的自学
  * public static double sqrt(double a)

```java
package cn.itcast.otherclass;

public class Demo01_Math {

	/**
	 * * public static int abs(int a)
	* public static double ceil(double a)
	* public static double floor(double a)
	* public static int max(int a,int b) min自学
	* public static double pow(double a,double b) //a^b 次方
	* public static double random()
	* public static int round(float a) 参数为double的自学
	* public static double sqrt(double a)
	* 13.00
	* 12.33
	* 12.00
	 */
	public static void main(String[] args) {
		//System.out.println(Math.PI);
		//System.out.println(Math.abs(-10));				//取绝对值
		//System.out.println(Math.abs(10));					
		//System.out.println(Math.ceil(12.33));				//天花板(向上取整,但是是一个double数) 13
		//System.out.println(Math.ceil(12.77));         13
		//System.out.println(Math.floor(12.33));			//地板(向下取整,但是是一个double数) 12
		//System.out.println(Math.floor(12.77));        12
		//System.out.println(Math.max(10, 100));
		//System.out.println(Math.pow(2, 3));	 			//2.0 ^ 3.0次方
		//System.out.println(Math.random());				//随机数(0.0 - 1.0之间,不包括1.0)
		//System.out.println(Math.round(12.33));
		//System.out.println(Math.round(12.53));			//四舍五入
		System.out.println(Math.sqrt(9));	 				//开平方
	}

}

```





### Random类的概述和方法使用
* A:Random类的概述
  * 此类用于产生随机数如果用相同的种子创建两个 Random 实例，
  * 则对每个实例进行相同的方法调用序列，它们将生成并返回相同的数字序列。
* B:构造方法
  * public Random()
  * public Random(long seed)
* C:成员方法
  * public int nextInt()
  * public int nextInt(int n)(重点掌握)



```java
package Yanhui.OtherClass;

import java.util.Random;

/**
 *
 * 当指定种子（seed）的时候，生出的随机数就确定了， 因为是根据算法算出来的。
 * 所以叫做伪随机数。
 *
 * Created by Archer on 2017/7/25.
 */
public class Demo2_Random {

  public static void main(String[] args) {

    Random random=new Random(11);

    for (int i = 0; i < 10; i++) {
      System.out.println(random.nextInt(10));//生成的数字在0-9之间
    }

  }

}

```



###System类的概述和方法使用
* A:System类的概述
  * System 类包含一些有用的类字段和方法。它不能被实例化。 
* B:成员方法
  * public static void gc()
  * public static void exit(int status)
  * public static long currentTimeMillis()
* C:案例演示
  * System类的成员方法使用



```java
package Yanhui.bean;

import java.lang.ref.PhantomReference;
import java.lang.ref.WeakReference;

/**
 * Created by Archer on 2017/7/25.
 */
public class Person {

  public Person(String name, int age) {
    super();
    this.name = name;
    this.age = age;
  }

public Person(){

}

  private  String name;
  private int age;

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

  @Override
  public String toString() {
    return "Person{" +
        "name='" + name + '\'' +
        ", age=" + age +
        '}';
  }

  /**
   * Called by the garbage collector on an object when garbage collection
   * determines that there are no more references to the object.
   * A subclass overrides the {@code finalize} method to dispose of
   * system resources or to perform other cleanup.
   * <p>
   * The general contract of {@code finalize} is that it is invoked
   * if and when the Java&trade; virtual
   * machine has determined that there is no longer any
   * means by which this object can be accessed by any thread that has
   * not yet died, except as a result of an action taken by the
   * finalization of some other object or class which is ready to be
   * finalized. The {@code finalize} method may take any action, including
   * making this object available again to other threads; the usual purpose
   * of {@code finalize}, however, is to perform cleanup actions before
   * the object is irrevocably discarded. For example, the finalize method
   * for an object that represents an input/output connection might perform
   * explicit I/O transactions to break the connection before the object is
   * permanently discarded.
   * <p>
   * The {@code finalize} method of class {@code Object} performs no
   * special action; it simply returns normally. Subclasses of
   * {@code Object} may override this definition.
   * <p>
   * The Java programming language does not guarantee which thread will
   * invoke the {@code finalize} method for any given object. It is
   * guaranteed, however, that the thread that invokes finalize will not
   * be holding any user-visible synchronization locks when finalize is
   * invoked. If an uncaught exception is thrown by the finalize method,
   * the exception is ignored and finalization of that object terminates.
   * <p>
   * After the {@code finalize} method has been invoked for an object, no
   * further action is taken until the Java virtual machine has again
   * determined that there is no longer any means by which this object can
   * be accessed by any thread that has not yet died, including possible
   * actions by other objects or classes which are ready to be finalized,
   * at which point the object may be discarded.
   * <p>
   * The {@code finalize} method is never invoked more than once by a Java
   * virtual machine for any given object.
   * <p>
   * Any exception thrown by the {@code finalize} method causes
   * the finalization of this object to be halted, but is otherwise
   * ignored.
   *
   * @throws Throwable the {@code Exception} raised by this method
   * @jls 12.6 Finalization of Class Instances
   * @see WeakReference
   * @see PhantomReference
   */
  @Override
  protected void finalize() throws Throwable {
    System.out.println("垃圾被回收了");
  }
}

```





```java
package Yanhui.OtherClass;

import Yanhui.bean.Person;

/**
 * gc的使用
 * exit的使用
 * Created by Archer on 2017/7/25.
 */
public class Demo1_System {

  public static void main(String[] args) {
//    demo1();
//    demo2();
//    demo3();


  }

  private static void demo3() {
    ////返回的毫秒的值初始化时间是1970年1月1日，为了纪念unix和c语言的诞生。
    long start = System.currentTimeMillis();
    for (int i = 0; i < 10000; i++) {
      System.out.println("*");
    }

    long End = System.currentTimeMillis();
    System.out.println(End-start);//0.4秒，可以检测方法的执行时间
  }

  private static void demo2() {
    Person Y =new Person("李四",24);
    System.out.println(Y.toString());

    System.exit(0);//使用exit方法关闭Jvm虚拟机

    Person Y1 =new Person("结束了",24);
    System.out.println(Y1);
  }

  private static void demo1() {
    //演示垃圾回收的方式，调用System.gc()方法
    Person mPerson= new Person("张三",23);
    mPerson.setAge(26);
    System.out.println(mPerson);
    mPerson=null;
    System.gc();
  }
}

```



### BigInteger类的概述和方法使用
* A:BigInteger的概述
  * 可以让超过Integer范围内的数据进行运算
* B:构造方法
  * public BigInteger(String val)
* C:成员方法
  * public BigInteger add(BigInteger val)
  * public BigInteger subtract(BigInteger val)
  * public BigInteger multiply(BigInteger val)
  * public BigInteger divide(BigInteger val)
  * public BigInteger[] divideAndRemainder(BigInteger val)

```java
package Yanhui.OtherClass;

import java.math.BigInteger;

/**
 * BigInteger的使用
 * Created by Archer on 2017/7/25.
 */
public class Demo4_BigInteger {

  public static void main(String[] args) {

    BigInteger b1 =new BigInteger("22");
    BigInteger b2 =new BigInteger("2");
//    demo(b1, b2);

    demo2(b1, b2);
  }

  //把商和余数保存在一个长度为2的数组里面数据里面，
  private static void demo2(BigInteger b1, BigInteger b2) {
    BigInteger[] bigIntegers = b1.divideAndRemainder(b2);

    for (BigInteger bigInteger : bigIntegers) {
      System.out.println(bigInteger);
    }
  }

  // 加减乘除
  private static void demo(BigInteger b1, BigInteger b2) {
    System.out.println(b1.add(b2));
    System.out.println(b1.subtract(b2));
    System.out.println(b1.multiply(b2));
    System.out.println(b1.divide(b2));
  }

}

```



###bigDecimal类的概述和方法使用

* A:BigDecimal的概述
  * 由于在运算的时候，float类型和double很容易丢失精度，演示案例。
  * 所以，为了能精确的表示、计算浮点数，Java提供了BigDecimal

  * 不可变的、任意精度的有符号十进制数。
* B:构造方法
  * public BigDecimal(String val)
* C:成员方法
  * public BigDecimal add(BigDecimal augend)
  * public BigDecimal subtract(BigDecimal subtrahend)
  * public BigDecimal multiply(BigDecimal multiplicand)
  * public BigDecimal divide(BigDecimal divisor)
* D:案例演示
  * BigDecimal类的构造方法和成员方法使用



```java
package Yanhui.OtherClass;

import java.math.BigDecimal;

/**
 * BigDecimal 为了更精确的解决小数。
 * Created by Archer on 2017/7/25.
 */
public class Demo5_BigDecimal {

  public static void main(String[] args) {
    System.out.println(2.0-1.1);//存在误差 0.8999999999999999

    BigDecimal bigDecimal=new BigDecimal(2.0);
    BigDecimal bigDecimal1=new BigDecimal(1.1);
    System.out.println(bigDecimal.subtract(bigDecimal1));//用double数字不精确
    System.out.println("===========================");
    BigDecimal b1=new BigDecimal("2.0");
    BigDecimal b2=new BigDecimal("1.1");
    System.out.println(b1.subtract(b2));//0.9

    //开发首选
    System.out.println("===================");
    BigDecimal b3=BigDecimal.valueOf(2.0);
    BigDecimal b4=BigDecimal.valueOf(1.2);
    System.out.println(b1.subtract(b2));//0.9


  }
}

```



### Date类的概述和方法使用
* A:Date类的概述
  * 类 Date 表示特定的瞬间，精确到毫秒。 
* B:构造方法
  * public Date()
  * public Date(long date)
* C:成员方法
  * public long getTime()
  * public void setTime(long time)

```java
package Yanhui.OtherClass;

import java.util.Date;

/**
 * Date的使用
 *
 * Created by Archer on 2017/7/25.
 */
public class Demo6_Date {

  public static void main(String[] args) {
//    demo1();
    Date d1=new Date();
    d1.setTime(1000);//设置时间
    System.out.println(d1.getTime());//得到时间
    System.out.println(System.currentTimeMillis());//以1970/1/1为基点的毫秒值，系统时间



  }

  private static void demo1() {
    Date date=new Date(0); //我们国家时区在东八区
    System.out.println(date);

    Date   date1=new Date(2000); //根据指定毫秒设置时间的返回值
    System.out.println(date1);
  }
}

```



###SimpleDateFormat类实现日期和字符串的相互转换
* A:DateFormat类的概述
  * DateFormat 是日期/时间格式化子类的抽象类，它以与语言无关的方式格式化并解析日期或时间。是抽象类，所以使用其子类SimpleDateFormat
* B:SimpleDateFormat构造方法
  * public SimpleDateFormat()
  * public SimpleDateFormat(String pattern)
* C:成员方法
  * public final String format(Date date)
  * public Date parse(String source)




```java
private static void demo() {

  Date date=new Date();
  DateFormat dateFormat=DateFormat.getDateInstance();//创建子类对象。
  SimpleDateFormat simpleDateFormat=new SimpleDateFormat("yyyy年MM月d天 HH:mm:ss"); //mm 分，秒。
  System.out.println(dateFormat.format(date));
  System.out.println(simpleDateFormat.format(date));
}	
```



### 你来到这个世界多少天案例
* A:案例演示
  * 需求：算一下你来到这个世界多少天?

```java
package Yanhui.OtherClass;

import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * dateFormat 的使用
 * Created by Archer on 2017/7/25.
 */
public class Demo7_DateFormat {

  public static void main(String[] args) throws ParseException {
//    demo();
    long Mine = demo2("1993年08月31日 16:55:10");
    long yours = demo2("1994年05月18日 16:55:10");

    System.out.println(Math.abs(Mine-yours));

  }

  /*
  计算活了多少天
  String string ="1994年05月18日 16:55:10";
    String string1 ="1993年08月31日 16:55:10";
   */
  private static long demo2(String s) throws ParseException {

    SimpleDateFormat simpleDateFormat=new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss"); //特别注意，格式一定要一一匹配
    Date date = simpleDateFormat.parse(s); //使用simpleDateFormat，解析含有出生生日的string，得到一个date对象
    long time = date.getTime(); //返回的时间值是以1970年为时间基点的，即1970 1.1-1994 5.18的毫秒值
    long end = System.currentTimeMillis();//返回的是 1970 1.1 -当前系统时间 的毫秒值
    long day = (end - time)/1000/60/60/24; //二者相减得到活了多少毫秒，除1000得秒 除60得分 除60得小时，除24得天
    System.out.println(day);

    return day;
  }
}
```





### 日期工具类的编写和测试案例 
* A:案例演示
  * 日期工具类的编写
  * 日期工具类的测试



```java
package Yanhui.Utils;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * date转换成字符串

 * Created by Archer on 2017/7/25.
 */
public class Utils {



  //将date转换成字符串
  public  static String DateToString(Date date, String string){

    SimpleDateFormat simpleDateFormat=new SimpleDateFormat(string);

    return simpleDateFormat.format(date);
  }

  //将字符串传唤成date

 public   static Date  StringToDate(String string,String Pattern) throws ParseException {

  SimpleDateFormat simpleDateFormat=new SimpleDateFormat(Pattern);
   Date date = simpleDateFormat.parse(string);

  return date;
 }

}

```



### Calendar类的概述和获取日期的方法
* A:Calendar类的概述
  * Calendar 类是一个抽象类，它为特定瞬间与一组诸如 YEAR、MONTH、DAY_OF_MONTH、HOUR 等日历字段之间的转换提供了一些方法，并为操作日历字段（例如获得下星期的日期）提供了一些方法。
* B:成员方法
  * public static Calendar getInstance()
  * public int get(int field)


```java
package Yanhui.OtherClass;

import java.util.Calendar;

/**
 * Calendar 的使用
 * 
 * Created by Archer on 2017/7/25.
 */
public class Demo8_Calendar {

  public static void main(String[] args) {
    Calendar mCalendar=Calendar.getInstance();
//通过制定的字段获取年月日
    int year = mCalendar.get(Calendar.YEAR);
    int Month = mCalendar.get(Calendar.MONTH );//月份从0开始，要加1
    int day = mCalendar.get(Calendar.DAY_OF_MONTH);

    System.out.println(year);
    System.out.println(Month+1);
    System.out.println(day);

  }
}

```





###Calendar类的add()和set()方法
* A:成员方法
  * public void add(int field,int amount)
  * public final void set(int year,int month,int date)
* B:案例演示
  * Calendar类的成员方法使用

```java
 private static void demo2() {
    Calendar calendar= Calendar.getInstance();
//     calendar.add(Calendar.YEAR,1);// 对前面的日期字段根据正负数进行修改。
//    calendar.add(Calendar.MONTH,-1); //calendar从0月开始算起，实际少1.加了递增

    calendar.set(2020,12,19);

    System.out.println(calendar.get(Calendar.YEAR));
    System.out.println(calendar.get(Calendar.MONTH)+1);// month要加1
  }
```

###  如何获取任意年份的2月份有多少天
* A:案例演示
  * 需求：键盘录入任意一个年份，获取任意一年的二月有多少天?

```java
/*
 判断一个年头是不是闰年
1.  首先设置到当年3月1日
2.  然后调用add方法 -1，回到当年的2月最后一天
3.get到最后一天的数值和29比较，等于就是闰年，不等于就是平年。
  */
  
  public static boolean RunYear(int year){
    Calendar calendar=Calendar.getInstance();
    calendar.set(year,2,1);
    calendar.add(Calendar.DAY_OF_MONTH,-1);

    return  calendar.get(Calendar.DAY_OF_MONTH)==29;

  }
```



