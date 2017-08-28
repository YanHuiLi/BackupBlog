---
title: Java学习笔记整理（21）
date: 2017-08-08 12:38:23
tags: [J2SE]
categories: J2SE
top: 21
---

---

1. 操作字符流
2. FileReader和FileWriter
3. 字符流拷贝
4. 纯文本情况下,,字符流拷贝好,还是字节好?
5. 只读或者只写的时候使用字符流
6. 字符流能不能拷贝非纯文本的数据?
7. LineNumberReader的使用
8. 装饰者设计模式
9. 使用指定的码表读写字符

<!--more-->


### 字符流FileReader
* 1.字符流是什么
  * 字符流是可以直接读写字符的IO流
  * 字符流读取字符, 就要先读取到字节数据, 然后转为字符. 如果要写出字符, 需要把字符转为字节再写出.(靠编码表)    
* 2.FileReader, FileWriter
  * FileReader类的read()方法可以按照字符大小读取

```java
package cn.itcast.chario;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

public class Demo1_FileReader {

	/**
	 * @param args
	 * @throws IOException 
	 */
	public static void main(String[] args) throws IOException {
		//demo1();
		FileReader fr = new FileReader("a.txt");
		int ch;
		while((ch = fr.read()) != -1) {			//read()方法是通过编码表按照字符的大小读取
			System.out.print((char)ch);
		}
		
		fr.close();
	}

	public static void demo1() throws IOException {
		FileReader fr = new FileReader("a.txt");
		int x = fr.read();						//将char类型提升为int类型,因为文件结束标记都是-1,char取值范围0到65535,不能存储-1
		System.out.println((char)x);
		int y = fr.read();
		System.out.println(y);
		int z = fr.read();
		System.out.println(z);
		int a = fr.read();
		System.out.println(a);
		fr.close();
	}

}
```

  * FileWriter类的write()方法可以自动把字符转为字节写出

### 字符流FileWriter
* FileWriter类的write()方法可以自动把字符转为字节写出
```java
package cn.itcast.chario;

import java.io.FileWriter;
import java.io.IOException;

public class Demo2_FileWriter {

	/**
	 * @param args
	 * @throws IOException 
	 */
	public static void main(String[] args) throws IOException {
		FileWriter fw = new FileWriter("b.txt");			//创建字符输出流对象,关联b.txt
		fw.write(97);										//写出一个字符
		fw.write("你要减肥,造吗?");								//写出一个字符串
		fw.close();	//关流内植了一个2k的缓冲区，如果不关流就没调用flush方法，没数据写入。
	}

}

```
### 字符流的拷贝
```java
package cn.itcast.chario;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Demo3_Copy {

	/**
	 * @param args
	 * @throws IOException 
	 * 拷贝纯文本的文件用字符流好还是用字节流好?
	 * 拷贝纯文本字节流更好
	 * 
	 * 字符流在什么时候用呢?
	 * 操作纯文本
	 * 只读,一次读取的是一个字符,不会出现乱码
	 * 只写,可以直接写出的是字符串,不用转换
	 * 
	 * 当字符流拷贝非纯文本文件和拷贝纯文本的操作是一样的,需要先将字节转换为字符,转换字符如果没有转换成功就会变成?,写出去的时候
	 * 就会将?写出
	 * 
	 * 字符流只能操作纯文本的文件
	 * 字节流可以操作的是任意文件
	 */
	public static void main(String[] args) throws IOException {
		//demo1();
		FileReader fr = new FileReader("4.mp3");
		FileWriter fw = new FileWriter("copy.mp3");
		
		int ch;
		while((ch = fr.read()) != -1) {
			fw.write(ch);
		}
		
		fr.close();
		fw.close();
	}

	public static void demo1() throws FileNotFoundException, IOException {
		FileReader fr = new FileReader("a.txt");
		FileWriter fw = new FileWriter("b.txt");
		
		int ch;
		while((ch = fr.read()) != -1) {
			fw.write(ch);
		}
		
		fr.close();
		fw.close();
	}

}

```
### 什么情况下使用字符流
* 字符流也可以拷贝文本文件, 但不推荐使用. 因为读取时会把字节转为字符, 写出时还要把字符转回字节.
* 程序需要读取一段文本, 或者需要写出一段文本的时候可以使用字符流

