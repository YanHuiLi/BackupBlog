---
title: Java学习笔记整理（17）
date: 2017-08-01 11:35:34
tags: [J2SE,Set集合]
categories: J2SE
top: 17
---

---

1. hashSet如何保证元素的唯一性。（重写HashCode和equals方法）
2. LinkedHashSet底层链表实现，怎么存怎么取
3. TreeSet（自然排序和传入比较器进行排序）
4. compareTo方法（根据返回值来判断储存情况，二叉树结构，正右，负作，0不存）。
5. List的四种遍历方法
6. Set有两种遍历方法

<!--more-->

### HashSet存储字符串并遍历

- A:Set集合概述及特点

  - 通过API查看即可

- B:案例演示

  - HashSet存储字符串并遍历
```java
   HashSet<String> hs = new HashSet<>();
    boolean b1 = hs.add("a");
    boolean b2 = hs.add("a");			//当存储不成功的时候,返回false

    System.out.println(b1);
    System.out.println(b2);
    for(String s : hs) {
    	System.out.println(s);
    }
```







###  HashSet存储自定义对象保证元素唯一性

- A:案例演示

  - 存储自定义对象，并保证元素唯一性。
 ```
    HashSet<Person> hs = new HashSet<>();
    hs.add(new Person("张三", 23));
    hs.add(new Person("张三", 23));
    hs.add(new Person("李四", 23));
    hs.add(new Person("李四", 23));
    hs.add(new Person("王五", 23));
    hs.add(new Person("赵六", 23));
 ```

- 重写hashCode()和equals()方法

```java
package cn.itcast.set;

import java.util.HashSet;

import cn.itcast.bean.Person;

public class Demo1_HashSet {

	/**
	 * @param args
	 * List
	 * 		有序(存和取是一致的),有索引,可以重复
	 * Set
	 * 		无序(存和取不是一致的),无索引,不可以重复
	 */
	public static void main(String[] args) {
		//demo1();
		//demo2();
		HashSet<Person> hs = new HashSet<>();
		hs.add(new Person("张三", 23));
		hs.add(new Person("张三", 23));
		hs.add(new Person("李四", 23));
		hs.add(new Person("李四", 23));
		hs.add(new Person("王五", 23));
		hs.add(new Person("赵六", 23));
		
		System.out.println(hs);
	}

	public static void demo2() {
		HashSet<String> hs = new HashSet<>();
		hs.add("a");
		hs.add("b");
		hs.add("c");
		hs.add("d");
		
		System.out.println(hs);// 根据哈希的值算出来的
	}

	public static void demo1() {
		HashSet<String> hs = new HashSet<>();
		boolean b1 = hs.add("a");
		boolean b2 = hs.add("a");			//当存储不成功的时候,返回false
		
		System.out.println(b1);
		System.out.println(b2);
		System.out.println(hs);
	}

}

```





```java
package cn.itcast.bean;

public class Person implements Comparable<Person>{
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
	public void setAge(int age) {
		this.age = age;
	}
	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}
	/*@Override
	public boolean equals(Object obj) {

	    //验证hashSet的唯一性，是不是由equals决定，证明无关。
		System.out.println("equals");
		Person p = (Person)obj;
		return this.name.equals(p.name) && this.age == p.age;
	}
	@Override
	public int hashCode() {
		// 张三.hashCode()= 40 + 30
		// 李四.hashCode()= 50 + 20
		int num = 38;
		return name.hashCode() + age * num;
	}*/
	/*
	 * 为什么是31
	 * 1,31是一个质数(只能被1和本身整除)
	 * 2,31这个数既不大,也不小
	 * 3,31是2的5次方-1(2向左移动5次再减一)
	 */
	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + age;
		result = prime * result + ((name == null) ? 0 : name.hashCode());
		return result;
	}
	
	@Override
	public boolean equals(Object obj) {
		if (this == obj)							//调用的对象和传入的对象是同一个对象
			return true;							//直接返回true
		if (obj == null)							//传入的对象为null
			return false;							//返回false
		if (this.getClass() != obj.getClass())		//调用的对象对应的字节码对象和传入对象对应的字节码对象不是同一个对象
			return false;							//返回false
		Person other = (Person) obj;				//向下转型,可以调用子类特有的属性和行为
		if (age != other.age)						//如果调用的对象的age不等于传入对象的age
			return false;							//返回false
		if (name == null) {							//如果调用的name为null
			if (other.name != null)					//而传入的对象name不为
				return false;						//返回false
		} else if (!name.equals(other.name))		//如果调用对象的name和传入对象的name不是同一个name
			return false;							//返回false
		return true;								//返回true
	}
	
	/*@Override
	public int compareTo(Person o) {				//按照年龄排序
		int num = this.age - o.age;					//年龄作为主要条件比较
		return num == 0 ? this.name.compareTo(o.name) : num;//姓名作为次要条件,只有年龄相同的时候比较姓名
	}*/
	
	/*@Override
	public int compareTo(Person o) {				//按照姓名排序
		int num = this.name.compareTo(o.name);		//姓名作为主要条件
		return num == 0 ? this.age - o.age : num;	//年龄是次要条件
	}*/
	
	@Override
	public int compareTo(Person o) {				//按照姓名的长度排序
		int num = this.name.length() - o.name.length();	//长度作为主要条件
		int num2 = num == 0 ? this.name.compareTo(o.name) : num;//姓名的内容作为次要条件
		return num2 == 0 ? this.age - o.age : num2;		//年龄作为次次要条件
	}
}
```



