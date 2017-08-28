---
title: Java学习笔记整理（15）
date: 2017-07-26 06:26:40
tags: [J2SE,集合框架,Collection,List]
categories: J2SE
top: 15
---

1. 集合框架使用
2. Collection是最顶层的接口。分为（List和set）
3. List接口
4. 迭代器（Iterator的使用）
5. ListIterator
6. 并发修改异常（面试题）。
7. ArrayList，LinkedList，Vector优缺点（面试）



<!--more-->

###  对象数组的概述和使用

- 案例演示

  - 需求：我有5个学生，请把这个5个学生的信息存储到数组中，并遍历数组，获取得到每一个学生信息。

    - Student[] arr = new Student[5];			//存储学生对象

  ```java
    arr[0] = new Student("张三", 23);
    arr[1] = new Student("李四", 24);
    arr[2] = new Student("王五", 25);
    arr[3] = new Student("赵六", 26);
    arr[4] = new Student("马哥", 20);
    
    for (int i = 0; i < arr.length; i++) {
        System.out.println(arr[i]);
    }
  ```

```java
//学生bean

package Yanhui.bean;

/**
 * Student bean
 * Created by Archer on 2017/7/26.
 */
public class Student {

  private String name;
  private int age;

  public Student() {
  }

  public Student(String name, int age) {
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
    return "Student{" +
        "name='" + name + '\'' +
        ", age=" + age +
        '}';
  }
}

```



```java
package Yanhui.collection;

import Yanhui.bean.Student;

public class Demo1_Array {

	/**
	 *  A:案例演示
	* 需求：我有5个学生，请把这个5个学生的信息存储到数组中，并遍历数组，获取得到每一个学生信息。
	* 通过这个案例:
	* 		数组既可以存储基本数据类型,又可以存储引用数据类型
	* 		数组长度是固定的,不能自动增长
	* 
	* 数组和集合的区别
	* 		1:数组既可以存储基本数据类型,又可以存储引用数据类型
	* 		  集合只能存储引用数据类型(对象)
	* 		2:数组长度是固定的,不能自动增长
	* 		集合的长度的是可变的,可以根据元素的增加而增长
	* 
	 */
	public static void main(String[] args) {
		Student[] arr = new Student[5];					//存储学生对象
		arr[0] = new Student("张三", 23);
		arr[1] = new Student("李四", 24);
		arr[2] = new Student("王五", 25);
		arr[3] = new Student("赵六", 26);
		arr[4] = new Student("马哥", 20);
		
		for (int i = 0; i < arr.length; i++) {
			System.out.println(arr[i]);
		}
		
	}

}

```



### 对象数组的内存图解

- A:画图演示
  - 把学生数组的案例画图讲解
  - 数组和集合存储引用数据类型,存的都是地址值

