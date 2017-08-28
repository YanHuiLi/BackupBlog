---
title: Java学习笔记整理（24）
date: 2017-08-17 08:42:52
tags: [多线程,同步代码块,synchronized,并行并发,死锁，同步方法,线程安全]
categories: J2SE
top: 24
---

---

1. 什么是线程
2. 并行和并发
3. 两种实现线程的方式（好处和弊端）
4. 获取多线程的名字和设置名字
5. 线程休眠
6. join()
7. 同步代码块和同步方法
8. 案例分析

<!--more-->

### 多线程(多线程的引入)

- 1.什么是线程
  - 线程是程序执行的一条路径, 一个进程中可以包含多条线程
  - 多线程并发执行可以提高程序的效率, 可以同时完成多项工作
- 2.多线程的应用场景
  - 红蜘蛛同时共享屏幕给多个电脑
  - 迅雷开启多条线程一起下载
  - QQ同时和多个人一起视频
  - 服务器同时处理多个客户端请求

### 多线程(多线程并行和并发的区别)

- 并行就是两个任务同时运行，就是甲任务进行的同时，乙任务也在进行。(需要多核CPU)
- 并发是指两个任务都请求运行，而处理器只能按受一个任务，就把这两个任务安排轮流进行，由于时间间隔较短，使人感觉两个任务都在运行。
- 比如我跟两个网友聊天，左手操作一个电脑跟甲聊，同时右手用另一台电脑跟乙聊天，这就叫并行。
- 如果用一台电脑我先给甲发个消息，然后立刻再给乙发消息，然后再跟甲聊，再跟乙聊。这就叫并发。

### 多线程(Java程序运行原理和JVM的启动是多线程的吗)

- A:Java程序运行原理
  - Java命令会启动java虚拟机，启动JVM，等于启动了一个应用程序，也就是启动了一个进程。该进程会自动启动一个 “主线程” ，然后主线程去调用某个类的 main 方法。
- B:JVM的启动是多线程的吗
  - JVM启动至少启动了垃圾回收线程和主线程，所以是多线程的。



```java
package cn.itcast.thread;

public class Demo01_Thread {

	/**
	 * JVM是多线程的
	 * 交替执行证明是多线程的
	 */
	public static void main(String[] args) {
      
		for(int i = 0; i < 1000000; i++) {
			new Demo();
		}
		
		for(int i = 0; i < 10000; i++) {
			System.out.println("主方法执行的代码.............................");
		}
	}

}

class Demo {

	@Override
	public void finalize() {
		System.out.println("垃圾被清扫了");
	}
	
}
```



### 多线程(多线程程序实现的方式1)

- 1.继承Thread

  - 定义类继承Thread

  - 重写run方法

  - 把新线程要做的事写在run方法中

  - 创建线程对象

  - 开启新线程, 内部会自动执行run方法


```java
    public class Demo2_Thread {

    	/**
    	 * @param args
    	 */
    	public static void main(String[] args) {
    		MyThread mt = new MyThread();							//4,创建自定义类的对象
    		mt.start();												//5,开启线程
    		
    		for(int i = 0; i < 3000; i++) {                         //主方法中要做的事情
    			System.out.println("bb");
    		}
    	}

    }
    class MyThread extends Thread {									//1,定义类继承Thread
    	public void run() {											//2,重写run方法
    		for(int i = 0; i < 3000; i++) {							//3,将要执行的代码,写在run方法中
    			System.out.println("aaaaaaaaaaaaaaaaaaaaaaaaaaaa");
    		}
    	}
    }
```

### 多线程(多线程程序实现的方式2)

- 2.实现Runnable

  - 定义类实现Runnable接口

  - 实现run方法

  - 把新线程要做的事写在run方法中

  - 创建自定义的Runnable的子类对象

  - 创建Thread对象, 传入Runnable

  - 调用start()开启新线程, 内部会自动调用Runnable的run()方法
```java
package cn.itcast.thread;

public class Demo03_Runnable {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		MyRunnable mr = new MyRunnable();					//4,创建Runnable接口的子类对象 mr = 0x0011
		Thread t = new Thread(mr);							//5,创建线程对象,将Runnable接口的子类对象以参数传递
															//Runnable target = new MyRunnable();
		t.start();											//6,开启线程
		
		for(int i = 0; i < 1000; i++) {
			System.out.println("bb");
		}
	}

}


class MyRunnable implements Runnable {						//1,自定义类实现Runnable

	@Override
	public void run() {										//2,重写run方法
		for(int i = 0; i < 1000; i++) {						//3,将要执行的代码写在run方法中
			System.out.println("aaaaaaaaaaaaaaaaaaaaaa");
		}
	}
}
```

### 多线程(实现Runnable的原理)

