---
title: Java学习笔记整理（16）
date: 2017-07-27 14:17:01
tags: [J2SE,集合框架]
categories: J2SE
top: 16
---

1. ArrayList除去重复的
2. LinkedList的使用
3. 泛型的使用（泛型类，方法，静态方法的使用）
4. 接口的泛型
5. 泛型的好处
6. foreach语句的使用
7. 可变参数列表
8. 静态导入（不提倡）
9. Java 1.5的五个新特性
10. 嵌套ArrayList的使用
11. asList方法。

<!--more-->

###  去除ArrayList中重复字符串元素方式

- A:案例演示

  - 需求：ArrayList去除集合中字符串的重复值(字符串的内容相同)

  - 思路：创建新集合方式。



```java

    /**
   *  A:案例演示
   * 需求：ArrayList去除集合中字符串的重复值(字符串的内容相同)
   * 思路：创建新集合方式
   */
  public static void main(String[] args) {
      ArrayList list = new ArrayList();
      list.add("a");
      list.add("a");
      list.add("b");
      list.add("b");
      list.add("b");
      list.add("c");
      list.add("c");
      list.add("c");
      list.add("c");
      
      System.out.println(list);
      ArrayList newList = getSingle(list);
      System.out.println(newList);
  }
  
  /*
   * 去除重复
   * 1,返回ArrayList
   * 2,参数列表ArrayList
   */
  public static ArrayList getSingle(ArrayList list) {
      ArrayList newList = new ArrayList();            //创建一个新集合
      Iterator it = list.iterator();                  //获取迭代器
      while(it.hasNext()) {                           //判断老集合中是否有元素
          String temp = (String)it.next();            //将每一个元素临时记录住
          if(!newList.contains(temp)) {               //如果新集合中不包含该元素
              newList.add(temp);                      //将该元素添加到新集合中
          }
      }
      return newList;                                 //将新集合返回
  }
```







### 去除ArrayList中重复自定义对象元素

- A:案例演示
  - 需求：ArrayList去除集合中自定义对象元素的重复值(对象的成员变量值相同)
- B:注意事项
  - 重写equals()方法的


```java
package cn.itcast.test;

import java.util.ArrayList;
import java.util.Iterator;

import cn.itcast.bean.Person;

@SuppressWarnings({ "rawtypes", "unchecked" })
public class Test2_ArrayList {

	/**
	 *  A:案例演示
	* 需求：ArrayList去除集合中自定义对象元素的重复值(对象的成员变量值相同)
	 */
	public static void main(String[] args) {
		ArrayList list = new ArrayList();
		list.add(new Person("张三", 23));
		list.add(new Person("张三", 23));
		list.add(new Person("张三", 23));
		list.add(new Person("李四", 24));
		list.add(new Person("李四", 24));
		list.add(new Person("李四", 24));
		list.add(new Person("李四", 24));
		
		System.out.println(list);
		//ArrayList newList = getSingle(list);
		//System.out.println(newList);
		boolean b1 = list.remove(new Person("张三", 23));
		System.out.println(b1);
		System.out.println(list);
		
	}

	/*
	 * 去除重复
	 * 1,返回ArrayList
	 * 2,参数列表ArrayList
	 */
	public static ArrayList getSingle(ArrayList list) {
		ArrayList newList = new ArrayList();			//创建一个新集合
    for (Object aList : list) {              //判断老集合中是否有元素
      Person temp = (Person) aList;      //将每一个元素临时记录住
      if (!newList.contains(temp)) {        //如果新集合中不包含该元素
        newList.add(temp);            //将该元素添加到新集合中
      }
    }
		return newList;									//将新集合返回
	}
	
	/*
	 * contains和remove方法底层依赖与equals方法,如果没有重写equals方法,比较的是对象的地址值,重写之后比较的是对象的属性值

       public int indexOf(Object o) {
        if (o == null) {
            for (int i = 0; i < size; i++)
                if (elementData[i]==null)
                    return i;
        } else {
            for (int i = 0; i < size; i++)
                if (o.equals(elementData[i]))
                    return i;
        }
        return -1;
    }


    // String 的equals能判断是因为string重写了 equals方法
    //源码
     public boolean equals(Object anObject) {
        if (this == anObject) {
            return true;
        }
        if (anObject instanceof String) {
            String anotherString = (String)anObject;
            int n = value.length;
            if (n == anotherString.value.length) {
                char v1[] = value;
                char v2[] = anotherString.value;
                int i = 0;
                while (n-- != 0) {
                    if (v1[i] != v2[i])
                        return false;
                    i++;
                }
                return true;
            }
        }
        return false;
    }


	 */
}

```