### HashSet存储自定义对象保证元素唯一性图解及代码优化

- A:画图演示
  - 画图说明比较过程
- B:代码优化
  - 为了减少比较，优化hashCode()代码写法。

  - 最终版就是自动生成即可。

重写了hashCode()和equals()以后实现了，去除hashset里面内容相同的对象，保证元素的唯一性。

### HashSet如何保证元素唯一性的原理

- 1.HashSet原理
  - 我们使用Set集合都是需要去掉重复元素的, 如果在存储的时候逐个equals()比较, 效率较低,哈希算法提高了去重复的效率, 降低了使用equals()方法的次数
  - 当HashSet调用add()方法存储对象的时候, 先调用对象的hashCode()方法得到一个哈希值, 然后在集合中查找是否有哈希值相同的对象
    - 如果没有哈希值相同的对象就直接存入集合
    - 如果有哈希值相同的对象, 就和哈希值相同的对象逐个进行equals()比较,比较结果为false就存入, true则不存
- 2.将自定义类的对象存入HashSet去重复
  - 类中必须重写hashCode()和equals()方法
  - hashCode(): 属性相同的对象返回值必须相同, 属性不同的返回值尽量不同(提高效率)
  - equals(): 属性相同返回true, 属性不同返回false,返回false的时候存储

###  LinkedHashSet的概述和使用

- A:LinkedHashSet的特点
- B:案例演示
  - LinkedHashSet的特点（链表）
    - 可以保证怎么存就怎么取 


当有保证元素唯一和怎么存怎么取的时候，优先使用LinkedHashSet。

```java
package cn.itcast.set;

import java.util.LinkedHashSet;

public class Demo2_LinkedHashSet {

   /**
    * @param args
    */
   public static void main(String[] args) {
      LinkedHashSet<String> lh = new LinkedHashSet<>();     //可以怎么存就怎么取
      lh.add("a");
      lh.add("a");
      lh.add("b");
      lh.add("c");
      lh.add("d");
      
      System.out.println(lh);
   }

}
```


###  产生10个1-20之间的随机数要求随机数不能重复

- A:案例演示

  - 需求：编写一个程序，获取10个1至20的随机数，要求随机数不能重复。并把最终的随机数输出到控制台。
  ```java
    HashSet<Integer> hs = new HashSet<>();	//创建集合对象，必须使用基本类型的包装类才有hashCode方法。
    Random r = new Random();					//创建随机数对象
    while(hs.size() < 10) {
    	int num = r.nextInt(20) + 1;			//生成1到20的随机数
    	hs.add(num);
    }

    for (Integer integer : hs) {				//遍历集合
    	System.out.println(integer);			//打印每一个元素
    }
  ```

### 练习

- 使用Scanner从键盘读取一行输入,去掉其中重复字符, 打印出不同的那些字符

  - aaaabbbcccddd

 ```java
    Scanner sc = new Scanner(System.in);			//创建键盘录入对象
    System.out.println("请输入一行字符串:");
    String line = sc.nextLine();					//将键盘录入的字符串存储在line中
    char[] arr = line.toCharArray();				//将字符串转换成字符数组
    HashSet<Character> hs = new HashSet<>();		//创建HashSet集合对象

    for(char c : arr) {								//遍历字符数组
    	hs.add(c);									//将字符数组中的字符添加到集合中
    }

    for (Character ch : hs) {						//遍历集合
    	System.out.println(ch);
    }
 ```

