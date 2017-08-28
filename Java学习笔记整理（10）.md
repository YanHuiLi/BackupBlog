---
title: Java学习笔记整理（10）
date: 2017-07-19 16:12:42
tags: [J2SE]
categories: [J2SE]
top: 10

---

---

1. package的作用
2. import导包
3. 权限介绍
4. 内部类的介绍与使用
5. 匿名内部类

<!--more-->

### package关键字的概述及作用

- A:为什么要有包
  - 将字节码(.class)进行分类存放 
- B:包的概述
- C:包的作用

帮助分类，精准定位到特定文件。

###  包的定义及注意事项

- A:定义包的格式
  - package 包名;
  - 多级包用.分开即可
- B:定义包的注意事项
  - A:package语句必须是程序的第一条可执行的代码
  - B:package语句在一个java文件中只能有一个
  - C:如果没有package，默认表示无包名
- C:案例演示
  - 包的定义及注意事项

###  带包的类编译和运行

- A:如何编译运行带包的类
  - a:javac编译的时候带上-d即可
    - javac -d . HelloWorld.java
  - b:通过java命令执行。
    - java 包名.HellWord

###  不同包下类之间的访问

- A:案例演示
  - 不同包下类之间的访问

###  import关键字的概述和使用

- A:案例演示
  - 为什么要有import
- B:导包格式
  - import 包名;
  - 注意：
  - 这种方式导入是到类的名称。
  - 虽然可以最后写*，但是不建议。
- C:package,import,class有没有顺序关系(面试题)

###  四种权限修饰符的测试

- A:案例演示

  - 四种权限修饰符

- B:结论


            本类	 同一个包下(子类和无关类)	 不同包下(子类)	 不同包下(无关类)
     private 	Y		
     默认	          Y	                Y
     protected	Y		   Y							Y
     public		Y		   Y							Y				Y

主要是解释protected的含义，就是在不同包下，继承的子类可以使用该方法，其他都不行。



  ###  类及其组成所使用的常见修饰符

- A:修饰符：
  - 权限修饰符：private，默认的，protected，public
  - 状态修饰符：static，final
  - 抽象修饰符：abstract
- B:类：
  - 权限修饰符：默认修饰符，public （protected 针对子类提供的）
  - 状态修饰符：final
  - 抽象修饰符：abstract
  - 用的最多的就是：public
- C:成员变量：
  - 权限修饰符：private，默认的，protected，public
  - 状态修饰符：static，final
  - 用的最多的就是：private
- D:构造方法：
  - 权限修饰符：private，默认的，protected，public
  - 用的最多的就是：public
- E:成员方法：
  - 权限修饰符：private，默认的，protected，public
  - 状态修饰符：static，final
  - 抽象修饰符：abstract
  - 用的最多的就是：public
- F:除此以外的组合规则：
  - 成员变量：public static final
  - 成员方法：
    - public static 
    - public abstract
    - public final

###  内部类概述和访问特点

- A:内部类概述
- B:内部类访问特点
  - a:内部类可以直接访问外部类的成员，包括私有。
  - b:外部类要访问内部类的成员，必须创建对象。
- C:案例演示
  - 内部类极其访问特点

###  成员内部类私有使用

- private

```java
class Demo1InnerClass {
	public static void main(String[] args) {
		//Inner i = new Inner();
		//i.print();
		//外部类名.内部类名 对象名 = 外部类对象.内部类对象;
		//Outer.Inner o = new Outer().new Inner();
		//o.print();

		Outer o = new Outer();
		o.method();
	}
}

class Outer {							//外部类
	//成员内部类
	private int num = 10;
	private class Inner {				//内部类
		public void print() {			//内部类的成员方法
			System.out.println(Outer.this.num);//内部类持有外部类的引用，所以可以拿到
		}
	}

	public void method() {
		Inner i = new Inner();			//创建内部类对象
		i.print();						//调用内部类方法,必须写在方法里面
	}
}

```



###  静态成员内部类

- static
- B:成员内部类被静态修饰后的访问方式是:
  - 外部类名.内部类名 对象名 = new 外部类名.内部类名();

```java
class Demo2InnerClass {
	public static void main(String[] args) {
		//外部类名.内部类名 对象名 = 外部类名.内部类对象;
		/*Outer.Inner oi = new Outer.Inner();
		oi.print();

		Outer.Inner.method();*/

	}
}

class Outer {
	static class Inner {
		public void print() {
			System.out.println("Hello World!");
		}

		public static void method() {
			System.out.println("method");
		}
	}

	/*class Inner2 {							//非静态的成员内部类中不可以定义静态的成员
		public static void run() {
			System.out.println("run");
		}
	}*/
}
```



