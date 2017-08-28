---
title: Java学习笔记整理（22）
date: 2017-08-10 08:30:11
tags: [J2SE,IO流]
categories: J2SE
top: 22
---

---

1. 序列流

2. **内存输入输出流**

3. 对象输出输入流

4. **打印流**

5. 标准输入输出流

6. **两种键盘录入**

7. 随机访问流

8. Properties


<!--more-->


### IO流(序列流)
* 1.什么是序列流
  * 序列流可以把多个字节输入流整合成一个, 从序列流中读取数据时, 将从被整合的第一个流开始读, 读完一个之后继续读第二个, 以此类推.
* 2.使用方式
  * 整合两个: SequenceInputStream(InputStream, InputStream)




```java
        FileInputStream fis1 = new FileInputStream("a.txt");			//创建输入流对象,关联a.txt
     	FileInputStream fis2 = new FileInputStream("b.txt");			//创建输入流对象,关联b.txt
     	SequenceInputStream sis = new SequenceInputStream(fis1, fis2);	//将两个流整合成一个流
     	FileOutputStream fos = new FileOutputStream("c.txt");			//创建输出流对象,关联c.txt
     	
     	int b;
     	while((b = sis.read()) != -1) {									//用整合后的读
     		fos.write(b);												//写到指定文件上
     	}
     	
     	sis.close();
     	fos.close(); 
```
### IO流（序列流整合多个）
* 整合多个: SequenceInputStream(Enumeration)
```java
    FileInputStream fis1 = new FileInputStream("a.txt");	//创建输入流对象,关联a.txt
   	FileInputStream fis2 = new FileInputStream("b.txt");	//创建输入流对象,关联b.txt
   	FileInputStream fis3 = new FileInputStream("c.txt");	//创建输入流对象,关联c.txt
   	Vector<InputStream> v = new Vector<>();					//创建vector集合对象
   	v.add(fis1);											//将流对象添加
   	v.add(fis2);
   	v.add(fis3);
   	Enumeration<InputStream> en = v.elements();				//获取枚举引用
   	SequenceInputStream sis = new SequenceInputStream(en);	//传递给SequenceInputStream构造
   	FileOutputStream fos = new FileOutputStream("d.txt");
   	int b;
   	while((b = sis.read()) != -1) {
   		fos.write(b);
   	}
   	
   	sis.close();
   	fos.close();
   	
```

### IO流（内存输出流）
* 1.什么是内存输出流
  * 该输出流可以向内存中写数据, 把内存当作一个缓冲区, 写出之后可以一次性获取出所有数据
* 2.使用方式
  * 创建对象: new ByteArrayOutputStream()
  * 写出数据: write(int), write(byte[])
  * 获取数据: toByteArray()



```java
     FileInputStream fis = new FileInputStream("a.txt"); //a: 你好，你好
     ByteArrayOutputStream baos = new ByteArrayOutputStream();
     int b;
     while((b = fis.read()) != -1) {
     	baos.write(b);
     }

     //byte[] newArr = baos.toByteArray();				//将内存缓冲区中所有的字节存储在newArr中
     //System.out.println(new String(newArr));
     System.out.println(baos);
     fis.close();
```
### IO流（内存输出流之黑马面试题）
* 定义一个文件输入流,调用read(byte[] b)方法,将a.txt文件中的内容打印出来(byte数组大小限制为5)

```java
package site.yanhui.otherio;

import java.io.*;

public class Demo2_ByteArrayOutputStream {


    /**
     * @param args
     * 字节流在读中文会出现半个中文,乱码
     * 解决
     * 1,字符流
     * 2,ByteArrayOutputStream
     * @throws IOException
     */
    
    public static void main(String[] args) throws IOException {

//        demo01();

        FileInputStream fis= new FileInputStream("e.txt");
 
        ByteArrayOutputStream bos =new ByteArrayOutputStream(); //底层实现了一个可以增长的字节数组，所以不会造成乱码的情况

        int len;
        byte[] arr =new byte[5];//每次读五个字节

        while ((len=fis.read(arr))!=-1){

            bos.write(arr,0,len); //每次五个字节，一次又一次的写入bos底层的字节数组里面
        }
        System.out.println(bos);//统一输出，避免了乱码问题

        fis.close();
        bos.close();



    }

    private static void demo01() throws IOException {
        FileInputStream fis= new FileInputStream("e.txt");

        byte[] arr= new byte[3];//一个中文占了两个字节，一个符号占了1个字节

        int len;

        while ((len=fis.read(arr))!=-1) { //用arr 3个字节数组来装输入流，容易造成乱码
            System.out.println(new String(arr,0,len));// 输出这个字节流

        }

        fis.close();
    }
}
```