- 查看源码
  - 1,看Thread类的构造函数,传递了Runnable接口的引用 
  - 2,通过init()方法找到传递的target给成员变量的target赋值
  - 3,查看run方法,发现run方法中有判断,如果target不为null就会调用Runnable接口子类对象的run方法

###  多线程(两种方式的区别)（面试源码级）

- 查看源码的区别:
  - a.继承Thread : 由于子类重写了Thread类的run(), 当调用start()时, 直接找子类的run()方法
  - b.实现Runnable : 构造函数中传入了Runnable的引用, 成员变量记住了它, start()调用run()方法时内部判断成员变量Runnable的引用是否为空, 不为空编译时看的是Runnable的run(),运行时执行的是子类的run()方法
- 继承Thread
  - 好处是:可以直接使用Thread类中的方法,代码简单
  - 弊端是:如果已经有了父类,就不能用这种方法
- 实现Runnable接口
  - 好处是:即使自己定义的线程类有了父类也没关系,因为有了父类也可以实现接口,而且接口是可以多实现的
  - 弊端是:不能直接使用Thread中的方法需要先获取到线程对象后,才能得到Thread的方法,代码复杂

###  多线程(匿名内部类实现线程的两种方式)

- 继承Thread类

```java
  new Thread() {													//1,new 类(){}继承这个类


  	public void run() {											//2,重写run方法
  		for(int i = 0; i < 3000; i++) {							//3,将要执行的代码,写在run方法中
  			System.out.println("aaaaaaaaaaaaaaaaaaaaaaaaaaaa");
  		}
  	}
  }.start();
```

- 实现Runnable接口
```java
  new Thread(new Runnable(){									//1,new 接口(){}实现这个接口
  	public void run() {											//2,重写run方法
  		for(int i = 0; i < 3000; i++) {							//3,将要执行的代码,写在run方法中
  			System.out.println("bb");
  		}
  	}
  }).start(); 
```

###  多线程(获取名字和设置名字)

- 1.获取名字

  - 通过getName()方法获取线程对象的名字

- 2.设置名字

  - 通过构造函数可以传入String类型的名字

 ```java
  new Thread("康师傅") {

   
    	public void run() {
    		for(int i = 0; i < 1000; i++) {
    			System.out.println(this.getName() + "....aaaaaaaaaaaaaaaaaaaaaaa");
    		}
    	}
    }.start();

    new Thread("周老虎") {
    	public void run() {
    		for(int i = 0; i < 1000; i++) {
    			System.out.println(this.getName() + "....bb");
    		}
    	}
    }.start(); 
 ```

  - 通过setName(String)方法可以设置线程对象的名字

```java
Thread t1 = new Thread() {
    	public void run() {
    		for(int i = 0; i < 1000; i++) {
    			System.out.println(this.getName() + "....aaaaaaaaaaaaaaaaaaaaaaa");
    		}
    	}
    };

    Thread t2 = new Thread() {
    	public void run() {
    		for(int i = 0; i < 1000; i++) {
    			System.out.println(this.getName() + "....bb");
    		}
    	}
    };
    t1.setName("芙蓉姐姐");
    t2.setName("凤姐");

    t1.start();
    t2.start();
```


###  多线程(获取当前线程的对象)

- Thread.currentThread(), 主线程也可以获取


```java
package cn.itcast.thread;

public class Demo06_CurrentThread {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		
		Thread t1 = new Thread(new Runnable() {
			
			@Override
			public void run() {
				for(int i = 0; i < 1000; i++) {						
					System.out.println(Thread.currentThread().getName() + "....aaaaaaaaaaaaaaaaaaaaaa");
				}
			}
		},"张三");
		
		Thread t2 = new Thread(new Runnable() {
			
			@Override
			public void run() {
				for(int i = 0; i < 1000; i++) {						
					System.out.println(Thread.currentThread().getName() + "....bb");
				}
			}
		},"李四");
		
		//t1.setName("马哥");
		//t2.setName("辉哥");
		Thread.currentThread().setName("我是主线程");						//Thread.currentThread写在哪个线程里就代表哪个线程引用
		System.out.println(Thread.currentThread().getName());
		
		t1.start();
		t2.start();
	}

}
```


###  多线程(休眠线程)

- Thread.sleep(毫秒,纳秒), 控制当前线程休眠若干毫秒1秒= 1000毫秒 1秒 = 1000 * 1000 * 1000纳秒 1000000000

```java
  new Thread() {

  		public void run() {
  			for(int i = 0; i < 10; i++) {
  				System.out.println(getName() + "...aaaaaaaaaaaaaaaaaaaaaa");
  				try {
  					Thread.sleep(10);//sleep 10毫秒
  				} catch (InterruptedException e) {
  					e.printStackTrace();
  				}
  			}
  		}
  	}.start();
  	
  	new Thread() {
  		public void run() {
  			for(int i = 0; i < 10; i++) {
  				System.out.println(getName() + "...bb");
  				try {
  					Thread.sleep(10);//sleep 10毫秒
  				} catch (InterruptedException e) {
  					e.printStackTrace();
  				}
  			}
  		}
  	}.start();
```