​    

```java
//Person.java 里面重写的equals方法

public boolean equals(Object obj) {
		Person p = (Person)obj;
		return this.name.equals(p.name) && this.age == p.age;
	}
```




###  LinkedList的特有功能

- A:LinkedList类概述
- B:LinkedList类特有功能
  - public void addFirst(E e)及addLast(E e)
  - public E getFirst()及getLast()
  - public E removeFirst()及public E removeLast()
  - public E get(int index);



```java
package cn.itcast.list;

import java.util.LinkedList;

@SuppressWarnings({ "rawtypes", "unchecked" })
public class Demo3_LinkedList {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		LinkedList list = new LinkedList();
		list.addFirst("a");
		list.addFirst("b");
		list.addFirst("c");
		list.addFirst("d");						//在第一个位置添加
		//list.addLast("e");						//在末尾追加
		System.out.println(list);
		
		Object obj1 = list.getFirst();			//获取集合中的第一个元素
		System.out.println(obj1);
		Object obj2 = list.getLast();			//获取集合中的最后一个元素
		System.out.println(obj2);
		
		System.out.println(list);
		
		Object obj3 = list.removeFirst();		//删除集合中的第一个元素
		System.out.println(obj3);
		Object obj4 = list.removeLast();		//删除集合中的最后一个元素
		System.out.println(obj4);
		
		System.out.println(list);
		
	}
/*dcba*/
}

```



###  栈和队列数据结构

- 栈
  - 先进后出 
- 队列
  - 先进先出

#### LinkedList模拟栈和队列的数据结构

```java
package Yanhui.bean;

import java.util.LinkedList;

/**
 * Created by Archer on 2017/7/28.
 */
@SuppressWarnings("ALL")
public class Stack {

  private LinkedList linkedList=new LinkedList();

  public void  in(Object o){
     linkedList.addLast(o);
  }

  public  Object out(){
   return linkedList.removeLast(); //出去的方式不同，队列的换成 removeFirst()即可。
  }

  public  boolean isEmpty(){
    return linkedList.isEmpty();
  }

public  LinkedList  Show(){
  return linkedList;
  }


}

```





```java
package Yanhui.test;

import Yanhui.bean.Stack;
import java.util.LinkedList;

/**
 * 用linkedlist模拟进出栈
 * Created by Archer on 2017/7/28.
 */
@SuppressWarnings("ALL")
public class Test3_LinkedList {

  public static void main(String[] args) {

//    demo1();

    Stack stack=new Stack();
    stack.in("a");
    stack.in("b");
    stack.in("c");
    stack.in("d");

    System.out.println(stack.Show());

    while (!stack.isEmpty()){

      System.out.println(stack.out());

    }


  }

  private static void demo1() {
    LinkedList linkedList=new LinkedList();

    linkedList.add("a");
    linkedList.add("b");
    linkedList.add("c");
    linkedList.add("d");

    System.out.println(linkedList);

    while ( !linkedList.isEmpty()) {

      Object remove = linkedList.removeLast();
      System.out.println(remove);
    }
  }

}
```







###  LinkedList模拟栈数据结构的集合并测试

- A:案例演示

  - 需求：请用LinkedList模拟栈数据结构的集合，并测试

  - 创建一个类将Linked中的方法封装