### IO流（对象操作流ObjecOutputStream）
* 1.什么是对象操作流
  * 该流可以将一个对象写出, 或者读取一个对象到程序中. 也就是执行了序列化和反序列化的操作.
* 2.使用方式
  * 写出: new ObjectOutputStream(OutputStream), writeObject()



```java
          public class Demo3_ObjectOutputStream {

             /**
     		 * @param args
     		 * @throws IOException 
     		 * 将对象写出,序列化
     		 */
     		public static void main(String[] args) throws IOException {
     			Person p1 = new Person("张三", 23);
     			Person p2 = new Person("李四", 24);
     	//		FileOutputStream fos = new FileOutputStream("e.txt");
     	//		fos.write(p1);
     	//		FileWriter fw = new FileWriter("e.txt");
     	//		fw.write(p1);
     			//无论是字节输出流,还是字符输出流都不能直接写出对象
     			ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("e.txt"));//创建对象输出流
     			oos.writeObject(p1);
     			oos.writeObject(p2);
     			oos.close();
     		}
     	
     	}
```
### IO流(对象操作流ObjectInputStream)
* 读取: new ObjectInputStream(InputStream), readObject()






```java
     public class Demo3_ObjectInputStream {

     		/**
     		 * @param args
     		 * @throws IOException 
     		 * @throws ClassNotFoundException 
     		 * @throws FileNotFoundException 
     		 * 读取对象,反序列化
     		 */
     		public static void main(String[] args) throws IOException, ClassNotFoundException {
     			ObjectInputStream ois = new ObjectInputStream(new FileInputStream("e.txt"));
     			Person p1 = (Person) ois.readObject();
     			Person p2 = (Person) ois.readObject();
     			System.out.println(p1);
     			System.out.println(p2);
     			ois.close();
     		}
     	
     	}
```

### IO流(对象操作流优化)





```java
package cn.itcast.otherio;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.FileWriter;
import java.io.IOException;
import java.io.ObjectOutputStream;
import java.util.ArrayList;

import cn.itcast.bean.Person;

public class Demo03_ObjectOutputStream {

	/**
	 * @param args
	 * @throws IOException 
	 * 序列化,将对象写到文件上(存档)
	 * ArrayList集合实现了序列化接口，所以能够序列化
	 */
	public static void main(String[] args) throws IOException {
		//demo1();
		//demo2();
		Person p1 = new Person("张三", 23);
		Person p2 = new Person("李四", 24);
		Person p3 = new Person("马哥", 18);
		Person p4 = new Person("辉哥", 20);
		
		ArrayList<Person> list = new ArrayList<>();
		list.add(p1);
		list.add(p2);
		list.add(p3);
		list.add(p4);
		
		ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("f.txt"));
		oos.writeObject(list);									//写出集合对象
		
		oos.close();
	}

	public static void demo2() throws IOException, FileNotFoundException {
		Person p1 = new Person("张三", 23);
		Person p2 = new Person("李四", 24);
		ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("e.txt"));
		oos.writeObject(p1);
		oos.writeObject(p2);
		oos.close();
	}

	public static void demo1() throws FileNotFoundException, IOException {
		Person p1 = new Person("张三", 23);
		FileOutputStream fos = new FileOutputStream("e.txt");
		//fos.write(p1);										//字节输出流不可以写出对象
		FileWriter fw = new FileWriter("e.txt");
		//fw.write(p1);											//字符输出流不可以写出对象
	}

}

```