###  多线程(守护线程)

- setDaemon(), 设置一个线程为守护线程, 该线程不会单独执行, 当其他非守护线程都执行结束后, 自动退出

```java
package cn.itcast.thread;

public class Demo09_SetDaemon {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		Thread t1 = new Thread() {
			public void run() {
				for(int i = 0; i < 50; i++) {
					System.out.println(getName() + "...aaaaaaaaaaaaaaaaa");
				}
			}
		};
		
		Thread t2 = new Thread() {
			public void run() {
				for(int i = 0; i < 2; i++) {
					System.out.println(getName() + "...bb");
				}
			}
		};

		Thread t3 = new Thread() {
			public void run() {
				for(int i = 0; i < 10; i++) {
					System.out.println(getName() + "...cc");
				}
			}
		};
		
		t1.setDaemon(true);						//设置t1为守护线程
                                                //当t2,t3执行完成以后，t1立马结束
		
		t1.start();
		t2.start();
		t3.start();
	}

}

```


###  多线程(加入线程)

- join(), 当前线程暂停, 等待指定的线程执行结束后, 当前线程再继续

- join(int), 可以等待指定的毫秒之后继续

```java
    	final Thread t1 = new Thread() {
    	public void run() {
    		for(int i = 0; i < 50; i++) {
    			System.out.println(getName() + "...aaaaaaaaaaaaaaaaaaaaaa");
    			try {
    				Thread.sleep(10);
    			} catch (InterruptedException e) {
    				e.printStackTrace();
    			}
    		}
    	}
    };

    Thread t2 = new Thread() {
    	public void run() {
    		for(int i = 0; i < 50; i++) {
    			if(i == 2) {
    				try {
    					//t1.join();						//插队,加入级别高执行完毕，强行插队
    					t1.join(30);						//加入,有固定的时间,过了固定时间,继续交替执行
    					Thread.sleep(10);
    				} catch (InterruptedException e) {
    					
    					e.printStackTrace();
    				}
    			}
    			System.out.println(getName() + "...bb");
    		
    		}
    	}
    };

    t1.start();
    t2.start();
```

### 多线程(同步代码块)

- 1.什么情况下需要同步

  - 当多线程并发, 有多段代码同时执行时, 我们希望某一段代码执行的过程中CPU不要切换到其他线程工作. 这时就需要同步.
  - 如果两段代码是同步的, 那么同一时间只能执行一段, 在一段代码没执行结束之前, 不会执行另外一段代码.

- 2.同步代码块

  - 使用synchronized关键字加上一个锁对象来定义一段代码, 这就叫同步代码块

  - 多个同步代码块如果使用相同的锁对象, 那么他们就是同步的

```java
package cn.itcast.syn;

public class Demo1_Synchronized {

	/**
	 *匿名内部类在使用方法中的成员变量的时候，需要用final修饰
	 * @param args
	 */
	public static void main(String[] args) {
		final Printer p = new Printer();
		final Printer p2 = new Printer();
		
		new Thread() {
			public void run() {
				while(true) {
					p.print1();
				}
			}
		}.start();
		
		new Thread() {
			public void run() {
				while(true) {
					p.print2();
				}
			}
		}.start();
	}

}

class Printer {
	//Demo d = new Demo();
	/*
	 * 注意同一把锁
	 * 任意对象都可以当作锁对象
	 * 注意:匿名对象不可以当作锁对象,因为不能保证两个锁对象是同一个对象
	 * 非静态的同步方法,锁对象是this
	 * 静态的同步方法,锁对象是当前类的字节码对象
	 */
	Object obj = new Object();
	public static void print1() {
		//同步代码块
		synchronized(Printer.class) {					//获取锁
			System.out.print("黑");
			System.out.print("马");
			System.out.print("程");
			System.out.print("序");
			System.out.print("员");
			System.out.print("\r\n");//换行
		}												//释放锁
	}
	
	public static synchronized void print2() {
		//synchronized(obj) {
			System.out.print("传");
			System.out.print("智");
			System.out.print("播");
			System.out.print("客");
			System.out.print("\r\n");
		//}
	}
	
}

class Demo{}
```

### 多线程(同步方法)

