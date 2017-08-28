---
title: Java学习笔记整理（20）
date: 2017-08-07 09:01:42
tags: [J2SE]
categories: J2SE
top: 20
---

---

1. IO流的顶层四个基类.
   * InputStream,outputStream(字节流抽象父类)
   * Reader,Writer(字符流抽象父类)
   * 导包-》异常处理-》释放资源
2. 四种拷贝的方法
   * 单个字节拷贝，效率太低
   * 大数组拷贝，内存溢出
   * 小数组（1024*8）（推荐）
   * 装饰着模式的BufferedInputStream和BufferOutputStream缓冲流（推荐）
3. BufferedInputStream和BufferOutputStream缓冲流原理
4. flush方法和close方法的区别
5. 字节流读中文的问题
6. 1.6和1.7的标准的异常处理方式

<!--more-->

###  IO流概述及其分类

- 1.概念
  - IO流用来处理设备之间的数据传输
  - Java对数据的操作是通过流的方式
  - Java用于操作流的类都在IO包中
  - 流按流向分为两种：输入流，输出流。
  - 流按操作类型分为两种：
    - 字节流 : 字节流可以操作任何数据,因为在计算机中任何数据都是以字节的形式存储的
    - 字符流 : 字符流只能操作纯字符数据，比较方便。
- 2.IO流常用父类
  - 字节流的抽象父类：
    - InputStream 
    - OutputStream
  - 字符流的抽象父类：
    - Reader 
      - Writer
- 3.IO程序书写
  - 使用前，导入IO包中的类
  - 使用时，进行IO异常处理
  - 使用后，释放资源

### FileInputStream

* read()一次读取一个字节

```java
package cn.itcast.io;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
public class Demo1_FileInputStream {
	/**
	 * @param args
	 * @throws IOException 
	 * read()方法每读一次返回的是一个字节,为什么不用byte接收,而用int接收呢?
	 * 因为字节输入流可以操作任意类型的文件,比如图片音频等,这些文件底层都是以二进制形式的存储的,如果每次读取都返回byte,有可能在读到中间的时候遇到111111111
	 * 那么这11111111是byte类型的-1,我们的程序是遇到-1就会停止不读了,后面的数据就读不到了,所以在读取的时候用int类型接收,如果11111111会在其前面补上
	 * 24个0凑足4个字节,那么byte类型的-1就变成int类型的255了这样可以保证整个数据读完,而结束标记的-1就是int类型
	 */
	public static void main(String[] args) throws IOException {
		//demo1();
		FileInputStream fis = new FileInputStream("aaa.txt");	//创建一个文件输入流对象,并关联aaa.txt
		int b;													//定义变量,记录每次读到的字节
		while((b = fis.read()) != -1) {							//将每次读到的字节赋值给b并判断是否是-1
			System.out.println(b);								//打印每一个字节
		}
		
		fis.close();											//关闭流释放资源
	}

	public static void demo1() throws FileNotFoundException, IOException {
		FileInputStream fis = new FileInputStream("aaa.txt");	//创建一个文件输入流对象,并关联aaa.txt
		int x = fis.read();										//每读取一次就向后移动一次
		System.out.println(x);
		int y = fis.read();
		System.out.println(y);
		int z = fis.read();
		System.out.println(z);
		int a = fis.read();
		System.out.println(a);
		int b = fis.read();
		System.out.println(b);
		fis.close();
	}

}
```



### FileInputStream返回值为什么是int

- read()方法读取的是一个字节,为什么返回是int,而不是byte
- 因为字节输入流可以操作任意类型的文件,比如图片音频等,这些文件底层都是以二进制形式的存储的,如果每次读取都返回byte,有可能在读到中间的时候遇到111111111
- 那么这11111111是byte类型的-1,我们的程序是遇到-1就会停止不读了,后面的数据就读不到了,所以在读取的时候用int类型接收,如果11111111会在其前面补上24个0凑足4个字节,那么byte类型的-1就变成int类型的255了这样可以保证整个数据读完,而结束标记的-1就是int类型。