```java
  public class Stack {

          private LinkedList list = new LinkedList();     //创建LinkedList对象
    
          public void in(Object obj) {
              list.addLast(obj);                          //封装addLast()方法
          }
          
          public Object out() {
              return list.removeLast();                   //封装removeLast()方法
          }
          
          public boolean isEmpty() {
              return list.isEmpty();                      //封装isEmpty()方法
          }
      }
```

###  泛型概述和基本使用

- A:泛型概述
- B:泛型好处
  - 提高安全性(将运行期的错误转换到编译期) 
  - 省去强转的麻烦
- C:泛型基本使用
  - <>中放的必须是引用数据类型 
- D:泛型使用注意事项
  - 前后的泛型必须一致,或者后面的泛型可以省略不写(1.7的新特性菱形泛型)  



````java
package cn.itcast.list;

import java.util.ArrayList;
import java.util.Iterator;

import cn.itcast.bean.Person;

public class Demo4_ArrayList {

	/**
	 * @param args
	 * <>泛型1.5版本出现
	 * <>中放的是引用数据类型
	 * 八种基本类型必须要用他们的包装类，才能使用。
	 */
	public static void main(String[] args) {
		//demo1();
		ArrayList<Person> list = new ArrayList<>();			//1.7版本出现,菱形泛型
		list.add(new Person("张三", 23));
		list.add(new Person("李四", 24));
		list.add(new Person("王五", 25));
		
		Iterator<Person> it = list.iterator();
		while(it.hasNext()) {
			Person p = it.next();							//省去了强转的麻烦
			System.out.println(p.getName() + "," + p.getAge());
		}
	}

	public static void demo1() {
		//ArrayList<String> list = new ArrayList<String>();
		ArrayList list2 = new ArrayList<>();
		list2.add("abc");
		list2.add(123);
		list2.add(new Person("张三", 23));
		
		Iterator it = list2.iterator();
		while(it.hasNext()) {
			Person p = (Person)it.next();
			System.out.println(p.getName() + "," + p.getAge());
		}
	}

}

````



###  ArrayList存储字符串和自定义对象并遍历泛型版

- A:案例演示
  - ArrayList存储字符串并遍历泛型版

```java
package cn.itcast.test;

import java.util.ArrayList;
import java.util.Iterator;

public class Test4_ArrayList {

	/**
	 *  A:案例演示
	* ArrayList存储字符串并遍历泛型版
	 */
	public static void main(String[] args) {
		ArrayList<String> list = new ArrayList<>();	//创建集合对象
		list.add("a");
		list.add("b");
		list.add("c");
		list.add("d");
		
		Iterator<String> it = list.iterator();		//获取迭代器
		while(it.hasNext()) {						//判断集合中是否有元素
			String s = it.next();					//获取集合中的元素
			System.out.println(s);					//打印
		}
	}

}

```



```java
package cn.itcast.test;

import java.util.ArrayList;
import java.util.Iterator;

import cn.itcast.bean.Person;
import cn.itcast.bean.Student;

public class Test5_ArrayList {

	/**
	 *  A:案例演示
	* ArrayList存储自定义对象并遍历泛型版
	* 1,泛型前面和后面的数据类型必须一致,或者可以省略后面(菱形泛型)
	* 2,泛型可以定义成Object,但是没有意义
	 */
	public static void main(String[] args) {
		demo1();
//		ArrayList<Object> list = new ArrayList<>();
//    System.out.println(list);
  }

	public static void demo1() {
		ArrayList<Person> list = new ArrayList<>();
		list.add(new Person("张三", 23));
		list.add(new Person("李四", 24));
		list.add(new Person("王五", 25));
		list.add(new Person("赵六", 26));
		list.add(new Student("马哥", 18));				//父类引用指向子类对象 student 继承自Person类同样可以使用
		
		Iterator<Person> it = list.iterator();
		while(it.hasNext()) {
			Person p = it.next();
			System.out.println(p.getName() + "," + p.getAge());
		}
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
	public void setAge(int age) {
		this.age = age;
	}
	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}
	@Override
	public boolean equals(Object obj) {
		Person p = (Person)obj;
		return this.name.equals(p.name) && this.age == p.age;
	}
	
	/*
	 * Preson p1 = new Person("张三",23);
	 * Preson p2 = new Person("李四",24);
	 * p1.equals(p2);
	 */
}

```