```java
package cn.itcast.otherio;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.util.ArrayList;

import cn.itcast.bean.Person;

public class Demo04_ObjectInputStream {

   /**
    * @param args
    * @throws IOException 
    * @throws ClassNotFoundException 
    * @throws FileNotFoundException 
    * 反序列化,将文件上的对象读出来(读档)
    * 如果写出两个对象,读三个对象会抛出EOFException
    * EOF end of File 文件到末尾异常
    */
   @SuppressWarnings("unchecked")
   public static void main(String[] args) throws IOException, ClassNotFoundException {
      //demo1();
      ObjectInputStream ois = new ObjectInputStream(new FileInputStream("f.txt"));
      ArrayList<Person> list = (ArrayList<Person>)ois.readObject();  //泛型在运行期会被擦除,索引运行期相当于没有泛型
                                                      //想去掉黄色可以加注解@SuppressWarnings("unchecked")
      for (Person person : list) {
         System.out.println(person);
      }
      
      ois.close();
   }

   public static void demo1() throws IOException, FileNotFoundException,
         ClassNotFoundException {
      ObjectInputStream ois = new ObjectInputStream(new FileInputStream("e.txt"));
      Person p1 = (Person)ois.readObject();           //读一个对象
      Person p2 = (Person)ois.readObject();
      //Person p3 = (Person)ois.readObject();
      
      System.out.println(p1);
      System.out.println(p2);
      //System.out.println(p3);
      ois.close();
   }

}
```





### IO流(加上id号)

* 注意
  * 要写出的对象必须实现Serializable接口才能被序列化
  * 不用必须加id号（序列化就是一个读档存档的过程）
  * 加上ID号的话更加直观，否则就是系统随机给的随机值


```java
package site.yanhui.bean;

import java.io.Serializable;

public class Person1  implements Serializable {

    //存档的问题
//    private static final long serialVersionUID = 4L;		//对版本升级
    private String name;
    private  int age;
    private String gender;


    public Person1(String name, int age) {
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
        return "Person1{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```





### IO流(打印流的概述和特点)
* 1.什么是打印流
  * 该流可以很方便的将对象的toString()结果输出, 并且自动加上换行, 而且可以使用自动刷出的模式
  * System.out就是一个PrintStream, 其默认向控制台输出信息




```java
package cn.itcast.otherio;

import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.PrintStream;

import cn.itcast.bean.Person;

public class Demo05_PrintStream {

	/**
	 * @param args
	 * @throws IOException 
	 * PrintStream是打印流所以他只操作的是数据目的
	 */
	public static void main(String[] args) throws IOException {
		//demo1();
		PrintStream ps = new PrintStream("e.txt");
		ps.write(97);
		ps.println(97);
		
		ps.close();
	}

	public static void demo1() {
		Person p = new Person("张三", 23);
		PrintStream ps = System.out;
		ps.println(p);
		ps.println(97);					//底层通过Integer.toString(97)将97转换成字符串
		ps.write(97);					//不会将其转换成对应的字符串,写入a
		
		ps.close();
	}

}
   	
```









### IO流(标准输入输出流概述和输出语句)

* 1.什么是标准输入输出流
  * System.in是InputStream, 标准输入流, 默认可以从键盘输入读取字节数据
  * System.out是PrintStream, 标准输出流, 默认可以向Console中输出字符和字节数据
* 2.修改标准输入输出流
  * 修改输入流: System.setIn(InputStream)
  * 修改输出流: System.setOut(PrintStream)




```java
package cn.itcast.otherio;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.PrintStream;
import java.util.Scanner;

public class Demo07_SetInOut {

	/**
	 * @param args
	 * @throws IOException 
	 */
	public static void main(String[] args) throws IOException {
		//Scanner sc = new Scanner(System.in);
		//demo1();
		//demo2();
		System.setIn(new FileInputStream("a.txt"));				//改变标准输入流
		System.setOut(new PrintStream("h.txt"));				//改变标准输出流
		
		InputStream is = System.in;								//标准键盘输入流已经被改变,到a.txt读取文件
		PrintStream ps = System.out;							//标准键盘输出流已经被改变,写到h.txt上
		int b;
		while((b = is.read()) != -1) {
			ps.write(b);
		}
		
		is.close();
		ps.close();
	}

	public static void demo2() {
		System.out.println(97); 								//标准的输出流,默认指向控制
		System.out.close();										//不用关闭
		System.out.println(97);
	}

	public static void demo1() throws IOException {
		InputStream is = System.in;							//标准的输入流,默认指向键盘
		int x = is.read();
		
		int y = is.read();
		System.out.println(x);
		System.out.println(y);
	}

}
```
### IO流(修改标准输入输出流拷贝图片)

```java
package cn.itcast.test;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;
import java.io.PrintStream;

public class Test2 {

	/**
	 * @param args
	 * 修改标准输入输出流拷贝图片
	 * @throws IOException 
	 */
	public static void main(String[] args) throws IOException {
		System.setIn(new FileInputStream("IO图片.png"));		//改变标准输入流
		System.setOut(new PrintStream("copy.png")); 		//改变标准输出流
		
		InputStream is = System.in;							//获取标准输入流
		PrintStream ps = System.out;						//获取标准输出流
		
		int len;
		byte[] arr = new byte[1024 * 8];
		
		while((len = is.read(arr)) != -1) {
			ps.write(arr, 0, len);
		}
		
		is.close();
		ps.close();
	}

}
```
### IO流(两种方式实现键盘录入)
* A:BufferedReader的readLine方法。
  * BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