### 练习

- 将集合中的重复元素去掉
```java

public static void main(String[] args) {
  		ArrayList<String> list = new ArrayList<>();
  		list.add("a");
  		list.add("a");
  		list.add("a");
  		list.add("b");
  		list.add("b");
  		list.add("b");
  		list.add("b");
  		list.add("c");
  		list.add("c");
  		list.add("c");
  		list.add("c");
  		
  		System.out.println(list);
  		System.out.println("去除重复后:");
  		getSingle(list);
  		System.out.println(list);
  	}
  	
  	/*
  	 * 将集合中的重复元素去掉
  	 * 1,void
  	 * 2,List<String> list
  	 */
  	
  	public static void getSingle(List<String> list) {
  		LinkedHashSet<String> lhs = new LinkedHashSet<>();
  		lhs.addAll(list);									//将list集合中的所有元素添加到lhs
  		list.clear();										//清空原集合
  		list.addAll(lhs);									//将去除重复的元素添回到list中
  	}
```

### TreeSet存储Integer类型的元素并遍历

- A:案例演示
  - TreeSet存储Integer类型的元素并遍历

```java
public static void demo1() {
		TreeSet<Integer> ts = new TreeSet<>();	//
		ts.add(5);
		ts.add(1);
		ts.add(3);
		ts.add(2);
		ts.add(2);
		ts.add(4);
		ts.add(4);
		
		System.out.println(ts);
	}

//out ：[1, 2, 3, 4, 5] 有顺序的，没有重复的元素。（从小到大）
```





### TreeSet存储自定义对象

- A:案例演示
  - 存储Person对象

```java
public static void demo3() {
		TreeSet<Person> ts = new TreeSet<>();
		ts.add(new Person("张三", 23));
		ts.add(new Person("王五", 25));
		ts.add(new Person("李四", 24));
		ts.add(new Person("马哥", 24));
		ts.add(new Person("赵六", 26));
		
		System.out.println('张' + 0);				//根据码表值排序
		System.out.println('李' + 0);
		System.out.println('王' + 0);
		System.out.println('赵' + 0);
		System.out.println('马' + 0);
		System.out.println(ts);
	}
```



```java
public int compareTo(Person o) {				//按照年龄排序
		int num = this.age - o.age;					//年龄作为主要条件比较
		return num == 0 ? this.name.compareTo(o.name) : num;//姓名作为次要条件,只有年龄相同的时候比较姓名
	}
```



treeSet用来比较排序的，因此必须要在Person.java里面实现compareTo的接口，用于排序，否则就会报错。

###  TreeSet保证元素唯一和自然排序的原理和图解

- A:画图演示
  - TreeSet保证元素唯一和自然排序的原理和图

