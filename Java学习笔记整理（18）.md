---
title: Java学习笔记整理（18）
date: 2017-08-03 15:09:59
tags: [J2SE,集合框架]
categories: J2SE
top: 18 
---



* Map的结构以及使用原理
* KeySet and Entryset and size方法（双链集合的两种遍历）
* 保证键的唯一性需要重写hashCode和equals方法
* 需要排序的话重写comepareTo方法或者是传入比较器
* Hashtable和HashMap的区别
* Collecions 和Collections的区别
* 模拟斗地主洗发牌

<!--more-->

###  Map集合概述和特点

- A:Map接口概述
  - 查看API可以知道：
    - 将键映射到值的对象
    - 一个映射不能包含重复的键
    - 每个键最多只能映射到一个值
- B:Map接口和Collection接口的不同
  - Map是双列的,Collection是单列的
  - Map的键唯一,Collection的子体系Set是唯一的
  - Map集合的数据结构值针对键有效，跟值无关;Collection集合的数据结构是针对元素有效



### Map集合的功能概述

- A:Map集合的功能概述
  - a:添加功能
    - V put(K key,V value):添加元素。
      - 如果键是第一次存储，就直接存储元素，返回null
      - 如果键不是第一次存在，就用值把以前的值替换掉，返回以前的值
  - b:删除功能
    - void clear():移除所有的键值对元素
    - V remove(Object key)：根据键删除键值对元素，并把值返回
  - c:判断功能
    - boolean containsKey(Object key)：判断集合是否包含指定的键
    - boolean containsValue(Object value):判断集合是否包含指定的值
    - boolean isEmpty()：判断集合是否为空
  - d:获取功能
    - Set<Map.Entry<K,V>> entrySet():???
    - V get(Object key):根据键获取值
    - Set<K> keySet():获取集合中所有键的集合
    - Collection<V> values():获取集合中所有值的集合
  - e:长度功能
    - int size()：返回集合中的键值对的个数



```java
package cn.itcast.map;

import java.util.Collection;
import java.util.HashMap;

public class Demo1_Map {

    /**
     * @param args HashSet底层依赖于HashMap
     *             TreeSet底层依赖于TreeMap
     *             a:添加功能
     *             V put(K key,V value):添加元素。
     *             如果键是第一次存储，就直接存储元素，返回null
     *             如果键不是第一次存在，就用值把以前的值替换掉，返回以前的值
     *             b:删除功能
     *             void clear():移除所有的键值对元素
     *             V remove(Object key)：根据键删除键值对元素，并把值返回
     *             c:判断功能
     *             boolean containsKey(Object key)：判断集合是否包含指定的键
     *             boolean containsValue(Object value):判断集合是否包含指定的值
     *             boolean isEmpty()：判断集合是否为空
     *             d:获取功能
     *             Set<Map.Entry<K,V>> entrySet():???
     *             V get(Object key):根据键获取值
     *             Set<K> keySet():获取集合中所有键的集合
     *             Collection<V> values():获取集合中所有值的集合
     *             e:长度功能
     *             int size()：返回集合中的键值对的对数
     */
    public static void main(String[] args) {
        demo1();
        //demo2();
        //demo3();
//		demo4();
    }

    private static void demo4() {
        HashMap<String, String> hm = new HashMap<>();
        hm.put("张三", "北京");
        hm.put("李四", "上海");
        hm.put("王五", "广州");
        hm.put("赵六", "北京");
        String value = hm.get("xx");                    //根据键获取值,如果没有指定的键返回null
        System.out.println(value);
        Collection<String> c = hm.values();                //获取所有值
        System.out.println(c);

        System.out.println(hm.size());                    //获取键值对的个数
    }

    public static void demo3() {
        HashMap<String, String> hm = new HashMap<>();
        hm.put("张三", "北京");
        hm.put("李四", "上海");
        hm.put("王五", "广州");
        hm.put("赵六", "北京");
        boolean b1 = hm.containsKey("张三");                //判断是否包含指定的键
        boolean b2 = hm.containsKey("xxx");
        System.out.println(b1);
        System.out.println(b2);
        System.out.println("--------------------------");
        boolean b3 = hm.containsValue("北京");            //判断是否包含指定的值
        boolean b4 = hm.containsValue("深圳");
        System.out.println(b3);
        System.out.println(b4);
        System.out.println("--------------------------");
        boolean b5 = hm.isEmpty();                        //判断集合是否为空
        hm.clear();
        boolean b6 = hm.isEmpty();
        System.out.println(b5);
        System.out.println(b6);
        System.out.println(hm);
    }

    public static void demo2() {
        HashMap<String, String> hm = new HashMap<>();
        hm.put("张三", "北京");
        hm.put("李四", "上海");
        hm.put("王五", "广州");
        hm.put("赵六", "北京");
        System.out.println(hm);
        //hm.clear();
        String value = hm.remove("张三");        //根据键删除,返回的是键对应的值
        System.out.println(value);
        System.out.println(hm);
    }

    public static void demo1() {
        HashMap<String, String> hm = new HashMap<>();
        String s1 = hm.put("张三", "北京");            //第一次存储返回的是null
        String s2 = hm.put("张三", "上海");            //第二次存储返回的是被覆盖的值
        String s3 = hm.put("李四", "广州");            //HashMap的键是唯一的
        String s4 = hm.put("李四", "深圳");
        String s5 = hm.put("王五", "深圳");            //HashMap的值不是唯一的

        System.out.println(s1);
        System.out.println(s2);
        System.out.println(s3);
        System.out.println(s4);
        System.out.println(hm);

		/*输出
null
北京
null
广州
{李四=深圳, 张三=上海, 王五=深圳}
		 */
    }

}

```