```java
package cn.itcast.bean;

public class Student extends Person {

	public Student() {
		super();
		
	}

	public Student(String name, int age) {
		super(name, age);
		
	}
	
}

```



###  泛型的由来

- A:案例演示
  - 泛型的由来:通过Object转型问题引入
  - 早期的Object类型可以接收任意的对象类型，但是在实际的使用中，会有类型转换的问题。也就存在这隐患，所以Java提供了泛型来解决这个安全问题。

### 泛型类的概述及使用

- A:泛型类概述<T>
  - 把泛型定义在类上
- B:定义格式
  - public class 类名<泛型类型1,…>
  - C:注意事项
  - 泛型类型必须是引用类型
- D:案例演示
  - 泛型类的使用



```java
package Yanhui.test;

/**
 * Created by Archer on 2017/7/28.
 */
public class Tool<E> {

  private E e;

  public E getObject() {
    return e;
  }

  public void setObject(E e) {
    this.e = e;
  }
}
```



```java
package Yanhui.bean;

/**
 * Created by Archer on 2017/7/28.
 */
public class Student extends Person {

  public Student() {
  }

  public Student(String name, int age) {
    super(name, age);

  }

  @Override
  public String toString() {

//    return super.toString();
    return  "学生的名字是："+ super.getName()+",  "+"年龄是："+super.getAge();

  }
}

```



```java
package Yanhui.list;

import Yanhui.bean.Student;
import Yanhui.bean.Worker;
import Yanhui.test.Tool;

/**
 * Created by Archer on 2017/7/28.
 */
@SuppressWarnings("ALL")
public class demo5_ArrayList {

  public static void main(String[] args) {
    Tool tool=new Tool();
    tool.setObject(new Student("李四",24));
    Student object = (Student) tool.getObject();
    System.out.println(object);

  }


}

```



###  泛型方法的概述和使用

- A:泛型方法概述
  - 把泛型定义在方法上
  - B:定义格式
  - public <泛型类型> 返回类型 方法名(泛型类型 变量名)
- C:案例演示
  - 泛型方法的使用

```java
//tootl.java 添加次方法
public  static<T> void  print(T e){ //相当于优先制定静态方法的泛型
    System.out.println(e);    //当需要使用的时候，传入什么引用类型就是什么泛型。
                               //因为本质上，泛型就是用来避免输出型异常，控制在调试期就出现的
  }
```

```java
//主函数中调试  

Tool.print("abc");
```

### 泛型接口的概述和使用

- A:泛型接口概述
  - 把泛型定义在接口上
  - B:定义格式
  - public interface 接口名<泛型类型>
- C:案例演示
  - 泛型接口的使用

```java
package cn.itcast.test;

public class Demo implements Inter<String>{		//开发推荐用第一种,实现接口的时候,你已经可以明确具体是什么类型了

	@Override
	public void show(String t) {
		System.out.println(t);
	}

}

/*public class Demo<T> implements Inter<T> {

	@Override
	public void show(T t) {
	}
	
}*/

interface Inter<T> {
	public void show(T t);
}
```



```java
Demo d = new Demo();
d.show("abc");
```

### 泛型高级之通配符

- A:泛型通配符<?>
  - 任意类型，如果没有明确，那么就是Object以及任意的Java类了
- B:? extends E
  - 向下限定，E及其子类
- C:? super E
  - 向上限定，E及其父类