- 使用synchronized关键字修饰一个方法, 该方法中所有的代码都是同步的
```java
  	class Printer {
  	public static void print1() {
  		synchronized(Printer.class){				//锁对象可以是任意对象,但是被锁的代码需要保证是同一把锁,不能用匿名对象
  			System.out.print("黑");
  			System.out.print("马");
  			System.out.print("程");
  			System.out.print("序");
  			System.out.print("员");
  			System.out.print("\r\n");
  		}
  	}
  	/*
  	 * 不是同一把锁
  	 * 非静态同步函数的锁是:this（默认this）
  	 * 静态的同步函数的锁是:字节码对象
  	 */
  	public static synchronized void print2() {	
  		System.out.print("传");
  		System.out.print("智");
  		System.out.print("播");
  		System.out.print("客");
  		System.out.print("\r\n");
  	}
  }
```

###  多线程线程安全问题(火车站卖票的例子继承thread)

- 多线程并发操作同一数据时, 就有可能出现线程安全问题

- 使用同步技术可以解决这种问题, 把操作数据的代码进行同步, 不要多个线程一起操作

```java
package cn.itcast.syn;

public class Demo2_Ticket {

	/**
	 * @param args
	 * 模拟铁路售票,4个窗口卖100张票
	 */
	public static void main(String[] args) {
		new TicketSeller("窗口1").start();
		new TicketSeller("窗口2").start();
		new TicketSeller("窗口3").start();
		new TicketSeller("窗口4").start();
	}

}

class TicketSeller extends Thread {
	private static int tickets = 100;								//定义了100张票
	
	public TicketSeller(String name) {
		super(name);
	}
	//private static Object obj = new Object();
	public void run() {
		while(true) {
			synchronized(TicketSeller.class) {						//用字节码对象可以保证四条线程是同一个锁对象
				if(tickets <= 0)
					break;
				try {
					Thread.sleep(10);			//线程1睡,线程2睡,线程3睡,线程4睡
				} catch (InterruptedException e) {
					
					e.printStackTrace();
				}
				System.out.println(getName()  +"....这是第" + tickets-- + "号票");
			}
		}
	}
}
```

### 多线程(火车站卖票的例子用实现Runnable接口)

```java
package cn.itcast.syn;

public class Demo3_Ticket {

	/**
	 * @param args
	 * 多次开启一条线程是非法的
	 */
	public static void main(String[] args) {
		Ticket t = new Ticket();
		/*Thread t1 = new Thread(t,"窗口1");
		Thread t2 = new Thread(t,"窗口2");
		Thread t3 = new Thread(t,"窗口3");
		Thread t4 = new Thread(t,"窗口4");
		
		t1.start();
		t2.start();
		t3.start();
		t4.start();*/
		
		new Thread(t,"窗口1").start();
		new Thread(t,"窗口2").start();
		new Thread(t,"窗口3").start();
		new Thread(t,"窗口4").start();
	}

}

class Ticket implements Runnable {
	private int tickets = 100;

//	private Object obj= new Object();  锁不住的
	//obj 和this 都锁不住 因为每一个窗口出票的时候，都会有一个不同的锁生成。
	@Override
	public void run() {
			while(true) {
				synchronized(this) {	
								
				if(tickets <= 0)
					break;
				try {
					Thread.sleep(10);			//线程1睡,线程2睡,线程3睡,线程4睡
				} catch (InterruptedException e) {
					
					e.printStackTrace();
				}
				System.out.println(Thread.currentThread().getName()  +"....这是第" + tickets-- + "号票");
			}
		}
	}
	
}
```



###  多线程(死锁)

- 多线程同步的时候, 如果同步代码嵌套, 使用相同锁, 就有可能出现死锁

  - 尽量不要嵌套使用

```java
    private static String s1 = "筷子左";
    private static String s2 = "筷子右";
    public static void main(String[] args) {
    	new Thread() {
    		public void run() {
    			while(true) {
    				synchronized(s1) {
    					System.out.println(getName() + "...拿到" + s1 + "等待" + s2);
    					synchronized(s2) {
    						System.out.println(getName() + "...拿到" + s2 + "开吃");
    					}
    				}
    			}
    		}
    	}.start();
    	
    	new Thread() {
    		public void run() {
    			while(true) {
    				synchronized(s2) {
    					System.out.println(getName() + "...拿到" + s2 + "等待" + s1);
    					synchronized(s1) {
    						System.out.println(getName() + "...拿到" + s1 + "开吃");
    					}
    				}
    			}
    		}
    	}.start();
    }
```

###  多线程(以前的线程安全的类回顾)

- A:回顾以前说过的线程安全问题

  - 看源码：Vector,StringBuffer,Hashtable,Collections.synchroinzed(xxx)

- 线程安全

  * StringBuffer


  * Hashtable
  * Vector

- 线程不安全

  * HashMap
  * StringBuilder
  * ArrayList
  * LinkedList

- Collections.synchroinzed(xxx) 把不安全的变成


  ###  多线程(总结)