![](http://ogtmd8elu.bkt.clouddn.com/201708090705_103.png "字节流和字符流的图解")

###字符流是否可以拷贝非纯文本的文件
* 不可以拷贝非纯文本的文件
* 因为在读的时候会将字节转换为字符,在转换过程中,可能找不到对应的字符,就会用?代替,写出的时候会将字符转换成字节写出去
* 如果是?,直接写出,这样写出之后的文件就乱了,看不了了  

###自定义字符数组的拷贝
```java
   FileReader fr = new FileReader("aaa.txt");			//创建字符输入流,关联aaa.txt
   FileWriter fw = new FileWriter("bbb.txt");			//创建字符输出流,关联bbb.txt

   	int len;
   	char[] arr = new char[1024*8];						//创建字符数组
   	while((len = fr.read(arr)) != -1) {					//将数据读到字符数组中
   		fw.write(arr, 0, len);							//从字符数组将数据写到文件上
                                                        //效率没有直接拷贝字节高,因为多了转化为字符串的过程
   	}
   	
   	fr.close();											//关流释放资源
   	fw.close();	
```

### 带缓冲的字符流 
* BufferedReader的read()方法读取字符时会一次读取若干字符到缓冲区, 然后逐个返回给程序, 降低读取文件的次数, 提高效率
* BufferedWriter的write()方法写出字符时会先写到缓冲区, 缓冲区写满时才会写到文件, 降低写文件的次数, 提高效率
```java
   BufferedReader br = new BufferedReader(new FileReader("aaa.txt"));	//创建字符输入流对象,关联aaa.txt
   	BufferedWriter bw = new BufferedWriter(new FileWriter("bbb.txt"));	//创建字符输出流对象,关联bbb.txt

   	int ch;				
   	while((ch = br.read()) != -1) {		//read一次,会先将缓冲区读满,从缓冲去中一个一个的返给临时变量ch
   		bw.write(ch);					//write一次,是将数据装到字符数组,装满后再一起写出去
   	}
   	
   	br.close();							//关流
   	bw.close();  
```

### readLine()和newLine()方法	
* BufferedReader的readLine()方法可以读取一行字符(不包含换行符号)
* BufferedWriter的newLine()可以输出一个跨平台的换行符号"\r\n"
```java
BufferedReader br = new BufferedReader(new FileReader("aaa.txt"));
   	BufferedWriter bw = new BufferedWriter(new FileWriter("bbb.txt"));
   	String line;
   	while((line = br.readLine()) != null) {
   		bw.write(line);
   		//bw.write("\r\n");					//只支持windows系统
   		bw.newLine();						//跨平台的
   	}
   	
   	br.close();
   	bw.close(); 
```

###将文本反转
* 将一个文本文档上的文本反转,第一行和倒数第一行交换,第二行和倒数第二行交换

```java
package cn.itcast.test;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;

public class Test1 {
	/*
	 * 将一个文本文档上的文本反转,第一行和倒数第一行交换,第二行和倒数第二行交换
	 * 流对象,尽量的晚开早关(为了节省内存)
	 */
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new FileReader("a.txt"));	//创建输入流对象,关联a.txt
		ArrayList<String> list = new ArrayList<>();							//创建集合对象
		String line;
		while((line = br.readLine()) != null) {								//读取a.txt上的数据
			list.add(line);													//将读到的字符串存储在集合中
		}
		br.close();															//关闭输入流
		
		BufferedWriter bw = new BufferedWriter(new FileWriter("a.txt"));	//创建写出流对象
		for(int i = list.size() - 1; i >= 0; i--) {							//遍历循环集合
			bw.write(list.get(i));											//将集合中的元素写到文本上
			bw.newLine();													//写出换行符
		}
		bw.close();															//关闭输出流
	}
}

```







###LineNumberReader 
* LineNumberReader是BufferedReader的子类, 具有相同的功能, 并且可以统计行号
  * 调用getLineNumber()方法可以获取当前行号
  * 调用setLineNumber()方法可以设置当前行号
```java
package cn.itcast.chario;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.io.LineNumberReader;

public class Demo6_LineNumberReader {

	/**
	 * @param args
	 * @throws IOException 
	 * 通过getLineNumber()和setLineNumber()方法我们知道肯定有lineNumber,当readLine()一次
	 * lineNumber就自身+1一次,所以每次通过getLineNumber()获取行号的时候都是变化的
	 */
	public static void main(String[] args) throws IOException {
		LineNumberReader lnr = new LineNumberReader(new FileReader("b.txt"));
		String line;
		lnr.setLineNumber(100);
		while((line = lnr.readLine()) != null) {
			System.out.println(lnr.getLineNumber() + ":" + line);
		}
		
		lnr.close();
	}

}
```
### 装饰设计模式

```java
package site.yanhui;

public class Demo07_Wrap {

    /**
     * 装饰设计模式
     * 装饰设计模式的好处是耦合性降低了,被装饰类,可以被装饰,也可以不被装饰,也可以选择其他的装饰
     * 目的主要是为了解耦，降低耦合性，添加更多功能。
     * @param args
     */
    public static void main(String[] args) {

//        ItStudent student= new ItStudent(new Student());
//        student.Coder();


        Student student1= new Student();
        student1.Coder();
    }


}

interface  Coder{

    void Coder();

}


class Student implements  Coder{


    @Override
    public void Coder() {
        System.out.println("C");
        System.out.println("java");
        System.out.println("数据结构");

    }
}

class ItStudent implements Coder{

    private Student student;

    public ItStudent(Student student) {
        this.student = student;
    }

    @Override
    public void Coder() {

        student.Coder();

        System.out.println("Android");
        System.out.println("设计模式");
        System.out.println("J2ee");
    }
}




```

###  使用指定的码表读写字符 
* FileReader是使用默认码表读取文件, 如果需要使用指定码表读取, 那么可以使用InputStreamReader(字节流,编码表)
* FileWriter是使用默认码表写出文件, 如果需要使用指定码表写出, 那么可以使用OutputStreamWriter(字节流,编码表)
```java
package cn.itcast.chario;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.UnsupportedEncodingException;

public class Demo8_TransIO {

	/**
	 * @param args
	 * 转换流
	 * @throws IOException 
	 */
	public static void main(String[] args) throws IOException {
		//demo1();
		//demo2();
      //16K的缓冲区.
		BufferedReader br = 												//InputStreamReader是字节通向字符的桥梁
				new BufferedReader(new InputStreamReader(new FileInputStream("UTF-8.txt"), "UTF-8"));
		BufferedWriter bw = 												//OutputStreamWriter是字符通向字节的桥梁
				new BufferedWriter(new OutputStreamWriter(new FileOutputStream("GBK.txt"), "GBK"));
		
		int ch;
		while((ch = br.read()) != -1) {
			bw.write(ch);
		}
		
		br.close();
		bw.close();
	}

	public static void demo2() throws UnsupportedEncodingException,
			FileNotFoundException, IOException {
		InputStreamReader isr = 
				new InputStreamReader(new FileInputStream("UTF-8.txt"), "UtF-8");		//通过指定编码表读
		OutputStreamWriter osw = 
				new OutputStreamWriter(new FileOutputStream("GBK.txt"), "GBK");			//通过指定编码表写
		
		int ch;
		while((ch = isr.read()) != -1) {
			osw.write(ch);
		}
		
		isr.close();
		osw.close();
	}

	public static void demo1() throws FileNotFoundException, IOException {
		FileReader fr = new FileReader("UTF-8.txt");	//字节流+GBK
		FileWriter fw = new FileWriter("GBK.txt");
		
		int ch;
		while((ch = fr.read()) != -1) {
			fw.write(ch);
		}
		
		fr.close();
		fw.close();
	}

}
```
### 转换流图解
* 画图分析转换流

![](http://ogtmd8elu.bkt.clouddn.com/201708091138_27.png)

UTF-8 一个中文是占三个字节,存有(你好你好)就有12个字节,但是默认的GBK编码收到12个字节以后,拆分的是两个字节一个中文,拆成了6个中文,造成了乱码的情况.

字节->字符->字节->字符.

InputStream是一个装饰类,用来制定编码表还原成字符.再由outputstream制定编码表还原成字节,最后写到文件中



![](http://ogtmd8elu.bkt.clouddn.com/201708091149_259.png)

### 获取文本上字符出现的次数
* 获取一个文本上每个字符出现的次数,将结果写在time.txt上



```java
package site.yanhui;

import java.io.*;
import java.util.TreeMap;

/**
 * 需求：获取一个文本上每个字符出现的次数,将结果写在time.txt上。
 * <p>
 * 1. 关联要统计的文件
 * 2. 用treeMap进行排序
 * 3. 将treeMap里面的数据写入到time.txt上。
 */
public class Demo08_Copy {

    public static void main(String[] args) throws IOException {


        //关联在项目根目录下的a.txt文件
        BufferedReader br = new BufferedReader(new FileReader("a.txt"));

        //初始化一个treeMap进行排序
        TreeMap<Character, Integer> treeMap = new TreeMap<>();

        //将数据存入treeMap里面
        int ch;
        while ((ch = br.read()) != -1) {
            char ch1 = (char) ch;
//            if (!treeMap.containsKey(ch1)) {
//                treeMap.put(ch1, 1);
//            } else {
//                treeMap.put(ch1, treeMap.get(ch1) + 1);
//            }

            treeMap.put(ch1,!treeMap.containsKey(ch1)? 1:treeMap.get(ch1) + 1);
        }
        br.close();

        BufferedWriter bw = new BufferedWriter(new FileWriter("time.txt"));
        for (Character character : treeMap.keySet()) {

            switch (character) {
                case '\t':
                    bw.write("\\t" + " 出现了 " + treeMap.get(character));
                    break;
                case '\n':
                    bw.write("\\n" + " 出现了 " + treeMap.get(character));
                    break;
                case '\r':
                    bw.write("\\r" + " 出现了 " + treeMap.get(character));
                    break;
                default:
                    bw.write(character + " 出现了 " + treeMap.get(character) + "次");
                    break;

            }
            bw.newLine();
        }

        bw.close();


    }
}
```



### 试用版软件
* 当我们下载一个试用版软件,没有购买正版的时候,每执行一次就会提醒我们还有多少次使用机会用学过的IO流知识,模拟试用版软件,试用10次机会,执行一次就提示一次您还有几次机会,如果次数到了提示请购买正版
```java
package cn.itcast.test;

import java.io.BufferedReader;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Test3 {

	/**
	 * @param args
	 * 当我们下载一个试用版软件,没有购买正版的时候,每执行一次就会提醒我们还有多少次使用机会用学过的IO流知识,
	 * 模拟试用版软件,试用10次机会,执行一次就提示一次您还有几次机会,如果次数到了提示请购买正版
	 * @throws IOException 
	 */
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new FileReader("config.txt"));	//创建输入流对象,关联config.txt
		String line = br.readLine();											//从文件上读取配置文件的数字
		br.close();																//关流
		int times = Integer.parseInt(line);										//将数字字符串转换成数字
		if(times > 0) {															//如果次数大于0
			FileWriter fw = new FileWriter("config.txt");						//创建输出流对象,关联config.txt
			fw.write(--times + "");												//将次数减一后写会原配置文件
			System.out.println("您还有" + times + "次机会");						//提示
			fw.close();															//关流
		}else {
			System.out.println("您的试用次数已到,请购买正版");							//提示
		}
		
		//System.out.println(Integer.MAX_VALUE);								//可以获取int类型的最大值
	}

}

```



###  总结
* 1.会用BufferedReader读取GBK码表和UTF-8码表的字符
* 2.会用BufferedWriter写出字符到GBK码表和UTF-8码表的文件中
* 3.会使用BufferedReader从键盘读取一行