### FileOutputStream

- write()一次写出一个字

```java
package cn.itcast.io;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class Demo2_FileOutputStream {

	/**
	 * @param args
	 * @throws IOException 
	 * @throws IOException 
	 * FileOutputStream fos = new FileOutputStream("bbb.txt");	
	 * 如果没有bbb.txt,会创建一个新bbb.txt
	 * 如果有bbb.txt,会将bbb.txt清空
	 * 
	 * FileOutputStream fos = new FileOutputStream("bbb.txt",true);
	 * 不会将bbb.txt清空,如果再次写入会续写
	 */
	public static void main(String[] args) throws IOException {
		FileOutputStream fos = new FileOutputStream("bbb.txt",true);	//如果没有bbb.txt,会创建出一个
		fos.write(97);						//虽然写出的是一个int数,但是在写出的时候会将前面的24个0去掉,所以写出的一个byte
		fos.write(98);
		fos.write(99);
		fos.close();
	}

}
```

### FileOutputStream追加

- A:案例演示
  - FileOutputStream的构造方法写出数据如何实现数据的追加写入
```java
  FileOutputStream fos = new FileOutputStream("bbb.txt",true);//如果没有bbb.txt,会创建出一个

      //fos.write(97);						//虽然写出的是一个int数,但是在写出的时候会将前面的24个0去掉,所以写出的一个byte
      fos.write(98);
      fos.write(99);
      fos.close();
```

### 拷贝图片

- FileInputStream读取

  ```java
  package cn.itcast.io;

  import java.io.FileInputStream;
  import java.io.FileNotFoundException;
  import java.io.FileOutputStream;
  import java.io.IOException;

  public class Demo3_Copy {

  	/**
  	 * @param args
  	 * @throws IOException 
  	 * 逐个字节的拷贝
  	 */
  	public static void main(String[] args) throws IOException {
  		//demo1();
  		FileInputStream fis = new FileInputStream("致青春.mp3");	//创建输入流对象,关联a.jpg
  		FileOutputStream fos = new FileOutputStream("copy.mp3");//创建输出流对象,关联b.jpg
  		
  		int b;
  		while((b = fis.read()) != -1) {
  			fos.write(b);
  		}
  		
  		fis.close();
  		fos.close();
  	}

  	public static void demo1() throws FileNotFoundException, IOException {
  		FileInputStream fis = new FileInputStream("a.jpg");	//创建输入流对象,关联a.jpg
  		FileOutputStream fos = new FileOutputStream("b.jpg");//创建输出流对象,关联b.jpg
  		
  		int b;
  		while((b = fis.read()) != -1) {
  			fos.write(b);
  		}
  		
  		fis.close();
  		fos.close();
  	}
  }
  ```

### 拷贝音频文件画原理图

- A:案例演示
  - 字节流一次读写一个字节复制音频
- 弊端:效率太低

```java
package cn.itcast.io;

import java.io.FileInputStream;

import java.io.FileOutputStream;
import java.io.IOException;

public class Demo3_Copy {

	/**
	 * @param args 数组
	 * @throws IOException 
	 * 逐个字节的拷贝
	 */
	public static void main(String[] args) throws IOException {
//		demo1();
		FileInputStream fis = new FileInputStream("致青春.mp3");	//创建输入流对象,关联a.jpg
		FileOutputStream fos = new FileOutputStream("copy.mp3");//创建输出流对象,关联b.jpg
		
		int b;
		while((b = fis.read()) != -1) {
			fos.write(b);
		}
		
		fis.close();
		fos.close();
	}

	private static void demo1() throws IOException {
		FileInputStream fis = new FileInputStream("a.jpg");	//创建输入流对象,关联a.jpg
		FileOutputStream fos = new FileOutputStream("b.jpg");//创建输出流对象,关联b.jpg
		
		int b;
		while((b = fis.read()) != -1) {
			fos.write(b);
		}
		
		fis.close();
		fos.close();
	}
}
```