* B:Scanner




```java
package cn.itcast.otherio;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Scanner;

public class Demo08_System_in {

   /**
    * @param args
    * 键盘录入都是用Scanner
    * @throws IOException 
    */
   public static void main(String[] args) throws IOException {
      Scanner sc = new Scanner(System.in);                  //对标准输入流封装
      System.out.println("请输入第一个字符串");
      String line = sc.nextLine();                        //获取键盘录入的字符串
      System.out.println(line);                          //打印字符串
      
      
      BufferedReader br = new BufferedReader(new InputStreamReader(System.in));//通过转换流将标准的键盘输入流转换为BufferReader
      System.out.println("请输入第二个字符串");
      String line2 = br.readLine();                       //键盘录入一行输入
      System.out.println(line2);
      
      
   }

}
```





### IO流(随机访问流概述和写出数据)
* A:随机访问流概述
  * RandomAccessFile概述
  * RandomAccessFile类不属于流，是Object类的子类。但它融合了InputStream和OutputStream的功能。
  * 支持对随机访问文件的读取和写入。
* B:read(),write(),seek()







```java
package cn.itcast.otherio;

import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.RandomAccessFile;

public class Demo09_RandomAccessFile {

   /**
    * @param args
    * @throws IOException 
    */
   public static void main(String[] args) throws IOException {
      RandomAccessFile raf = new RandomAccessFile("h.txt", "rw");
      //int x = raf.read(); //每次只读一个字节。
      //System.out.println(x);
      raf.seek(10);  //设置指针的位置为10
      raf.write(102);  //在第11个的时候放入数据
      
      raf.close();
   }

}
```







### IO流(数据输入输出流)
* 1.什么是数据输入输出流
  * DataInputStream, DataOutputStream可以按照基本数据类型大小读写数据
  * 例如按Long大小写出一个数字, 写出时该数据占8字节. 读取的时候也可以按照Long类型读取, 一次读取8个字节.
* 2.使用方式




```java
package cn.itcast.otherio;

import java.io.DataOutputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class Demo10_DataOutputStream {

	/**
	 * @param args
	 * @throws IOException 
	 * 00000000 00000000 00000011 11100101 997
	 * 00000000 00000000 00000000 11100101
	 */
	public static void main(String[] args) throws IOException {
		//demo1();
		DataOutputStream dos = new DataOutputStream(new FileOutputStream("h.txt"));
		dos.writeInt(997);
		dos.writeInt(998);
		dos.writeInt(999);
	
		dos.close();
	}

	public static void demo1() throws FileNotFoundException, IOException {
		FileOutputStream fos = new FileOutputStream("h.txt");//写入数据
		fos.write(997);
		fos.write(998);
		fos.write(999);
		
		fos.close();
	}

}

```



```java
package cn.itcast.otherio;

import java.io.DataInputStream;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class Demo11_DataInputStream {

   /**
    * @param args
    * @throws IOException 
    */
   public static void main(String[] args) throws IOException {
      //demo1();
      DataInputStream dis = new DataInputStream(new FileInputStream("h.txt"));
      int x = dis.readInt();
      int y = dis.readInt();
      int z = dis.readInt();
      System.out.println(x);
      System.out.println(y);
      System.out.println(z);
      
      dis.close();
   }

   public static void demo1() throws FileNotFoundException, IOException {
      FileInputStream fis = new FileInputStream("h.txt");
      int x = fis.read();
      int y = fis.read();
      int z = fis.read();
      
      System.out.println(x);
      System.out.println(y);
      System.out.println(z);
      
      fis.close();
   }

}
```



### IO流(Properties的概述和作为Map集合的使用)

* A:Properties的概述
  * Properties 类表示了一个持久的属性集。
  * Properties 可保存在流中或从流中加载。
  * 属性列表中每个键及其对应值都是一个字符串。 
* B:案例演示
  * Properties作为Map集合的使用







### IO流(Properties的特殊功能使用)
* A:Properties的特殊功能
  * public Object setProperty(String key,String value)
  * public String getProperty(String key)
  * public Enumeration<String> stringPropertyNames()