![](http://ogtmd8elu.bkt.clouddn.com/201708012014_914.png)

原理就是一个二叉树的使用，先进来的张三作为根节点，后进来的王五调用compareTo的方法比较年龄，如果年龄大就放在二叉树的右边，否则放在左边，李四先与张三比，如果比张三大就走左边和王五比，比王五小就放右边。依次类推，最后得到的结果从根节点开始。排序就是以年龄从小到大排序。



重写Person里面的compareTo方法

```java
public int compareTo(Person o) {				//按照年龄排序
		int num = this.age - o.age;					//年龄作为主要条件比较
		return num == 0 ? this.name.compareTo(o.name) : num;//姓名作为次要条件,只有年龄相同的时候比较姓名
```



### TreeSet存储自定义对象并遍历练习1

- A:案例演示
  - TreeSet存储自定义对象并遍历练习1(按照姓名排序)

```java
TreeSet<Person> ts = new TreeSet<>();
		ts.add(new Person("张三", 23));
		ts.add(new Person("王五", 25));
		ts.add(new Person("李四", 24));
		ts.add(new Person("马哥", 24));
		ts.add(new Person("赵六", 26));
		
		System.out.println('张' + 0);				//根据码表值(GBK)排序
		System.out.println('李' + 0);
		System.out.println('王' + 0);
		System.out.println('赵' + 0);
		System.out.println('马' + 0);
		System.out.println(ts);
/*
24352
26446
29579
36213
39532
[Person [name=张三, age=23], Person [name=李四, age=24], Person [name=王五, age=25], Person [name=赵六, age=26], Person [name=马哥, age=24]]

*/

```



Person里面重写的conpartTo的方法

```JAVA
public int compareTo(Person o) {				//按照姓名排序
		int num = this.name.compareTo(o.name);		//姓名作为主要条件
  //如果num等于0，就把this.age - o.age的值赋过去，如果不等于的话，就num作为主要条件。
		return num == 0 ? this.age - o.age : num;	//年龄是次要条件
```





### TreeSet存储自定义对象并遍历练习2

- A:案例演示
  - TreeSet存储自定义对象并遍历练习2(按照姓名的长度排序)

```java
TreeSet<Person> ts = new TreeSet<>();
		ts.add(new Person("Tom", 23));
		ts.add(new Person("john", 28));
		ts.add(new Person("black", 13));
		ts.add(new Person("xxxxxx", 33));
		ts.add(new Person("zzzzzz", 33));
		ts.add(new Person("wc", 53));
		ts.add(new Person("ccav", 50));

		System.out.println(ts);
```



```java
@Override
	public int compareTo(Person o) {				//按照姓名的长度排序
		int num = this.name.length() - o.name.length();	//长度作为主要条件
		int num2 = num == 0 ? this.name.compareTo(o.name) : num;//姓名的内容作为次要条件
		return num2 == 0 ? this.age - o.age : num2;		//年龄作为次次要条件
	}
```



###  TreeSet保证元素唯一和比较器排序的原理及代码实现

- A:案例演示
  - TreeSet保证元素唯一和比较器排序的原理及代码实现

```java
package cn.itcast.set;

import java.util.Comparator;
import java.util.TreeSet;

public class Demo4_TreeSet {

	/**
	 * @param args
	 * 需求:比较字符串的长度
	 */
	public static void main(String[] args) {
		demo1();
	}

	public static void demo1() {
		TreeSet<String> ts = new TreeSet<>(new CompareByLen());
		ts.add("nba");
		ts.add("cba");
		ts.add("wnba");
		ts.add("czbk");
		ts.add("heima");
		ts.add("z");
		ts.add("aaaaaaaaaaaaaaaa");
		
		System.out.println(ts);
	}

}

class CompareByLen implements Comparator<String> {

	@Override
	public int compare(String s1, String s2) {			//比较字符串的长度
		int num = s1.length() - s2.length();			//长度作为主要条件
		return num == 0 ? s1.compareTo(s2) : ;		//内容作为次要条件
	}
	
}

```



![](http://ogtmd8elu.bkt.clouddn.com/201708012208_492.png)

s2就是“nba“，根元素，然后一次进行比较。

###  TreeSet原理

- 1.特点
  - TreeSet是用来排序的, 可以指定一个顺序, 对象存入之后会按照指定的顺序排列
- 2.使用方式
  - a.自然顺序(Comparable)
    - TreeSet类的add()方法中会把存入的对象提升为Comparable类型
    - 调用对象的compareTo()方法和集合中的对象比较
    - 根据compareTo()方法返回的结果进行存储
  - b.比较器顺序(Comparator)
    - 创建TreeSet的时候可以制定 一个Comparator
    - 如果传入了Comparator的子类对象, 那么TreeSet就会按照比较器中的顺序排序
    - add()方法内部会自动调用Comparator接口中compare()方法排序
  - c.两种方式的区别
    - TreeSet构造函数什么都不传, 默认按照类中Comparable的顺序(没有就报错ClassCastException)
    - TreeSet如果传入Comparator, 就优先按照Comparator

###  练习

- 在一个集合中存储了无序并且重复的字符串,定义一个方法,让其有序(字典顺序),而且还不能去除重复

  ```java
  public static void main(String[] args) {
  		ArrayList<String> list = new ArrayList<>();
  		list.add("ccc");
  		list.add("ccc");
  		list.add("aaa");
  		list.add("aaa");
  		list.add("bbb");
  		list.add("ddd");
  		list.add("ddd");
  		
  		sort(list);
  		System.out.println(list);
  	}
  	
  	/*
  	 * 对集合中的元素排序,并保留重复
  	 * 1,void
  	 * 2,List<String> list
  	 */
  	public static void sort(List<String> list) {
  		TreeSet<String> ts = new TreeSet<>(new Comparator<String>() {		//定义比较器(new Comparator(){}是Comparator的子类对象)

  			@Override
  			public int compare(String s1, String s2) {						//重写compare方法
  				int num = s1.compareTo(s2);									//比较内容
  				return num == 0 ? 1 : num;									//如果内容一样返回一个不为0的数字即可
  			}
  		});
  		
  		ts.addAll(list);													//将list集合中的所有元素添加到ts中
  		list.clear();														//清空list
  		list.addAll(ts);													//将ts中排序并保留重复的结果在添加到list中
  	}
  ```

###  练习

- 从键盘接收一个字符串, 程序对其中所有字符进行排序,例如键盘输入: helloitcast程序打印:acehillostt


  ```java
  Scanner sc = new Scanner(System.in);			//创建键盘录入对象

  System.out.println("请输入一行字符串:");
  String line = sc.nextLine();					//将键盘录入的字符串存储在line中
  char[] arr = line.toCharArray();				//将字符串转换成字符数组
  TreeSet<Character> ts = new TreeSet<>(new Comparator<Character>() {

  	@Override
  	public int compare(Character c1, Character c2) {
  		//int num = c1.compareTo(c2);
  		int num = c1 - c2;					//自动拆箱
  		return num == 0 ? 1 : num;  //等于1的时候就是保留重复的元素
  	}
  });

  for(char c : arr) {
  	ts.add(c);
  }

  for(Character ch : ts) {
  	System.out.print(ch);
  }
  ```

### 练习

- 程序启动后, 可以从键盘输入接收多个整数, 直到输入quit时结束输入. 把所有输入的整数倒序排列打印.
  ``` java
  Scanner sc = new Scanner(System.in);		//创建键盘录入对象
  	System.out.println("请输入:");
  	TreeSet<Integer> ts = new TreeSet<>(new Comparator<Integer>() {//将比较器传给TreeSet的构造方法

  		@Override
  		public int compare(Integer i1, Integer i2) {
  			//int num = i2 - i1;					//自动拆箱
  			int num = i2.compareTo(i1);
  			return num == 0 ? 1 : num;
  		}
  	});
  	
  	while(true) {
  		String line = sc.nextLine();			//将键盘录入的字符串存储在line中
  		if("quit".equals(line))					//如果字符串常量和变量比较,常量放前面,这样不会出现空指针异常,变量里面可能存储null
  			break;
  		try {
  			int num = Integer.parseInt(line);		//将数字字符串转换成数字
  			ts.add(num);
  		} catch (Exception e) {
  			System.out.println("您录入的数据有误,请输入一个整数");
  		}
  		
  	}
  	
  	for (Integer i : ts) {						//遍历TreeSet集合
  		System.out.println(i);
  	}
  ```

###  键盘录入学生信息按照总分排序后输出在控制台

- A:案例演示

  - 需求：键盘录入5个学生信息(姓名,语文成绩,数学成绩,英语成绩),按照总分从高到低输出到控制台。

  ```
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入5个学生成绩格式是:(姓名,语文成绩,数学成绩,英语成绩)");
    TreeSet<Student> ts = new TreeSet<>(new Comparator<Student>() {
    	@Override
    	public int compare(Student s1, Student s2) {
    		int num = s2.getSum() - s1.getSum();			//根据学生的总成绩降序排列
    		return num == 0 ? 1 : num;
    	}
    });

    while(ts.size() < 5) {
    	String line = sc.nextLine();
    	try {
    		String[] arr = line.split(",");
    		int chinese = Integer.parseInt(arr[1]);				//转换语文成绩
    		int math = Integer.parseInt(arr[2]);				//转换数学成绩
    		int english = Integer.parseInt(arr[3]);				//转换英语成绩
    		ts.add(new Student(arr[0], chinese, math, english));
    	} catch (Exception e) {
    		System.out.println("录入格式有误,输入5个学生成绩格式是:(姓名,语文成绩,数学成绩,英语成绩");
    	}
    	
    }

    System.out.println("排序后的学生成绩是:");
    for (Student s : ts) {
    	System.out.println(s);
    }
  ```

### 总结

- 1.List
  - a.普通for循环, 使用get()逐个获取
  - b.调用iterator()方法得到Iterator, 使用hasNext()和next()方法
  - c.增强for循环, 只要可以使用Iterator的类都可以用
  - d.Vector集合可以使用Enumeration的hasMoreElements()和nextElement()方法
- 2.Set
  - a.调用iterator()方法得到Iterator, 使用hasNext()和next()方法
  - b.增强for循环, 只要可以使用Iterator的类都可以用