###  字节数组拷贝之available()方法

- A:案例演示
  - int read(byte[] b):一次读取一个字节数组
  - write(byte[] b):一次写出一个字节数组
  - available()获取读的文件所有的字节个数
- 弊端:有可能会内存溢出 

```java
  FileInputStream fis = new FileInputStream("致青春.mp3");
  FileOutputStream fos = new FileOutputStream("copy.mp3");
  byte[] arr = new byte[fis.available()];					//根据文件大小做一个字节数组
  fis.read(arr);											//将文件上的所有字节读取到数组中
  fos.write(arr);											//将数组中的所有字节一次写到了文件上
  fis.close();
  fos.close();
```



![](http://ogtmd8elu.bkt.clouddn.com/201708071550_387.png "大数组拷贝")



 ### 定义小数组

- write(byte[] b)
- write(byte[] b, int off, int len)写出有效的字节个数

```java
public static void main(String[] args) throws IOException {
		demo1();
		//demo2();
//        demo3();
	}



public static void demo1() throws FileNotFoundException, IOException {
		FileInputStream fis = new FileInputStream("aaa.txt");
		byte[] arr = new byte[2];
		int x = fis.read(arr);							//将文件上的字节数据读取到字节数组中
														//返回读取有效的字节个数
		for (byte b : arr) {
			System.out.println(b);
		}
		
		System.out.println(x);
		
		System.out.println("-----------------------");
		
		int y = fis.read(arr);
		for (byte b : arr) {
			System.out.println(b);
		}
		System.out.println(y);
		
		System.out.println("-----------------------");
		int z = fis.read(arr);						//如果文件上有数据读取的是字节数据,如果没有读取的-1
		for (byte b : arr) {
			System.out.println(b);
		}
		System.out.println(z);
		fis.close();
	}

//输出：
97
98
2
-----------------------
99            //第二次是从下标0的位置开始覆盖，所以99覆盖了97 ，有效个数是1
98
1
-----------------------
99           // 没有可以覆盖的数据了//读到了-1结束
98
-1
```



![定义小数组](http://ogtmd8elu.bkt.clouddn.com/201708071529_372.png "定义小数组")



### 定义小数组的标准格式

- A:案例演示
  - 字节流一次读写一个字节数组复制图片和视频

```java
    FileInputStream fis = new FileInputStream("致青春.mp3");
    FileOutputStream fos = new FileOutputStream("copy.mp3");
    int len;
    byte[] arr = new byte[1024 * 8];					//自定义字节数组
    while((len = fis.read(arr)) != -1) {
        //fos.write(arr);
        fos.write(arr, 0, len);							//写出字节数组写出有效个字节个数
    }
    fis.close();
    fos.close();
```





###  BufferedInputStream和BufferOutputStream拷贝

- A:缓冲思想
  - 字节流一次读写一个数组的速度明显比一次读写一个字节的速度快很多，
  - 这是加入了数组这样的缓冲区效果，java本身在设计的时候，
  - 也考虑到了这样的设计思想(装饰设计模式后面讲解)，所以提供了字节缓冲区流
- B.BufferedInputStream
  - BufferedInputStream内置了一个缓冲区(数组)
  - 从BufferedInputStream中读取一个字节时
  - BufferedInputStream会一次性从文件中读取8192个, 存在缓冲区中, 返回给程序一个
  - 程序再次读取时, 就不用找文件了, 直接从缓冲区中获取
  - 直到缓冲区中所有的都被使用过, 才重新从文件中读取8192个
- C.BufferedOutputStream
  - BufferedOutputStream也内置了一个缓冲区(数组)
  - 程序向流中写出字节时, 不会直接写到文件, 先写到缓冲区中
  - 直到缓冲区写满, BufferedOutputStream才会把缓冲区中的数据一次性写到文件里
- D.拷贝的代码 
  ​		
```java
  //创建文件输入流对象,关联致青春.mp3
  FileInputStream fis = new FileInputStream("致青春.mp3");	
  BufferedInputStream bis = new BufferedInputStream(fis);			//创建缓冲区对fis装饰
  FileOutputStream fos = new FileOutputStream("copy.mp3");		//创建输出流对象,关联copy.mp3
  BufferedOutputStream bos = new BufferedOutputStream(fos);		//创建缓冲区对fos装饰

  int b;
  while((b = bis.read()) != -1) {		
  	bos.write(b);
  }

  bis.close();						//只关装饰后的对象即可
  bos.close();
```

![](http://ogtmd8elu.bkt.clouddn.com/201708071605_530.png "Buffered缓存原理")

```java
package cn.itcast.io;

import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class Demo6_BufferedCopy {

	/**
	 * @param args
	 * @throws IOException 
	 * 如果文件足够大,read()方法执行一次,就会将文件上字节数据一次读取8192个字节存储在,BufferedInputStream的缓冲区中
	 * 从缓冲区中返给一个字节一个字节返给b
	 * 如果write一次,先将缓冲区装满,然后一股脑的写到文件上去
	 * 这么做减少了到硬盘上读和写的操作
	 */
	public static void main(String[] args) throws IOException {
		FileInputStream fis = new FileInputStream("致青春.mp3");			//创建文件输入流对象,关联致青春.mp3
		BufferedInputStream bis = new BufferedInputStream(fis);			//创建缓冲区对fis装饰
		FileOutputStream fos = new FileOutputStream("copy.mp3");		//创建输出流对象,关联copy.mp3
		BufferedOutputStream bos = new BufferedOutputStream(fos);		//创建缓冲区对fos装饰
		
		int b;
		while((b = bis.read()) != -1) {		
			bos.write(b);
		}
		
		bis.close();						//只关装饰后的对象即可
		bos.close();
		
	}

}
```



###  小数组的读写和带Buffered的读取哪个更快

- 定义小数组如果是8192个字节大小和Buffered比较的话
- 定义小数组会略胜一筹,因为读和写操作的是同一个数组
- 而Buffered操作的是两个数组

###  flush和close方法的区别

- flush()方法
  - 用来刷新缓冲区的,刷新后可以再次写出 
- close()方法
  - 用来关闭流释放资源的的,如果是带缓冲区的流对象的close()方法,不但会关闭流,还会再关闭流之前刷新缓冲区,关闭后不能再写出 

```java
package cn.itcast.io;

import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class Demo7_Flush {

	/**
	 * @param args
	 * @throws IOException 
	 * flush和close的区别
	 * flush刷新之后还可以继续写
	 * close在关闭之前会刷新一次,把缓冲区剩余的字节刷到文件上去,但是调用之后不能再写
	 */
	public static void main(String[] args) throws IOException {
		//demo1();
		FileInputStream fis = new FileInputStream("致青春.mp3");
		FileOutputStream fos = new FileOutputStream("copy.mp3");
		
		int b;
		while((b = fis.read()) != -1) {
			fos.write(b);
		}
		
		fis.close();
		fos.close();//释放资源
	}

	public static void demo1() throws FileNotFoundException, IOException {
		BufferedInputStream bis = new BufferedInputStream(new FileInputStream("致青春.mp3"));
		BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("copy.mp3"));
		
		int b;
		while((b = bis.read()) != -1) {
			bos.write(b);
			//bos.flush();		//需要随时刷新缓冲区内容的时候,用flush
		}
		
		bis.close();
		bos.close(); //释放缓冲区和资源
	}

}

```





###  字节流读写中文

- 字节流读取中文的问题
  - 字节流在读中文的时候有可能会读到半个中文,造成乱码 
- 字节流写出中文的问题
  - 字节流直接操作的字节,所以写出中文必须将字符串转换成字节数组 
  - 写出回车换行 write("\r\n".getBytes());

```java
package cn.itcast.io;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class Demo8_Chinese {

	/**
	 * @param args
	 * @throws IOException 
	 * 字节流只读中文是有弊端的,有可能会读到半个中文
	 * 解决方法有2
	 * 1,用字符流读(编码表+字节流)
	 * 2,将文件上的所有字节一次读到内存中,在内存中将所有字节转换成对应的字符串
	 * ByteArrayOutputStream
	 * 
	 * 字节流写中文
	 * 在只写中文的时候必须转换成字节数组写出去
	 * 
	 * 字节流可以拷贝任意类型的数据,因为所有的数据都是一字节的形式存在的
	 */
	public static void main(String[] args) throws IOException {
		//demo1();
		demo2();
		//demo3();
	}

	public static void demo3() throws FileNotFoundException, IOException {
		FileInputStream fis = new FileInputStream("aaa.txt");
		FileOutputStream fos = new FileOutputStream("bbb.txt");
		
		int b;
		while((b = fis.read()) != -1) {
			fos.write(b);
		}
		
		fis.close();
		fos.close();
	}

	public static void demo2() throws FileNotFoundException, IOException {
		FileOutputStream fos = new FileOutputStream("bbb.txt");
		fos.write("你好".getBytes());								//字节流写出中文的时候要转换成字节数组
		fos.write("\r\n".getBytes());							//写出回车换行
		fos.write("你好".getBytes());			
		fos.close();
	}

	public static void demo1() throws FileNotFoundException, IOException {
		FileInputStream fis = new FileInputStream("bbb.txt");	//创建文件输入流,关联bbb.txt
		byte[] arr = new byte[3];								//创建字节数组
		int len;
		while((len = fis.read(arr)) != -1) {					//先将字节读到数组中
			System.out.println(new String(arr,0,len));			//转换成字符串打印在控制台,转换的时候有可能乱码
		}
		fis.close();
	}

}

```





###  流的标准处理异常代码1.6版本及其以前


```java
//try finally嵌套

FileInputStream fis = null;
FileOutputStream fos = null;
try {
	fis = new FileInputStream("aaa.txt");
	fos = new FileOutputStream("bbb.txt");
	int b;
	while((b = fis.read()) != -1) {
		fos.write(b);
	}
} finally {
	try {
		if(fis != null)
			fis.close();
	}finally {
		if(fos != null)
			fos.close();
	}
}
```
### 流的标准处理异常代码1.7版本

```java
package cn.itcast.io;

        import java.io.FileInputStream;
        import java.io.FileNotFoundException;
        import java.io.FileOutputStream;
        import java.io.IOException;

public class Demo9_IOException {

    /**
     * @param args
     * @throws IOException 在try()中创建的流对象必须实现了AutoCloseable这个接口,如果实现了,在try后面的{}(读写代码)执行后就会自动调用
     *                     流对象的close方法将流关掉
     */
    public static void main(String[] args) throws IOException {
        //demo1();
      
      //java1.7以后，封装了流的close方法，try过以后，就会自动关闭
        try (
                FileInputStream fis = new FileInputStream("aaa.txt");
                FileOutputStream fos = new FileOutputStream("bbb.txt");
                MyClose mc = new MyClose();
        ) {
            int b;
            while ((b = fis.read()) != -1) {
                fos.write(b);
            }
        }

    }

    public static void demo1() throws FileNotFoundException, IOException {
        //标准的异常处理代码 1.6以前
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            fis = new FileInputStream("aaa.txt");
            fos = new FileOutputStream("bbb.txt");
            int b;
            while ((b = fis.read()) != -1) {
                fos.write(b);
            }
        } finally {
            try {
                if (fis != null)
                    fis.close();
            } finally {
                if (fos != null)
                    fos.close();
            }
        }
    }

}

class MyClose implements AutoCloseable {
    public void close() {
        System.out.println("我关了");
    }
}
```
- 原理
  - 在try()中创建的流对象必须实现了AutoCloseable这个接口,如果实现了,在try后面的{}(读写代码)执行后就会自动调用,流对象的close方法将流关掉 

### 图片加密

```java
package cn.itcast.test;

import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class Test2 {

	/**
	 * @param args
	 * 给图片加密
	 * @throws IOException 
	 */
	public static void main(String[] args) throws IOException {
		//demo1();
		BufferedInputStream bis = new BufferedInputStream(new FileInputStream("a.jpg"));
		BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("b.jpg"));
		
		int b;
		while((b = bis.read()) != -1) {
			bos.write(b ^ 123);
		}
		
		bis.close();
		bos.close();
	}

	public static void demo1() throws FileNotFoundException, IOException {
		FileInputStream fis = new FileInputStream("b.jpg");
		FileOutputStream fos = new FileOutputStream("c.jpg");
		
		int b;
		while((b = fis.read()) != -1) {
			fos.write(b ^ 123);
		}
		
		fis.close();
		fos.close();
	}

}

```
### 拷贝文件

- 在控制台录入文件的路径,将文件拷贝到当前项目下

```java
package site.yanhui.test;

import java.io.*;
import java.util.Scanner;


public class Test01 {

    public static void main(String[] args) throws IOException {

      //在控制台录入文件的路径,将文件拷贝到当前项目下
        File dir = getDir();
        CopyFile(dir);
    }



    /*
	 * 键盘获取一个文件夹路径
	 */
    public static File getDir() {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入一个文件路径:");
        while(true) {
            String line = sc.nextLine();
            File dir = new File(line);
            if(!dir.exists()) {
                System.out.println("您录入的路径不存在,请重新输入一个文件路径:");
            }else if(dir.isDirectory()) {
                System.out.println("您录入的是文件夹路径,请重新输入一个文件路径:");
            }else {
                return dir;
            }
        }
    }


    public static void CopyFile(File dir) throws IOException {

            File file=  dir;
        FileInputStream fileInputStream =new FileInputStream(dir);
        FileOutputStream fos= new FileOutputStream(file.getName()); //不管你是什么文件类型，都可以拷贝

        int len;
        byte [] arr=new byte[1024*8];

        while ((len=fileInputStream.read(arr))!=-1){
            fos.write(arr,0,len);
        }

        fileInputStream.close();
        fos.close();

    }
}
```

###  录入数据拷贝到文件

* 将键盘录入的数据拷贝到当前项目下的text.txt文件中,键盘录入数据当遇到quit时就退出

```java
package site.yanhui.test;

import java.io.FileOutputStream;
import java.io.IOException;
import java.util.Scanner;

public class test02 {
    public static void main(String[] args) throws IOException {
        FileOutputStream fos= new FileOutputStream("text11111.txt");
        Scanner mScanner = new Scanner(System.in);
        System.out.println("请输出一个字符串：");
        while(true){
        String line = mScanner.nextLine();
        if (line.equals("quit")){
            break;
        }
            byte[] bytes = line.getBytes();//得到二进制码写入
            fos.write(bytes);
            fos.write("\r\n".getBytes());//换行
        }
        fos.close();
    }
}

```





### 拷贝时应该注意的问题

```java
package cn.itcast.test;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class Test5 {

   /**
    * @param args
    * @throws IOException 
    * 
    */
   public static void main(String[] args) throws IOException {
      FileInputStream fis = new FileInputStream("aaa.txt");
      FileOutputStream fos = new FileOutputStream("bbb.txt");
      byte[] arr = new byte[8192];
      int len;
      while((len = fis.read(arr)) != -1) {//如果read方法忘记写字节数组了,就会返回的字节的码表值,写出就会比原来的文件大的多
         fos.write(arr, 0, len);
      }
      
      fis.close();
      fos.close();
   }

}
```









20.20_day20总结

- 把今天的知识点总结一遍。