* B:案例演示
  * Properties的特殊功能

### IO流(Properties的load()和store()功能)
* A:Properties的load()和list()功能
* B:案例演示
  * Properties的load()和list()功能






```java
package cn.itcast.otherio;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.PrintStream;
import java.util.Enumeration;
import java.util.Properties;

public class Demo12_Properties {

   /**
    * @param args
    * @throws IOException 
    * @throws FileNotFoundException 
    */
   @SuppressWarnings("unchecked")
   public static void main(String[] args) throws FileNotFoundException, IOException {
      //demo1();
      //demo2();
      Properties prop = new Properties();
      prop.load(new FileInputStream("config.properties"));      //获取文件上的数据
      prop.setProperty("username", "lisi");
      prop.list(new PrintStream("config.properties"));         //将数据写到文件上
      System.out.println(prop);
   }

   public static void demo2() {
      Properties prop = new Properties();
      prop.setProperty("username", "zhangsan");
      prop.setProperty("password", "123456");
      prop.setProperty("qq", "654321");
      prop.setProperty("tel", "18987654321");
      
      Enumeration<String> en = (Enumeration<String>) prop.propertyNames();
      while(en.hasMoreElements()) {
         String key = en.nextElement();
         String value = prop.getProperty(key);
         System.out.println(key + "=" + value);
      }
   }

   public static void demo1() {
      Properties prop = new Properties();
      prop.setProperty("username", "zhangsan");
      prop.setProperty("password", "123456");
      prop.setProperty("qq", "654321");
      prop.setProperty("tel", "18987654321");
      
      String value = prop.getProperty("username");
      System.out.println(value);
      System.out.println(prop);
   }

}
```



### 练习（算法）

```java
package cn.itcast.otherio;

public class Demo13 {

   /**
    * @param args
    * 算法题
    * 1,有x个人,分配房间,每个房间住6个人,问x个人住几个房间
    * (x + 5) / 6
    * 2,在控制台无限打印0-9
    */
   public static void main(String[] args) {
      /*int x = -1;
      while(true) {
         x = ++x % 10;
         System.out.println(x);
      }*/
      
      int x = 0;
      while(true) {
         if(x == 1000) {
            x = 0;
         }
         System.out.println(x++ % 10);
      }
   }

}
```



### 拷贝文件夹内容

```java
package site.yanhui.test;

import java.io.*;
import java.util.Scanner;

public class Test {

    /**
     * 	 * 5,从键盘接收两个文件夹路径,把其中一个文件夹中(包含内容)拷贝到另一个文件夹中

     * @param args
     */
    public static void main(String[] args) throws IOException {


        File src = getDir();
       File dest =getDir();

       if (src.equals(dest)){

           System.out.println("输入有误");

       }else{

           CopyFiles(src,dest);
       }

    }


    /**
     * 控制台输入
     * @return
     */

    public static File getDir(){

        Scanner scanner= new Scanner(System.in);

        System.out.println("请输出一个文件夹路径：");

        while (true) {
            String line = scanner.nextLine();
             File dir = new File(line);

             if (dir.isFile()){
                 System.out.println("输入有误，请重新输入");
             }else if (!dir.exists()){
                 System.out.println("输入有误重新输入");
             }else{
                 return dir;
             }

        }

    }


   /*
     * 拷贝带内容的文件夹
	 * 1,返回值类型void
	 * 2,参数列表File src,File dest
	 */
    public  static  void  CopyFiles(File src,File dest) throws IOException {

        File newDir= new File(dest,src.getName()); //指定dest目的的根目录+文件夹的名字

      
        newDir.mkdir(); //创建该文件夹

        File[] files = src.listFiles();

        for (File subFile : files) {

            if (subFile.isFile()){

                FileInputStream fis= new FileInputStream(subFile);//从src源文件夹read
                FileOutputStream fos= new FileOutputStream(new File(newDir,subFile.getName()));//写到新的文件夹下的文件里面，
                                                                             //名字就是subfile里面的名字，拷贝嘛
                byte[] arr = new byte[8192];//自己弄一个字节数组拷贝，一次8k
                int len;
                while ((len= fis.read(arr))!=-1){

                    fos.write(arr,0,len);
                }

                fis.close();
                fos.close();

            }else {
                CopyFiles(subFile, newDir); //如果遇到文件夹就一直递归下去。
            }
        }
    }

}

```