```java
package cn.itcast.list;

import java.util.ArrayList;
import java.util.Iterator;

import cn.itcast.bean.Person;
import cn.itcast.bean.Student;

public class Demo06_ArrayList {

	/**
	 * @param args
	 * ?泛型的通配符
	 * ? extends E
	 * 可以添加E类型以及E的子类型,E是上边界(固定上边界)
	 */
	public static void main(String[] args) {
		//ArrayList<Student> list = new ArrayList<>();
		//print(list);
		
		ArrayList<Person> list1 = new ArrayList<>();
		list1.add(new Person("张三", 23));
		list1.add(new Person("李四", 24));
		list1.add(new Person("王五", 25));
		
		ArrayList<Student> list2 = new ArrayList<>();
		list2.add(new Student("马哥", 18));
		list2.add(new Student("马哥粉丝团团长", 20));
		list2.add(new Student("马哥粉丝团秘书", 16));
		
		list1.addAll(list2);
//    list2.addAll(list1); 报错了
    /*
    person类可以接受student类以及student的子类，但是反之不行
    所以？ extend E 叫做固定上边界
     */
		System.out.println(list1);
	}
	
	public static<T> void print(ArrayList<T> list) {
		Iterator<T> it = list.iterator();
		while(it.hasNext()) {
			//Object obj = it.next();
			T s = it.next();
		}
	}
}

```



###  增强for的概述和使用

- A:增强for概述

  - 简化数组和Collection集合的遍历

- B:格式：
```
for(元素数据类型 变量 : 数组或者Collection集合) {

        使用变量即可，该变量就是元素
    }
```




- C:案例演示

  - 数组，集合存储元素用增强for遍历

- D:好处

  - 简化遍历



```java
package cn.itcast.list;

import java.util.ArrayList;

public class Demo07_Foreach {

	/**
	 * @param args
	 * Jdk1.5foreach增强for循环,底层就是迭代器实现 Iterator来实现的
	 * for(临时变量 : 容器名) {
	 * 	输出临时变量
	 * }
	 */
	public static void main(String[] args) {
		ArrayList<String> list = new ArrayList<>();
		list.add("a");
		list.add("b");
		list.add("c");
		list.add("d");
		
		/*for(String s : list) {
			System.out.println(s);
		}*/
//		for (String string : list) {
//			System.out.println(string);
//		}

    for (String s : list) {
      System.out.println(s);
    }

    int[] arr = {11,22,33,44,55}; //数组里面没有Iterator Foreach加强了
		for (int i : arr) {
			System.out.println(i);
		}
	}

}
```



###  ArrayList存储字符串和自定义对象并遍历增强for版

- A:案例演示

  - ArrayList存储字符串并遍历增强for版

```java
  ArrayList<String> list = new ArrayList<>();


      list.add("a");
      list.add("b");
      list.add("c");
      list.add("d");
      
      for(String s : list) {
          System.out.println(s);
      }
```



```java
package cn.itcast.test;

import java.util.ArrayList;
import java.util.Iterator;

import cn.itcast.bean.Person;

public class Test7_Foreach {

	/**
	 * @param args
	 * A:案例演示
	 * ArrayList存储自定义对象并遍历增强for版
	 */
	public static void main(String[] args) {
		ArrayList<Person> list = new ArrayList<>();
		list.add(new Person("张三", 23));
		list.add(new Person("李四", 24));
		list.add(new Person("王五", 25));
		list.add(new Person("赵六", 26));
		
		for(Person p : list) {						//增强for循环
			System.out.println(p.getName() + "," + p.getAge());
		}
		
		Iterator<Person> it = list.iterator();		//迭代器 麻烦
		while(it.hasNext()) {
			Person p = it.next();
			System.out.println(p.getName() + "," + p.getAge());
		}
	}

}

```






### 三种迭代的能否删除

- 普通for循环,可以删除.
- 迭代器,可以删除,但是必须使用迭代器自身的remove方法,否则会出现并发修改异常
- 增强for循环不能删除

### 静态导入的概述和使用

- A:静态导入概述
- B:格式：
  - import static 包名….类名.方法名;
  - 可以直接导入到方法的级别
- C:注意事项
  - 方法必须是静态的,如果有多个同名的静态方法，容易不知道使用谁?这个时候要使用，必须加前缀。由此可见，意义不大，所以一般不用，但是要能看懂。