![](http://ogtmd8elu.bkt.clouddn.com/201707260706_714.png)

集合也是一样的。

### 集合的由来及集合继承体系图

- A:集合的由来
  - 数组长度是固定,当需要增加和减少元素时需要对数组重新定义,太麻烦,java内部给我们提供了集合类,能存储任意对象,长度是可以改变的,随着元素的增加而增加,随着元素的减少而减少 
- B:数组和集合的区别
  - 区别1 : 
    - 数组既可以存储基本数据类型,又可以存储引用数据类型
    - 集合只能存储引用数据类型(对象)
  - 区别2:
    - 数组长度是固定的,不能自动增长
    - 集合的长度的是可变的,可以根据元素的增加而增长
- 数组和集合什么时候用
  - 1,如果元素个数是固定的推荐用数组
  - 2,如果元素个数不是固定的推荐用集合
- C:集合继承体系图

![](http://ogtmd8elu.bkt.clouddn.com/201707260707_669.png)



### Collection集合的基本功能测试

- A:案例演示	

  - 基本功能演示

  ```java
    boolean add(E e)
    boolean remove(Object o)
    void clear()
    boolean contains(Object o)
    boolean isEmpty()
    int size()

  ```

- B:注意:

- collectionXxx.java使用了未经检查或不安全的操作.

  ```
    注意:要了解详细信息,请使用 -Xlint:unchecked重新编译.
    java编译器认为该程序存在安全隐患
    温馨提示:这不是编译失败,所以先不用理会,等学了泛型你就知道了
  ```


  ```

### 集合的遍历之集合转数组遍历

- A:集合的遍历

  - 其实就是依次获取集合中的每一个元素。

- B:案例演示

  - 把集合转成数组，可以实现集合的遍历

  - toArray()*

    ```java
      Collection coll = new ArrayList();
      coll.add(new Student("张三",23));     //Object obj = new Student("张三",23);
      coll.add(new Student("李四",24));
      coll.add(new Student("王五",25));
      coll.add(new Student("赵六",26));
      
      Object[] arr = coll.toArray();              //将集合转换成数组
      for (int i = 0; i < arr.length; i++) {
          Student s = (Student)arr[i];            //强转成Student
          System.out.println(s.getName() + "," + s.getAge());
      }
  ```

```java
package cn.itcast.collection;

import java.util.ArrayList;
import java.util.Collection;

import cn.itcast.bean.Student;

@SuppressWarnings({ "rawtypes", "unchecked" })	//去除黄色提示
public class Demo2_Collection {

	/**
	 * @param args
	 * 定义集合对象,添加学生对象
	 * 将集合转换成数组
	 * 遍历数组打印每个学生的姓名和年龄
	 */
	public static void main(String[] args) {
		//demo1();
		//demo2();
		//demo3();
		//demo4();
		//demo5();
		//demo6();
		//demo7();
		Collection coll = new ArrayList();
		coll.add(new Student("张三",23));				//Object obj = new Student("张三",23)
		coll.add(new Student("李四",24));
		coll.add(new Student("王五",25));
		coll.add(new Student("赵六",26));
		
		Object[] arr = coll.toArray();				//将集合转换成数组
		for (int i = 0; i < arr.length; i++) {
			Student s = (Student)arr[i];			//强转成Student
			System.out.println(s.getName() + "," + s.getAge());
		}
	}

	public static void demo7() {
		Collection coll = new ArrayList();
		coll.add("a");
		coll.add("b");
		coll.add("c");
		coll.add("d");
		
		Object[] arr = coll.toArray();			//集合转化成数组
		
		for (int i = 0; i < arr.length; i++) {
			System.out.println(arr[i]);
		}
	}

	public static void demo6() {
		Collection coll = new ArrayList();
		coll.add("a");
		coll.add("b");
		coll.add("c");
		coll.add("d");
		
		System.out.println(coll.size()); 		//返回集合中元素个数
	}

	public static void demo5() {
		Collection coll = new ArrayList();
		coll.add("a");
		coll.add("b");
		coll.add("c");
		coll.add("d");
		
		boolean b1 = coll.remove("b");			//删除成功返回true
		boolean b2 = coll.remove("z");
		
		System.out.println(b1);
		System.out.println(b2);
	}

	public static void demo4() {
		Collection coll = new ArrayList();
		boolean b1 = coll.isEmpty();			//判断集合是否为空,为空返回true
		System.out.println(b1);
		
		coll.add("a");
		boolean b2 = coll.isEmpty();
		System.out.println(b2);
	}

	public static void demo3() {
		Collection coll1 = new ArrayList();
		coll1.add("a");
		coll1.add("b");
		coll1.add("c");
		coll1.add("d");
		
		//boolean b1 = coll.equals("abcd");		//equals不是集合和元素之间的比较
		//System.out.println(b1);
		
		Collection coll2 = new ArrayList();
		coll2.add("a");
		coll2.add("b");
		coll2.add("c");
		
		boolean b2 = coll1.equals(coll2);		//集合和集合之间的比较,元素一样,顺序一致返回true
		System.out.println(b2);
	}

	public static void demo2() {
		Collection coll = new ArrayList();
		coll.add("a");
		coll.add("b");
		coll.add("c");
		coll.add("d");
		//coll.clear();							//清空集合
		boolean b1 = coll.contains("a");		//判断是否包含传入的元素,包含返回true
		boolean b2 = coll.contains("e");
		System.out.println(b1);
		System.out.println(b2);
		System.out.println(coll);
	}

	public static void demo1() {
		Collection coll = new ArrayList();
		boolean b1 = coll.add(1);				//new Integer(1);自动装箱
		boolean b4 = coll.add(1);				//new Integer(1);自动装箱
		boolean b2 = coll.add("abc");
		boolean b3 = coll.add(new Student("马哥", 18));
		
		System.out.println(b1);
		System.out.println(b2);
		System.out.println(b3);
		System.out.println(b4);
		
		System.out.println(coll);				//ArrayList的间接父类重写了toString方法
	}

}

```











### Collection集合的高级功能测试

- A:案例演示

  - 带All的功能演示

  ```
    boolean addAll(Collection c)
    boolean removeAll(Collection c)
    boolean containsAll(Collection c)
    boolean retainAll(Collection c)
  ```


  ```



​```java
package cn.itcast.collection;

import java.util.ArrayList;
import java.util.Collection;
@SuppressWarnings("ALL")  //去除黄色提示
public class Demo3_CollectionAll {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		//demo1();
		//demo2();
		//demo3();
		Collection coll1 = new ArrayList();
		coll1.add("a");
		coll1.add("b");
		coll1.add("c");
		coll1.add("d");
		
		Collection coll2 = new ArrayList();
		coll2.add("x");
		coll2.add("y");
		coll2.add("z");
		
		boolean b1 = coll1.retainAll(coll2);		//取交集,看调用的集合是否改变,如果没有改变返回false,如果改变返回true
		System.out.println(b1);
		System.out.println(coll1);
	}

	public static void demo3() {
		Collection coll1 = new ArrayList();
		coll1.add("a");
		coll1.add("b");
		coll1.add("c");
		coll1.add("d");
		
		Collection coll2 = new ArrayList();
		coll2.add("a");
		coll2.add("b");
		coll2.add("z");
		
		boolean b1 = coll1.removeAll(coll2);			//删除的是交集,两个集合有交集返回true,并删除交集部分
		System.out.println(b1);
		System.out.println(coll1);
	}

	public static void demo2() {
		Collection coll1 = new ArrayList();
		coll1.add("a");
		coll1.add("b");
		coll1.add("c");
		coll1.add("d");
		
		Collection coll2 = new ArrayList();
		coll2.add("a");
		coll2.add("b");
		coll2.add("c");
		coll2.add("z");
		
		boolean b1 = coll1.containsAll(coll2);			//判断调用的集合是否包含传入的集合
		System.out.println(b1);
	}

	public static void demo1() {
		Collection coll1 = new ArrayList();
		coll1.add("a");
		coll1.add("b");
		coll1.add("c");
		coll1.add("d");
		
		Collection coll2 = new ArrayList();
		coll2.add("e");
		coll2.add("f");
		coll2.add("g");
		coll2.add("h");
		//[a, b, c, d, e, f, g, h]
		coll1.addAll(coll2);				//将集合对象中的每个对象遍历添加到调用集合对象中
		//coll1.add(coll2);					//将整个集合对象当作一个对象,添加到调用的集合中
		
		System.out.println(coll1);
		System.out.println(coll1.size());
	}

}

  ```



###  集合的遍历之迭代器遍历

- A:迭代器概述

  - 集合是用来存储元素,存储的元素需要查看,那么就需要迭代(遍历) 

- B:案例演示

  - 迭代器的使用

    Collection c = new ArrayList();

    ```java
      c.add("a");
      c.add("b");
      c.add("c");
      c.add("d");
      
      Iterator it = c.iterator();                     //获取迭代器的引用
      while(it.hasNext()) {                           //集合中的迭代方法(遍历)
          System.out.println(it.next());
      }

    ```

###  Collection存储自定义对象并遍历

- A:案例演示

  - Collection存储自定义对象并用迭代器遍历

    - Collection c = new ArrayList();

    ```java
      c.add(new Student("张三",23));
      c.add(new Student("李四",24));
      c.add(new Student("王五",25));
      c.add(new Student("赵六",26));
      c.add(new Student("赵六",26));
      
      for(Iterator it = c.iterator();it.hasNext();) {
          Student s = (Student)it.next();                     //向下转型
          System.out.println(s.getName() + "," + s.getAge()); //获取对象中的姓名和年龄
      }
      System.out.println("------------------------------");
      Iterator it = c.iterator();                             //获取迭代器
      while(it.hasNext()) {                                   //判断集合中是否有元素
          //System.out.println(((Student)(it.next())).getName() + "," + ((Student)(it.next())).getAge());
          Student s = (Student)it.next();                     //向下转型
          System.out.println(s.getName() + "," + s.getAge()); //获取对象中的姓名和年龄
      }
    ```



```java
package cn.itcast.collection;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

import cn.itcast.bean.Student;

@SuppressWarnings("ALL")  //去除黄色提示
public class Demo4_Iterator {

 /**
  * @param args
  * 
  */
 public static void main(String[] args) {
  demo1();
  //demo2();
  Collection c = new ArrayList();
  
  c.add(new Student("张三",23));
  c.add(new Student("李四",24));
  c.add(new Student("王五",25));
  c.add(new Student("赵六",26));
  c.add(new Student("赵六",26));
  
  for(Iterator it = c.iterator();it.hasNext();) { //for语句更节省内存.迭代器it用完就被释放了.
   Student s = (Student)it.next();       //向下转型
   System.out.println(s.getName() + "," + s.getAge());  //获取对象中的姓名和年龄
  }
  System.out.println("------------------------------");
  Iterator it = c.iterator();         //获取迭代器
  while(it.hasNext()) {          //判断集合中是否有元素
   //System.out.println(((Student)(it.next())).getName() + "," + ((Student)(it.next())).getAge());
   Student s = (Student)it.next();       //向下转型
   System.out.println(s.getName() + "," + s.getAge());  //获取对象中的姓名和年龄
  }
  
  
  
 }

 public static void demo2() {
  Collection c = new ArrayList();
  c.add("a");
  c.add("b");
  c.add("c");
  c.add("d");
  
  Iterator it = c.iterator();       //获取迭代器的引用
  while(it.hasNext()) {        //集合中的迭代方法(遍历)
   System.out.println(it.next());
  }
 }

 public static void demo1() {
  Collection c = new ArrayList();
  c.add("a");
  c.add("b");
  //c.add("c");
  //c.add("d");
  
  Iterator it = c.iterator();
  boolean b1 = it.hasNext();
  Object obj1 = it.next();
  System.out.println(b1);
  System.out.println(obj1);
  
  boolean b2 = it.hasNext();   //alt + shift + r 改名
  Object obj2 = it.next();
  System.out.println(b2);
  System.out.println(obj2);
  
  boolean b3 = it.hasNext();
  Object obj3 = it.next();   //NoSuchElementException找不到元素异常
  System.out.println(b3);
  System.out.println(obj3);
 }

}
```



### 迭代器的原理及源码解析

- A:迭代器原理
  - 迭代器原理:迭代器是对集合进行遍历,而每一个集合内部的存储结构都是不同的,所以每一个集合存和取都是不一样,那么就需要在每一个类中定义hasNext()和next()方法,这样做是可以的,但是会让整个集合体系过于臃肿,迭代器是将这样的方法向上抽取出接口,然后在每个类的内部,定义自己迭代方式,这样做的好处有二,第一规定了整个集合体系的遍历方式都是hasNext()和next()方法,第二,代码有底层内部实现,使用者不用管怎么实现的,会用即可 
- B:迭代器源码解析
  - 1,在eclipse中ctrl + shift + t找到ArrayList类
  - 2,ctrl+o查找iterator()方法
  - 3,查看返回值类型是new Itr(),说明Itr这个类实现Iterator接口
  - 4,查找Itr这个内部类,发现重写了Iterator中的所有抽象方法 

### List集合的特有功能概述和测试

- A:List集合的特有功能概述
  - void add(int index,E element)
  - E remove(int index)
  - E get(int index)
  - E set(int index,E element)

```java
package Yanhui.list;

import java.util.ArrayList;
import java.util.List;

/**
 * List继承自collection,有了index的特性.
 * Created by Archer on 2017/7/26.
 */
@SuppressWarnings("ALL")
public class Demo01_List {

  public static void main(String[] args) {

//    demo1();
//    demo2();

//    demo3();


    List list=new ArrayList();

    list.add("a");
    list.add("b");
    list.add("b");
    list.add("c");
    list.add("d");

//    list.set(1,"z");
//    System.out.println(list); //[a, z, b, c, d]

    List list1 = list.subList(2, 4); //同样是包含头不包含尾
    System.out.println(list);
    System.out.println(list1);


  }

  private static void demo3() {
    List list=new ArrayList();

    list.add("a");
    list.add("b");
    list.add("b");
    list.add("c");
    list.add("d");

    List list1=new ArrayList();

    list1.add("b");
    list1.add("b");
    list1.add("z");

    boolean b1 = list.removeAll(list1);//在list中删掉list和list1共有的部分,并重新赋值给list

//    Object remove = list1.remove(1);
//    System.out.println(remove);

//    boolean b = list1.remove("b");

    System.out.println(b1);
    System.out.println(list);
  }

  private static void demo2() {
    List list=new ArrayList();

    list.add("a");
    list.add("b");
    list.add("b");
    list.add("c");
    list.add("d");
    Object o = list.get(0);//必须获取存在的对象,索引不存在就IndexOutOfBoundsException异常
    int b = list.indexOf("b");//反向获取索引

    int b1 = list.lastIndexOf("b");//从后向前遍历获取对象的索引
    System.out.println(b1);
    System.out.println(b);
    System.out.println(o);
  }

  private static void demo1() {
    List list=new ArrayList();

    list.add("a");
    list.add("b");
    list.add("c");
    list.add("d");

    List list11=new ArrayList();

    list11.add("e");
    list11.add("f");
    list11.add("g");
    list11.add("h");

//     list.addAll(0,list11);//向指定位置添加list,不指定下标就是往后面添加
//    list11.add(1,"z");//[a, z, b, c, d]
//    list11.add(4,"z");//z<=list11.lenth();相当于在末尾添加一个数据,但是不能隔空,隔空报错,新特性

    System.out.println(list);
  }


}

```





### List集合的特有遍历功能

- A:案例演示

  - 通过size()和get()方法结合使用遍历。

    List list = new ArrayList();

    ```
      list.add("a");
      list.add("b");
      list.add("c");
      list.add("d");
      list.add("e");
      
      //这种遍历只支持list集合,set集合不可以,因为set集合无索引
      for(int i = 0; i < list.size(); i++) {          
          System.out.println(list.get(i));                //根据索引获取值
      }
    ```


    ​```



```java
package Yanhui.test;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

/**
 * Created by Archer on 2017/7/26.
 */

@SuppressWarnings("ALL")

public class Test03_list {

  public static void main(String[] args) {

    List list = new ArrayList();
    list.add("a");
    list.add("ab");
    list.add("ac");
    list.add("ad");

    //只有list里面可以使用get(i)的做法,set里面没有index
    for (int i = 0; i < list.size(); i++) {
      System.out.println(list.get(i));
    }

    //最优化的写法 节省内存.
    for (Iterator iterator=list.iterator();iterator.hasNext();){
      System.out.println(iterator.next());
    }


  }

}

```



### List集合存储学生对象并遍历

- A:案例演示

  - List集合存储学生对象并遍历。

  - 通过size()和get()方法结合使用遍历。

    List list = new ArrayList();

    ```
      list.add(new Student("马哥", 18));
      list.add(new Student("马粉1", 18));
      list.add(new Student("马粉2", 18));
      list.add(new Student("马粉3", 18));
      list.add(new Student("马粉4", 18));
      
      for(int i = 0; i < list.size(); i++) {
          Student s = (Student)list.get(i);
          System.out.println(s.getName() + "," + s.getAge());
      }
    ```


    ​```



```java
package Yanhui.test;

import Yanhui.bean.Student;
import java.util.ArrayList;
import java.util.List;

/**
 * Created by Archer on 2017/7/26.
 * list遍历引用类型
 *
 */

@SuppressWarnings("ALL")

public class Test04_List {

  public static void main(String[] args) {

    List list= new ArrayList( );

    list.add(new Student("张三",23));
    list.add(new Student("张三1",24));
    list.add(new Student("张三2",25));
    list.add(new Student("张三3",26));

    for (int i = 0; i < list.size(); i++) {

      Student o = (Student) list.get(i);

      System.out.println(o.getAge()+" ,"+o.getName());
    }

  }

}

```

### ListIterator的特有功能

```java
package Yanhui.list;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.ListIterator;

/**
 * Created by Archer on 2017/7/26.
 */

@SuppressWarnings("ALL")

public class Demo2_ListIterator {

  public static void main(String[] args) {
    List list = new ArrayList();
    list.add("a");
    list.add("ab");
    list.add("ac");

     ListIterator listIterator=list.listIterator();
    while (listIterator.hasNext()){
      System.out.println(listIterator.next());
    }

    System.out.println("========================================");// 必须先要正着移到最后,才能倒着移动回去.否则没显示
    for( listIterator.hasPrevious();listIterator.hasPrevious();){
      System.out.println(listIterator.previous());
    }


  }
}

```



###  并发修改异常产生的原因及解决方案

- A:案例演示

  - 需求：我有一个集合，请问，我想判断里面有没有"world"这个元素，如果有，我就添加一个"javaee"元素，请写代码实现。

    List list = new ArrayList();

    ```
      list.add("a");
      list.add("b");
      list.add("world");
      list.add("d");
      list.add("e");
      
      /*Iterator it = list.iterator();
      while(it.hasNext()) {
          String str = (String)it.next();
          if(str.equals("world")) {
              list.add("javaee");         //这里会抛出ConcurrentModificationException并发修改异常
          }
      }*/

    ```

- B:ConcurrentModificationException出现

  - 迭代器遍历，集合修改集合

- C:解决方案

  - a:迭代器迭代元素，迭代器修改元素(ListIterator的特有功能add)

  - b:集合遍历元素，集合修改元素

    ListIterator lit = list.listIterator();		//如果想在遍历的过程中添加元素,可以用ListIterator中的add方法

    ```
      while(lit.hasNext()) {
          String str = (String)lit.next();
          if(str.equals("world")) {
              lit.add("javaee");  
              //list.add("javaee");
          }
      }

    ```



```java
package cn.itcast.list;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.ListIterator;

@SuppressWarnings({ "rawtypes", "unchecked" })
public class Demo2_ListIterator {

	/**
	 * @param args
	 * A:案例演示
	 * 需求：我有一个集合，请问，我想判断里面有没有"world"这个元素，如果有，我就添加一个"javaee"元素，请写代码实现。
	 * B:ConcurrentModificationException出现
		* 迭代器遍历，集合修改集合
	 * C:解决方案
	    * a:迭代器迭代元素，迭代器修改元素(ListIterator的特有功能add)
	    * b:集合遍历元素，集合修改元素

	 */
	public static void main(String[] args) {
		//demo1();
		List list = new ArrayList();
		list.add("a");
		list.add("b");
		list.add("world");
		list.add("d");
		list.add("e");
		
		/*Iterator it = list.iterator();
		while(it.hasNext()) {
			String str = (String)it.next();
			if(str.equals("world")) {
				list.add("javaee");			//这里会抛出ConcurrentModificationException并发修改异常
			}
		}*/
		
		ListIterator lit = list.listIterator();		//如果想在遍历的过程中添加元素,可以用ListIterator中的add方法
		while(lit.hasNext()) {
			String str = (String)lit.next();
			if(str.equals("world")) {
				lit.add("javaee");	
				//list.add("javaee");
			}
		}
		System.out.println(list);
	}

	public static void demo1() {
		List list = new ArrayList();
		list.add("a");
		list.add("b");
		list.add("c");
		list.add("d");
		list.add("e");
		
		ListIterator lit = list.listIterator();
		/*while(lit.hasNext()) {
			System.out.println(lit.next());
		}*/
		
		System.out.println("----------------------");
		while(lit.hasPrevious()) {						//倒着遍历
			System.out.println(lit.previous());
		}
	}

}

```

### ListIterator

- boolean hasNext()是否有下一个
- boolean hasPrevious()是否有前一个
- Object next()返回下一个元素
- Object previous();返回上一个元素

###  Vector的特有功能

- A:Vector类概述

- B:Vector类特有功能

  - public void addElement(E obj)

  - public E elementAt(int index)

  - public Enumeration elements()

  - C:案例演示

  - Vector的迭代
 ```java
    Vector v = new Vector();				//创建集合对象,List的子类

   
      v.addElement("a");
      v.addElement("b");
      v.addElement("c");
      v.addElement("d");
      
      //Vector迭代
      Enumeration en = v.elements();          //获取枚举
      while(en.hasMoreElements()) {           //判断集合中是否有元素
          System.out.println(en.nextElement());//获取集合中的元素
      }
 ```



### 数据结构之数组和链表

- A:数组
  - 查询快，修改也快
  - 增删慢
- B:链表
  - 查询慢,修改也慢
  - 增删快

###  数据结构之栈和队列 

* A： 先进后出
* B： 先进先出

### List的三个子类的特点

- A:List的三个子类的特点
 ```
    ArrayList:
        底层数据结构是数组，查询快，增删慢。
        线程不安全，效率高。
    Vector:
        底层数据结构是数组，查询快，增删慢。
        线程安全，效率低。
    Vector相对ArrayList查询慢(线程安全的)
    Vector相对LinkedList增删慢(数组结构)
    LinkedList:
        底层数据结构是链表，查询慢，增删快。
        线程不安全，效率高。
    
    Vector和ArrayList的区别
        Vector是线程安全的,效率低
        ArrayList是线程不安全的,效率高
    ArrayList和LinkedList的区别
        ArrayList底层是数组结果,查询和修改快
        LinkedList底层是链表结构的,增和删比较快,查询和修改比较慢
 ```

- B:List有三个儿子，我们到底使用谁呢?查询多用ArrayList

  ```
    增删多用LinkedList
    如果都多ArrayList
  ```