###  成员内部类的面试题

- A:面试题

- 要求：使用已知的变量，在控制台输出30，20，10。
  ​	
  ```java
  class Outer {
  	public int num = 10;
  	class Inner {
  		public int num = 20;
  		public void show() {
  			int num = 30;
  			System.out.println(?); // num
  			System.out.println(??);// this.num
  			System.out.println(???);//Outer.this.num
  		}
  	}
  }
  class InnerClassTest {
  	public static void main(String[] args) {
  		Outer.Inner oi = new Outer().new Inner();
  		oi.show();
  	}	
  }
  ```

###  

1. 就近原则
2. 使用this拿到内部类持有的变量
3. 使用Outer.this.num拿到outer持有的变量。





### 局部内部类访问局部变量的问题

- A:案例演示
  - 局部内部类访问局部变量必须用final修饰

![](http://ogtmd8elu.bkt.clouddn.com/201707200802_460.png)

###  匿名内部类的格式和理解

- A:匿名内部类

  - 就是内部类的简化写法。

- B:前提：存在一个类或者接口

  - 这里的类可以是具体类也可以是抽象类。

- C:格式：
  ```
  new 类名或者接口名(){

  	重写方法;
  }
  ```

- D:本质是什么呢?

  - 是一个继承了该类或者实现了该接口的子类匿名对象。

- E:案例演示

  - 按照要求来一个匿名内部类

  ```java
  class Demo4InnerClass {
  	public static void main(String[] args) {
  		Outer o = new Outer();
  		o.method();
  	}
  }

  interface Inter {
  	public void print();
  }

  class Outer {
  	/*class Inner implements Inter {					//有名字的内部类实现外部接口
  		public void print() {
  			System.out.println("Hello World!");
  		}
  	}

  	public void method() {
  		//Inner i = new Inner();
  		//i.print();
  		new Inner().print();
  	}*/
  	/*
  	new 接口或类名(){
  		需要重写的方法
  	}
  	*/
  	public void method() {
  		new Inter(){  //实现接口，或者是继承类
  			public void print() {
  				System.out.println("print");
  			}
  		}.print();
  	}
  }

  ```

  ​

###  匿名内部类的方法调用

- A:案例演示
  - 匿名内部类的方法调用

```java
class Demo5InnerClass {
	public static void main(String[] args) {
		Outer o = new Outer();
		o.method();
	}
}

interface Inter {
	public void show1();
	public void show2();
}

class Outer {
	/*class Inner implements Inter {				//有名字的内部类实现外部接口
		public void show1() {
			System.out.println("show1");
		}

		public void show2() {
			System.out.println("show2");
		}
	}

	public void method() {
		Inter i = new Inner();						//父类引用指向子类对象
		i.show1();
		i.show2();
	}*/

	public void method() {
		/*new Inter() {
			public void show1() {
				System.out.println("show1");
			}

			public void show2() {
				System.out.println("show2");
			}
		}.show1();

		new Inter() {
			public void show1() {
				System.out.println("show1");
			}

			public void show2() {
				System.out.println("show2");
			}
		}.show2();*/

		Inter i = new Inter() {					//匿名内部类最好重写一个方法的时候使用
			public void show1() {
				System.out.println("show1");
			}

			public void show2() {
				System.out.println("show2");
			}
		};
		i.show1();
		i.show2();
	}
}
```

###  匿名内部类在开发中的应用

- A:代码如下

 ```java
- //这里写抽象类，接口都行
 
  abstract class Person {
  	public abstract void show();
  }

  class PersonDemo {
  	public void method(Person p) {
  		p.show();
  	}
  }

  class PersonTest {
  	public static void main(String[] args) {
  		//如何调用PersonDemo中的method方法呢?
  		PersonDemo pd = new PersonDemo ();
        pd.method(new Person(){
          public void show(){
            system.out.println("1111111111");
          }
        });
  		
  	}
  }
 ```

###  匿名内部类的面试题

- A:面试题

- 按照要求，补齐代码
  ```java
  interface Inter { void show(); }
  class Outer { //补齐代码
    
  public static Inter method(){
    return new Inter(){
       public void show(){
      system.out.println("helloworld");
     }
      
    };
  }
   
    
  }
  class OuterDemo {
  	public static void main(String[] args) {
  		  Outer.method().show(); //调用一个方法返回的是对象，就是链式编程。
        // Outer.method() 返回的是一个对象。
  	  }
  }
  要求在控制台输出”HelloWorld”
  ```