```java
package cn.itcast.list;

import static java.util.Arrays.sort; //导入静态类
import static java.util.Arrays.toString;

public class Demo08_StaticImport {

	/**
	 * @param args
	 * Jdk1.5的新特性
	 * 1,自动拆装箱
	 * 2,泛型
	 * 3,增强for循环
	 * 4,静态导入:其实导入的是静态的方法,只要是静态的方法都可以用静态导入(开发严重不推荐)
	 */
	public static void main(String[] args) {
		int[] arr = {55,44,33,22,11};
		sort(arr);							//排序
//		System.out.println(toString(arr));	//数组转换成字符串并打印,和其他toString方法冲突
	}

}

```



### 可变参数的概述和使用

- A:可变参数概述
  - 定义方法的时候不知道该定义多少个参数
- B:格式
  - 修饰符 返回值类型 方法名(数据类型…  变量名){}
- C:注意事项：
  - 这里的变量其实是一个数组
  - 如果一个方法有可变参数，并且有多个参数，那么，可变参数肯定是最后一个。

```java
public static void print(int ... arr) {  //可变参数,其实就是一个可以变化的数组，功能更强大。
                                         //arr后面不能再添加参数，jvm无法识别，添加到前面可以
 for (int i : arr) {
  System.out.println(i);
 }
}
```







###  Arrays工具类的asList()方法的使用

- A:案例演示
  - Arrays工具类的asList()方法的使用
  - Collection中toArray(T[] a)泛型版的集合转数组

```java
public static void demo2() {
		String[] arr = {"a","b","c"};
		//System.out.println(arr.toString()); 数组不能直接打印，打印出来的是存的地址。而不是内容
		List<String> list = Arrays.asList(arr);		//数组转换成集合不能改变长度,但是可以应用集合中其他的方法
		System.out.println(list);
		list.add("d");
  }
```



```java
private static void demo3() {
    int[] arr = {11,22,33,44,55};
    List<int[]> list = Arrays.asList(arr);
    System.out.println(list);

    Integer[] arr2 = {11,22,33,44,55};			//11,22,33,44,55是Integer类型,自动装箱
    List<Integer> list2 = Arrays.asList(arr2);	//把数组转换成集合,必须是引用数据类型的数组,否则会将整个数组当作一个对象
    System.out.println(list2);
  }

  public static void demo2() {
		String[] arr = {"a","b","c"};
		//System.out.println(arr.toString()); 数组不能直接打印，打印出来的是存的地址。而不是内容
		List<String> list = Arrays.asList(arr);		//数组转换成集合不能改变长度,但是可以应用集合中其他的方法
		System.out.println(list);
		list.add("d");
  }
```





###  集合嵌套之ArrayList嵌套ArrayList

- A:案例演示
  - 集合嵌套之ArrayList嵌套ArrayList





```java
package cn.itcast.list;

import java.util.ArrayList;

import cn.itcast.bean.Student;

public class Demo10_ArrayListArrayList {

	/**
	 * @param args
	 * 集合嵌套集合
	 */
	public static void main(String[] args) {
		ArrayList<ArrayList<Student>> list = new ArrayList<>();	//集合中添加集合
		
		//第一个班级
		ArrayList<Student> first = new ArrayList<>();
		first.add(new Student("马哥", 18));
		first.add(new Student("马哥第一批粉丝团1", 88));
		first.add(new Student("马哥第一批粉丝团2", 20));
		
		//第二个班级
		ArrayList<Student> second = new ArrayList<>();
		second.add(new Student("马哥第二批粉丝团1", 5));
		second.add(new Student("马哥第二批粉丝团2", 100));
		
		//将第一个班级和第二个班级添加到list里
		list.add(first);
		list.add(second);
		
		for(ArrayList<Student> li : list) {
			for(Student s : li) {
				System.out.print(s + " ");
			}
			System.out.println();
		}
	}
	
	
}
```