### Map集合的遍历之键找值

- A:键找值思路：

  - 获取所有键的集合
  - 遍历键的集合，获取到每一个键
  - 根据键找值

- B:案例演示

  - Map集合的遍历之键找值

    ​

    ```java
    package cn.itcast.map;

    import java.util.HashMap;
    import java.util.Iterator;
    import java.util.Set;

    public class Demo2_KeySet {

    	/**
    	 * @param args
    	 * 遍历双列集合第一种方式
    	 */
    	public static void main(String[] args) {
    		//使用包装类
    		HashMap<String, Integer> hm = new HashMap<>();
    		hm.put("张三", 23);
    		hm.put("李四", 24);
    		hm.put("王五", 25);
    		hm.put("赵六", 26);
    		hm.put("赵六", 266);//覆盖了之前存的26
    ```


    			/*Set<String> keySet = hm.keySet();			//获取集合中所有的键
    		Iterator<String> it = keySet.iterator();	//获取迭代器
    		while(it.hasNext()) {						//判断单列集合中是否有元素
    			String key = it.next();					//获取集合中的每一个元素,其实就是双列集合中的键
    			Integer value = hm.get(key);			//根据键获取值
    			System.out.println(key + "=" + value);	//打印键值对
    		}


    		for(String key : hm.keySet()) {				//增强for循环迭代双列集合第一种方式
    			System.out.println(key + "=" + hm.get(key));
    		}*/
    
            Set<String> strings1 = hm.keySet();
            for (String s : strings1) {
    
                System.out.println(s+"="+hm.get(s));
            }
    
            System.out.println("======================");
            Set<String> strings = hm.keySet();
            Iterator<String> iterator = strings.iterator();
            while (iterator.hasNext()){
                String next = iterator.next();//这个next的值就是键
                System.out.println( next+"="+hm.get(next));
            }


        }

    }

    ​```

### Map集合的遍历之键值对对象找键和值

- A:键值对对象找键和值思路：

  - 获取所有键值对对象的集合
  - 遍历键值对对象的集合，获取到每一个键值对对象
  - 根据键值对对象找键和值

- B:案例演示

  - Map集合的遍历之键值对对象找键和值

    ​

    ```java
    package cn.itcast.map;

    import java.util.HashMap;
    import java.util.Iterator;
    import java.util.Map.Entry;
    import java.util.Set;

    public class Demo3_EntrySet {

    	/**
    	 * @param args
    	 * 遍历双列集合的第二种方式
    	 *  Set<Map.Entry<张三,23>>
    	 *  Set<new Person(张三,23)>
    	 *  Map集合entrySet()方法
    	 */
    	public static void main(String[] args) {
    		HashMap<String, Integer> hm = new HashMap<>();
    		hm.put("张三", 23);
    		hm.put("李四", 24);
    		hm.put("王五", 25);
    		hm.put("赵六", 26);

    //        Set<Entry<String, Integer>> entries = hm.entrySet();
    		/*Set<Map.Entry<String, Integer>> entrySet = hm.entrySet();	//获取所有的键值对象的集合
    		Iterator<Entry<String, Integer>> it = entrySet.iterator();//获取迭代器
    		while(it.hasNext()) {
    			Entry<String, Integer> en = it.next();				//获取键值对对象
    			String key = en.getKey();								//根据键值对对象获取键
    			Integer value = en.getValue();							//根据键值对对象获取值
    			System.out.println(key + "=" + value);
    		}*/
    		
    		for(Entry<String,Integer> en : hm.entrySet()) {
    			System.out.println(en.getKey() + "=" + en.getValue());
    		}
    ```


    		//两种遍历方式，使用foreach语句是最优化的选择。
            System.out.println("==============="); 
            Set<Entry<String, Integer>> entries = hm.entrySet();
            Iterator<Entry<String, Integer>> iterator = entries.iterator();
    
            while (iterator.hasNext()) {
                Entry<String, Integer> next = iterator.next();
                System.out.println(next.getKey()+"="+next.getValue());
    
            }
    
        }
    
    }
    
    ​```

### Map集合遍历的两种方式比较图解

- A:画图演示
  - Map集合遍历的两种方式比较

![](http://ogtmd8elu.bkt.clouddn.com/201708031743_382.png)



![](http://ogtmd8elu.bkt.clouddn.com/201708031744_288.png)





###  HashMap集合键是Student值是String的案例

- A:案例演示
  - HashMap集合键是Student值是String的案例

```java
package cn.itcast.map;

import java.util.HashMap;
import java.util.Map;
import java.util.Set;

import cn.itcast.bean.Person;

public class Demo5_HashMap {

	/**
	 * @param args
	 * HashMap想保证键的唯一依赖于hashCode()和equals方法
	 */
	public static void main(String[] args) {
		HashMap<Person, String> hm = new HashMap<>();
		hm.put(new Person("张三", 23), "北京");
//		hm.put(new Person("张三", 23), "北京");
		hm.put(new Person("李四", 24), "上海");

        Set<Map.Entry<Person, String>> entries = hm.entrySet();
        for (Map.Entry<Person, String> entry : entries) {

            System.out.println(entry.getKey()+"="+entry.getValue());
        }
        System.out.println(hm);
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
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Person other = (Person) obj;
		if (age != other.age)
			return false;
		if (name == null) {
			if (other.name != null)
				return false;
		} else if (!name.equals(other.name))
			return false;
		return true;
	}
	@Override
	public int compareTo(Person p) {
		int num = this.age - p.age;
		return num == 0 ? this.name.compareTo(p.name) : num;
	}
	
	
}

```



### LinkedHashMap的概述和使用

- A:案例演示
  - LinkedHashMap的特点
    - 底层是链表实现的可以保证怎么存就怎么取

```java
package cn.itcast.map;

import java.util.HashMap;
import java.util.LinkedHashMap;

public class Demo6_LinkedHashMap {

	/**
	 * @param args
     * 元素不重复，怎么存怎么取。
	 */
	public static void main(String[] args) {
		HashMap<String, Integer> hm = new LinkedHashMap<>();						//怎么存就怎么取
		hm.put("张三", 23);
		hm.put("李四", 24);
		hm.put("王五", 25);
		hm.put("赵六", 26);
		
		System.out.println(hm);
	}

}

```



### TreeMap集合键和值都是String类型的

```java
public static void demo1() {
   TreeMap<String, String> tm = new TreeMap<>(); //只是比较键，按照abcd排，不比较值
   tm.put("c", "1");
   tm.put("a", "3");
   tm.put("b", "4");
   tm.put("d", "2");
   
   System.out.println(tm);
}
```





### TreeMap集合键是Student值是String的案例

- A:案例演示
  - TreeMap集合键是Student值是String的案例

 ```java
package site.Yanhui.map;

import site.Yanhui.bean.Person;

import java.util.Comparator;

/*
实现了两种排序的办法，自然排序和自定义一个比较器排序。
系统会优先使用自己定义的比较器进行排序。


 */
public class TreeMap {

    public static void main(String[] args) {


    java.util.TreeMap<Person, String> mTreeMap= new java.util.TreeMap<>(new Comparator<Person>() {
        @Override
        public int compare(Person o1, Person o2) {

            //自己实现一个构造去完成以姓名去排序（名字的编码值）
            int num= o1.getName().compareTo(o2.getName());

            return num==0 ? o1.getAge()-o2.getAge():num;


        }
    });

        mTreeMap.put(new Person("李四",1),"上海");
        mTreeMap.put(new Person("张三",23),"北京");
        mTreeMap.put(new Person("王五",25),"云南");
        mTreeMap.put(new Person("赵六",26),"大连");
        //实现了自然排序，用年龄进行排序
        System.out.println(mTreeMap);

    }
}

 ```







```java
package site.Yanhui.bean;

public class Person implements Comparable<Person>{
    private  String name;
    private  int age;

    public Person() {
    }

    public Person(String name, int age) {
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
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        Person person = (Person) o;

        if (age != person.age) return false;
        return name != null ? name.equals(person.name) : person.name == null;
    }

    @Override
    public int hashCode() {
        int result = name != null ? name.hashCode() : 0;
        result = 31 * result + age;
        return result;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public int compareTo(Person o) {

        //年龄进行排序
        int num = this.age-o.age;

        return num==0 ?  this.name.compareTo(o.name):num;
    }
}
```

### 统计字符串中每个字符出现的次数

- A:案例演示

  - 需求：统计字符串中每个字符出现的次数
    ​

    ```java
    package site.Yanhui.text;

    import java.util.HashMap;
    import java.util.Map;
    import java.util.Scanner;

    /**
     * hashMap的遍历是用 entrySet 和 keySet实现的
     *
     *
     * * A:案例演示
     * 需求：统计字符串中每个字符出现的次数
     */
    public class Test {

        public static void main(String[] args) {
            Scanner scanner=new Scanner(System.in);
            System.out.println("请输出一个字符串");
            String s = scanner.nextLine();

            char[] chars = s.toCharArray();

            HashMap<Character ,Integer> hashMap =new HashMap<>();
            for (char aChar : chars) {

    //            if (!hashMap.containsKey(aChar)){
    //                hashMap.put(aChar,1);
    //            }else {
    //                hashMap.put(aChar,hashMap.get(aChar)+1);
    //            }
                //如果hashmap里面不包含值的话，传入1，包含的话就传入hashMap.get(aChar)+1
                //如果包含，hanshMap会覆盖掉之前的数值
                hashMap.put(aChar,!hashMap.containsKey(aChar)? 1:hashMap.get(aChar)+1);

            }

            for (Map.Entry<Character, Integer> characterIntegerEntry : hashMap.entrySet()) {

                System.out.print(characterIntegerEntry.getKey()+"次数是："+characterIntegerEntry.getValue()+"， ");
            }

        }

    }

    ```

###  集合嵌套之HashMap嵌套HashMap

- A:案例演示
  - 集合嵌套之HashMap嵌套HashMap



```java
package cn.itcast.map;

import java.util.HashMap;

import cn.itcast.bean.Person;


public class Demo8_HashMapHashMap {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		HashMap<String, HashMap<Person, String>> hm = new HashMap<>();		//创建HashMap,键是String,值是HashMap
		
		HashMap<Person, String> jc0302 = new HashMap<>();					//创建HashMap集合对象,键是Person,值是String
		jc0302.put(new Person("张三", 23), "北京");
		jc0302.put(new Person("李四", 24), "上海");
		jc0302.put(new Person("马哥", 18), "辽宁");
		
		HashMap<Person, String> jc0309 = new HashMap<>();					//创建HashMap集合对象,键是Person,值是String
		jc0309.put(new Person("王五", 25), "毛里求斯");
		jc0309.put(new Person("赵六", 26), "铁岭");
		
		hm.put("0302基础班", jc0302);
		hm.put("0309基础班", jc0309);
		
		for(String key : hm.keySet()) {										//遍历嵌套的集合,String key代表hm中的每一个键
			HashMap<Person, String> value = hm.get(key);					//根据key获取的值是HashMap对象
			for(Person k : value.keySet()) {								//再遍历这个HashMap集合对象
				System.out.print(key + "," +k + "," + value.get(k) + "\t");
			}
			System.out.println();
		}
	}

}

```





###  HashMap和Hashtable的区别(面试题)

- A:面试题
  - HashMap和Hashtable的区别
    - Hashtable是JDK1.0版本出现的,是线程安全的,效率低,HashMap是JDK1.2版本出现的,是线程不安全的,效率高
    - Hashtable不可以存储null键和null值,HashMap可以存储null键和null值
  - B:案例演示
  - HashMap和Hashtable的区别

```java
package cn.itcast.map;

import java.util.HashMap;
import java.util.Hashtable;

public class Demo9_Hashtable {

	/**
	 * @param args
	 * HashMap和Hashtable的区别
	 * 共同点:都是用哈希算法
	 * 1,Hashtable是JDK1.0版本的,是同步的(线程安全的),效率低
	 * HashMap是JDK1.2版本的,是不同步(线程不安全的),效率高
	 * 2,Hashtable不能存null键和null值
	 * HashMap可以存储null键和null值
	 */
	public static void main(String[] args) {
		/*Hashtable<String, String> ht = new Hashtable<>();
		ht.put("abc", null);
		System.out.println(ht);*/
		HashMap<String, String> hm = new HashMap<>();
		hm.put("abc", null);
		System.out.println(hm);
		
		
		System.out.println("1111111111111111");
	}

}

```



###  Collections工具类的概述和常见方法讲解

- A:Collections类概述

  - 针对集合操作 的工具类

- B:Collections成员方法

- public static <T> void sort(List<T> list)

  ```java
  public static <T> int binarySearch(List<?> list,T key)
  public static <T> T max(Collection<?> coll)
  public static void reverse(List<?> list)
  public static void shuffle(List<?> list)
  ```

```java
package cn.itcast.collections;

import java.util.ArrayList;
import java.util.Collections;

public class Demo1_Collections {

	/**
	 * public static <T> void sort(List<T> list)
		public static <T> int binarySearch(List<?> list,T key)
		public static <T> T max(Collection<?> coll)
		public static void reverse(List<?> list)
		public static void shuffle(List<?> list)
	 */
	public static void main(String[] args) {
		ArrayList<String> list = new ArrayList<>();
		list.add("c");
		list.add("a");
		list.add("b");
		list.add("e");
		list.add("d");
		
		/*Collections.sort(list);						//排序
		int index = Collections.binarySearch(list, "c");//二分查找法
		String max = Collections.max(list);			//获取最大值
		System.out.println(index);
		System.out.println(list);
		System.out.println(max);*/
		
		//Collections.reverse(list);					//将集合进行反转
        Collections.shuffle(list);
		System.out.println(list);
	}

}

```



### 模拟斗地主洗牌和发牌

- A:案例演示

  - 模拟斗地主洗牌和发牌，牌没有排序

    ​

    ```java
    //买一副扑克
    String[] num = {"A","2","3","4","5","6","7","8","9","10","J","Q","K"};
    String[] color = {"方片","梅花","红桃","黑桃"};
    ArrayList<String> poker = new ArrayList<>();

    for(String s1 : color) {
    	for(String s2 : num) {
    		poker.add(s1.concat(s2));
    	}
    }

    poker.add("小王");
    poker.add("大王");
    //洗牌
    Collections.shuffle(poker);
    //发牌
    ArrayList<String> gaojin = new ArrayList<>();
    ArrayList<String> longwu = new ArrayList<>();
    ArrayList<String> me = new ArrayList<>();
    ArrayList<String> dipai = new ArrayList<>();

    for(int i = 0; i < poker.size(); i++) {
    	if(i >= poker.size() - 3) {
    		dipai.add(poker.get(i));
    	}else if(i % 3 == 0) {
    		gaojin.add(poker.get(i));
    	}else if(i % 3 == 1) {
    		longwu.add(poker.get(i));
    	}else {
    		me.add(poker.get(i));
    	}
    }

    //看牌

    System.out.println(gaojin);
    System.out.println(longwu);
    System.out.println(me);
    System.out.println(dipai);
    ```

### 模拟斗地主洗牌和发牌并对牌进行排序的原理图解

- A:画图演示
  - 画图说明排序原理

![](http://ogtmd8elu.bkt.clouddn.com/201708040928_310.png)

### 模拟斗地主洗牌和发牌并对牌进行排序的代码实现

- A:案例演示

  - 模拟斗地主洗牌和发牌并对牌进行排序的代码实现

    ​

  ```java
  package cn.itcast.collections;

  import java.util.ArrayList;
  import java.util.Collections;
  import java.util.HashMap;
  import java.util.TreeSet;

  public class Demo3_Poker {

  	/**
  	 * @param args
  	 * 模拟斗地主排序
  	 */
  	public static void main(String[] args) {
  		//买一副牌
  		String[] num = {"3","4","5","6","7","8","9","10","J","Q","K","A","2"};
  		String[] color = {"方片","梅花","红桃","黑桃"};
  		HashMap<Integer, String> hm = new HashMap<>();			//存储索引和扑克牌
  		ArrayList<Integer> list = new ArrayList<>();			//存储索引
  		int index = 0;											//索引的开始值
  		for(String s1 : num) {
  			for(String s2 : color) {
  				hm.put(index, s2.concat(s1));					//将索引和扑克牌添加到HashMap中
  				list.add(index);								//将索引添加到ArrayList集合中
  				index++;
  			}
  		}
  		hm.put(index, "小王");
  		list.add(index);
  		index++;
  		hm.put(index, "大王");
  		list.add(index);
  		//洗牌
  		Collections.shuffle(list);
  		//发牌
  		TreeSet<Integer> gaojin = new TreeSet<>();
  		TreeSet<Integer> longwu = new TreeSet<>();
  		TreeSet<Integer> me = new TreeSet<>();
  		TreeSet<Integer> dipai = new TreeSet<>();
  		
  		for(int i = 0; i < list.size(); i++) {
  			if(i >= list.size() - 3) {
  				dipai.add(list.get(i)); 						//将list集合中的索引添加到TreeSet集合中会自动排序
  			}else if(i % 3 == 0) {
  				gaojin.add(list.get(i));
  			}else if(i % 3 == 1) {
  				longwu.add(list.get(i));
  			}else {
  				me.add(list.get(i));
  			}
  		}
  		
  		//看牌
  		lookPoker("高进", gaojin, hm);
  		lookPoker("龙五", longwu, hm);
  		lookPoker("野望", me, hm);
  		lookPoker("底牌", dipai, hm);
  		
  	}
  	
  	public static void lookPoker(String name,TreeSet<Integer> ts,HashMap<Integer, String> hm) {
  		System.out.print(name + "的牌是:");
  		for (Integer index : ts) {
  			System.out.print(hm.get(index) + " ");
  		}
  		
  		System.out.println();
  	}

  }

  ```


### 泛型固定下边界

- ? super E





```java
package cn.itcast.collections;

import java.util.ArrayList;
import java.util.Comparator;
import java.util.TreeSet;

import cn.itcast.bean.Person;
import cn.itcast.bean.Worker;

public class Demo4_TreeSet {

	/**
	 * @param args
	 * 高级泛型
	 * ? extends E  ?是自乐E是父类固定的(Person) 可以加入到父类的集合里面
	 * ? super E    E是子类?是父类 可以调用的父类的比较器
	 */
	public static void main(String[] args) {
		//demo1();
		TreeSet<Person> ts1 = new TreeSet<>(new CompareByAge());
		ts1.add(new Person("张三", 23));
		ts1.add(new Person("李四", 24));
		
		TreeSet<Worker> ts2 = new TreeSet<>(new CompareByAge());
		ts2.add(new Worker("王五", 25));
		ts2.add(new Worker("赵六", 15));
		ts2.add(new Worker("周七", 35));
		
		System.out.println(ts2);
	}

	public static void demo1() {
		ArrayList<Person> list1 = new ArrayList<>(); 
		list1.add(new Person("张三", 23));
		list1.add(new Person("李四", 24));
		
		ArrayList<Worker> list2 = new ArrayList<>(); 
		list2.add(new Worker("王五", 25));
		
		list1.addAll(list2);
	}

}

class CompareByAge implements Comparator<Person> {

	@Override
	public int compare(Person p1, Person p2) {
		int num = p1.getAge() - p2.getAge();
		return num == 0 ? p1.getName().compareTo(p2.getName()) : num;
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
      if (this == obj)
         return true;
      if (obj == null)
         return false;
      if (getClass() != obj.getClass())
         return false;
      Person other = (Person) obj;
      if (age != other.age)
         return false;
      if (name == null) {
         if (other.name != null)
            return false;
      } else if (!name.equals(other.name))
         return false;
      return true;
   }
   @Override
   public int compareTo(Person p) {
      int num = this.age - p.age;
      return num == 0 ? this.name.compareTo(p.name) : num;
   }
   
   
}
```

```java
package cn.itcast.bean;

public class Worker extends Person {

	public Worker() {
	}

	public Worker(String name, int age) {
		super(name, age);

	}
}

```

