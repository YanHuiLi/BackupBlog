---
title: Java学习笔记（9）
date: 2017-07-19 08:30:49
tags: [J2SE]
categories: [J2SE]
top: 9
---

---

1. 多态
2. 向上转型和向下转型。
3. 多态的优缺点。
4. 必须通过向下转型，得到子类的特性。
5. 抽象类
6. 接口



<!--more-->

多态的概述及其代码体现

- A:多态概述
  - 事物存在的多种形态 
- B:多态前提
  - a:要有继承关系。
  - b:要有方法重写。
  - c:要有父类引用指向子类对象。
- C:案例演示
  - 代码体现多态

```java
  class Demo1DuoTai {
      public static void main(String[] args) {
          Cat c = new Cat();
          //c.eat();
  
          Animal a = new Cat();                   //父类引用指向子类对象
          a.eat();
  
          Animal a2 = new Dog();
          //a2.eat();
  
          /*
          多态的三个前提
          1,继承
          2,要有重写（否则只是继承了父类的方法，并不是多态）
          3,父类引用指向子类对像
          */
      }
  }
  
  class Animal {
      public void eat() {
          System.out.println("动物吃饭");
      }
  }
  
  class Cat extends Animal {
      public void eat() {
          System.out.println("猫吃鱼");
      }
  }
  
  class Dog extends Animal {
      public void eat() {
          System.out.println("狗吃肉");
      }
  }

```

### 多态中的成员访问特点

- A:多态中的成员访问特点

  - a:成员变量

    - 编译看左边，运行看左边。

    ![img](http://ogtmd8elu.bkt.clouddn.com/201707190847_519.png)

    当zi继承fu以后，开辟的对象空间里面存有从fu继承而来的区域，当使用`Fu f =new Zi()` f得到的句柄是指向zi里存储的F继承的区域，因此拿到的f.number的值是10，而不是20.

  - b:成员方法

    - 编译看左边，运行看右边。

    ​

    ![img](http://ogtmd8elu.bkt.clouddn.com/201707190859_715.png)

    在执行成员方法的时候，存在动态绑定，jvm在编译的时候，会先检查`Fu.class`里面有没有print()方法，如果有则去执行zi继承fu的print方法里面的内容，这就是多态的动态绑定，如果不存在fu的print方法，则输出就报错，这也是以后编程非常方便的一点，即是确定了父类的类型以后，可以不用考虑子类的类型的选择，灵活的进行调用。

  - c:静态方法

    - 编译看左边，运行看左边。
    - (静态和类相关，算不上重写，所以，访问还是左边的)

    不存在动态绑定。


- B:案例演示
  - 多态中的成员访问特点

```java
  class Demo2DuoTai {
      public static void main(String[] args) {
          /*Fu f = new Zi();
          System.out.println(f.num);
  
          Zi z = new Zi();
          System.out.println(z.num);*/
  
          Fu f = new Zi();
          //f.print();
          f.method();
          Fu.method();
      }
  }
  /*
  多态中的
      成员变量:编译看左边,运行看左边
      成员方法:编译看左边,运行看右边(动态绑定)
      静态的成员方法:编译看左边,运行看左边
      静态的成员方法不存在覆盖的说法
  
      只有非静态的成员方法,编译看左边(父类)运行看右边(子类)
  */
  class Fu {
      int num = 10;
  
      public void print() {
          System.out.println("Fu");
      }
  
      public static void method() {
          System.out.println("Fu static");
      }
  }
  
  class Zi extends Fu {
      int num = 20;
  
      public void print() {
          System.out.println("Zi");
      }
  
      public static void method() {
          System.out.println("Zi static");
      }
  }
  

```

### 超人的故事

- A:案例分析
  - 通过该案例帮助学生理解多态的现象

![img](http://ogtmd8elu.bkt.clouddn.com/201707190924_980.png)

```java
  class Demo3DuoTai {
      public static void main(String[] args) {
          Person p = new SuperMan();              //父类引用指向子类对象(自动类型提升,向上转型)
          System.out.println(p.name);
  
          p.谈生意();
          
          SuperMan sm = (SuperMan)p;              //向下转型
          sm.fly();
      }
  }
  class Person {
      String name = "约翰";
      public void 谈生意() {
          System.out.println("谈生意");
      }
  }
  
  class SuperMan extends Person {
      String name = "超人";
  
      public void 谈生意() {
          System.out.println("谈几个亿的大单子");
      }
  
      public void fly() {
          System.out.println("飞出去救人");
      }
  }

```

### 多态中向上转型和向下转型

- A:案例演示
  - 详细讲解多态中向上转型和向下转型
  - 父类是Animal,子类是Cat
  - Animal a = new Cat();向上转型
  - Cat c = new Animal();错误的
  - Cat c = (Cat)a;向下转型

```
  * Cat c = new Cat();
  * Animal a = (Animal)c;         //其实加不加Animal都是一样的,都是向上转型
  *                               //c其实就是猫


```

```java
  class Demo4DuoTai {
      public static void main(String[] args) {
          榨汁机 z = new 榨汁机();
          z.run(new 苹果());
          z.run(new 橙子());
      }
  }
  
  class 水果 {
      public void 榨汁() {
          System.out.println("榨水果汁儿");
      }
  }
  
  class 苹果 extends 水果 {
      public void 榨汁() {
          System.out.println("榨了一杯苹果汁儿");
      }
  }
  
  class 橙子 extends 水果 {
      public void 榨汁() {
          System.out.println("榨了一杯橙子汁儿");
      }
  }
  
  class 榨汁机 {
      /*public void run(苹果 a) {       //苹果 a = new 苹果();
          a.榨汁();
      }
  
      public void run(橙子 o) {     
          o.榨汁();
      }*/
  
      public void run(水果 f) {     //水果 f = new 苹果();
          f.榨汁();
      }
  }

```

### 多态的好处和弊端

- A:多态的好处
  - a:提高了代码的维护性(继承保证)
  - b:提高了代码的扩展性(由多态保证)
- B:案例演示
  - 多态的好处
- C:多态的弊端
  - 不能使用子类的特有功能。


- D:案例演示

  - 多态的弊端

  ```java
    class Demo5DuoTai {
        public static void main(String[] args) {
            Animal a = new Cat();
            a.eat();
            //a.catchMouse();               //不能调用子类特有的方法
            Cat c = (Cat)a;                 //向下转型
            c.catchMouse();
        }
    }
    
    class Animal { 
        public void eat() {
            System.out.println("吃饭");
        }
    }
    
    class Cat extends Animal {
        public void eat() {
            System.out.println("吃鱼");
        }
    
        public void catchMouse() {
            System.out.println("抓老鼠");
        }
    }
    
    class Dog extends Animal {
        public void eat() {
            System.out.println("吃肉");
        }
    
        public void lookHome() {
            System.out.println("看家");
        }
    }

  ```

  ​

### 多态中的题目分析题

- A:看下面程序是否有问题，如果没有，说出结果

- ​

  ```java
    class Fu {  
    public void show() {
            System.out.println("fu show");
        }
    }
    
    class Zi extends Fu {
        public void show() {
            System.out.println("zi show");
        }
    
        public void method() {
            System.out.println("zi method");
        }
    }
    
    class Test1Demo {
        public static void main(String[] args) {
            Fu f = new Zi();
            //f.method();不能访问子类的特性，必须先向下转型，才可以访问到子类的特性方法。
            f.show();
        }
    }

  ```

- B:看下面程序是否有问题，如果没有，说出结果

  ​

  ```java
    class A {   
    
    public void show() {
            show2();
        }
        public void show2() {
            System.out.println("我");
        }
    }
    class B extends A {
        public void show2() {
            System.out.println("爱");
        }
    }
    class C extends B {
        public void show() {
            super.show();
        }
        public void show2() {
            System.out.println("你");
        }
    }
    public class Test2DuoTai {
        public static void main(String[] args) {
            A a = new B();
            a.show();
            
            B b = new C();
            b.show();
          
          //out
          /*
          爱
          你
          */
        }
    }

  ```

### 抽象类的概述及其特点

- A:抽象类概述

- B:抽象类特点

  - a:抽象类和抽象方法必须用abstract关键字修饰
    - abstract class 类名 {}
    - public abstract void eat();
  - b:抽象类不一定有抽象方法，有抽象方法的类一定是抽象类
  - c:抽象类不能实例化那么，抽象类如何实例化呢?
    - 按照多态的方式，由具体的子类实例化。其实这也是多态的一种，抽象类多态。
  - d:抽象类的子类
    - 要么是抽象类
    - 要么重写抽象类中的所有抽象方法

- C:案例演示

  - 抽象类特点

  ​

### 抽象类的成员特点

- A:抽象类的成员特点
  - a:成员变量：既可以是变量，也可以是常量。
  - b:构造方法：有。
    - 用于子类访问父类数据的初始化。
  - c:成员方法：既可以是抽象的，也可以是非抽象的。
- B:案例演示
  - 抽象类的成员特点
- C:抽象类的成员方法特性：
  - a:抽象方法 强制要求子类做的事情。
  - b:非抽象方法 子类继承的事情，提高代码复用性。

```java
  class Demo3Abstract {
      public static void main(String[] args) {
          Zi z = new Zi();
          z.print();                  //直接调用子类中的print方法
          z.method();
          Fu f = new Zi();
          f.print();                  //编译看父类,运行看子类
          f.method();
      }
  }
  
  abstract class Fu {
      public abstract void print();   //抽象方法必须有子类重写后使用
  
      public void method() {          //非抽象方法子类可以直接继承用
          System.out.println("Hello World!");
      }
  }
  
  class Zi extends Fu {
      public void print() {
          System.out.println("Zi");
      }
  }

```

```java
  class Demo1Abstract {
      public static void main(String[] args) {
          //Animal a = new Animal();
          //a.eat();
  
          //Animal a = new Cat();
          //a.eat();
  
          Cat c = new Cat();
          c.eat();
  
          Dog d = new Dog();
          d.eat();
  
          System.out.println(c.num);
      }
  }
  
  abstract class Animal {                     //抽象类
      int num = 10;
      public abstract void eat();             //抽象方法
  
      public void sleep() {                   //抽象类是不能被实例化(创建对象)
          System.out.println("动物睡觉");
      }
  
      public Animal() {
          
      }
  }
  
   class Cat extends Animal {
      public Cat(){
          super();
      }
      public void eat() {
          System.out.println("吃鱼");
      }
  }
  
  class Dog extends Animal {
      public void eat() {
          System.out.println("吃肉");
      }
  }
  

```

### 葵花宝典

- 案例演示
  - 抽象类的作用 

```java
  class Demo2Abstract {
      public static void main(String[] args) {
          岳不群 小岳子 = new 岳不群();
          小岳子.自宫();
      }
  }
  
  /*
  葵花宝典
  */
  
  abstract class 葵花宝典 {
      public abstract void 自宫();          //声明一种规则,强制子类重写
  }
  
  class 岳不群 extends 葵花宝典 {
      public void 自宫() {
          System.out.println("牙签");
      }
  }
  
  class 林平之 extends 葵花宝典 {
      public void 自宫() {
          System.out.println("指甲刀");
      }
  }
  
  class 东方不败 extends 葵花宝典 {
      public void 自宫() {
          System.out.println("锤子,不忍直视");
      }
  }

```

### 抽象类练习猫狗案例

- A:案例演示
  - 具体事物：猫，狗
  - 共性：姓名，年龄，吃饭
  - 猫的特性:抓老鼠
  - 狗的特性:看家

```java
  class Demo4Abstract {
      public static void main(String[] args) {
          Animal a = new Cat("加菲",5);
          a.eat();
  
          System.out.println(a.getName() + "," + a.getAge());
      }
  }
  
  /*
  * A:案例演示
      * 具体事物：猫，狗
      * 共性：姓名，年龄，吃饭
      * 猫的特性:抓老鼠
      * 狗的特性:看家
  */
  
  abstract class Animal {
      private String name;
      private int age;
  
      public Animal(){}
  
      public Animal(String name,int age) {
          this.name = name;
          this.age = age;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public void setAge(int age) {
          this.age = age;
      }
  
      public String getName() {
          return name;
      }
  
      public int getAge() {
          return age;
      }
  
      public abstract void eat();
  }
  
  class Cat extends Animal {
      public Cat(){}
  
      public Cat(String name,int age) {
          super(name,age);
      }
  
      public void eat() {
          System.out.println("吃鱼");
      }
  }

```

###  抽象类练习老师案例

- A:案例演示

  - 具体事物：基础班老师，就业班老师
  - 共性：姓名，年龄，讲课。

  ```java
    class Demo5Abstract {
        public static void main(String[] args) {
            Teacher t = new BaseTeacher("冯佳",32);
            t.teach();
        }
    }
    
    /*
    * A:案例演示
        * 具体事物：基础班老师，就业班老师
        * 共性：姓名，年龄，讲课。
    */
    abstract class Teacher {
        private String name;
        private int age;
    
        public Teacher(){}
    
        public Teacher(String name,int age) {
            this.name = name;
            this.age = age;
        }
    
        public void setName(String name) {
            this.name = name;
        }
    
        public void setAge(int age) {
            this.age = age;
        }
    
        public String getName() {
            return name;
        }
    
        public int getAge() {
            return age;
        }
    
        public abstract void teach();
    }
    
    class BaseTeacher extends Teacher {
        public BaseTeacher(){}
    
        public BaseTeacher(String name,int age) {
            super(name,age);
        }
    
        public void teach() {
            System.out.println("我的名字是:" +  this.getName() + ",我的年龄是:" + getAge() + ",我讲的javase");
        }
    }   

  ```

  ​

###  抽象类练习员工案例

- A:案例演示

  - 假如我们在开发一个系统时需要对程序员类进行设计，程序员包含3个属性：姓名、工号以及工资。
  - 经理，除了含有程序员的属性外，另为还有一个奖金属性。
  - 请使用继承的思想设计出程序员类和经理类。要求类中提供必要的方法进行属性访问。

  ```java
    class Demo6Abstract {
        public static void main(String[] args) {
            Coder c = new Coder("魏昊","007",7000.00);
            c.work();
            Manager m = new Manager("周怀","9527",5000,20000);
            m.work();
        }
    }
    /*
    * A:案例演示
        * 假如我们在开发一个系统时需要对程序员类进行设计，程序员包含3个属性：姓名、工号以及工资。
        * 经理，除了含有程序员的属性外，另为还有一个奖金属性。
        * 请使用继承的思想设计出程序员类和经理类。要求类中提供必要的方法进行属性访问。
    分析:
        共性:
            属性: 姓名,工号,工资
            行为: 工作
    */
    
    abstract class Employee { 
        private String name;
        private String id;
        private double pay;
    
        public Employee(){}
    
        public Employee(String name,String id,double pay) {
            this.name = name;
            this.id = id;
            this.pay = pay;
        }
    
        public void setName(String name) {
            this.name = name;
        }
    
        public void setId(String id) {
            this.id = id;
        }
    
        public void setPay(double pay) {
            this.pay = pay;
        }
    
        public String getName() {
            return name;
        }
    
        public String getId() {
            return id;
        }
    
        public double getPay() {
            return pay;
        }
    
        public abstract void work();
    }
    
    class Coder extends Employee {
        public Coder(){}
    
        public Coder(String name,String id,double pay) {
            super(name,id,pay);
        }
    
        public void work() {
            System.out.println("我的姓名是:" + getName() + ",我的工号是:" + getId() + ",我的薪资是:" + getPay() + ",我的工作内容是敲代码");
        }
    }
    
    class Manager extends Employee  {
        private int bonus;
    
        public Manager(){}
    
        public Manager(String name,String id,double pay,int bonus) {
            super(name,id,pay);
            this.bonus = bonus;
        }
    
        public void work() {
            System.out.println("我的姓名是:" + getName() + ",我的工号是:" + 
                getId() + ",我的薪资是:" + getPay() + ",我的奖金是:" + bonus + ",我的工作内容是管理");
        }
    }

  ```

  ​

### 抽象类中的面试题

- A:面试题1
  - 一个抽象类如果没有抽象方法，可不可以定义为抽象类?如果可以，有什么意义?
  - 可以
  - 这么做目的只有一个,就是不让其他类创建本类对象,交给子类完成 
- B:面试题2
  - abstract不能和哪些关键字共存

```java
  class Demo7Abstract {
      public static void main(String[] args) {
          //Zi z = new Zi();
          //z.print();
      }
  }
  
  abstract class Fu {
      //private abstract void print();        //Demo7Abstract.java:9: 错误: 非法的修饰符组合: abstract和private
                                          //private是私有不让子类看到,abstract是抽象就是为了让子类重写,他俩矛盾的
      //static abstract void print();     //Demo7Abstract.java:11: 错误: 非法的修饰符组合: abstract和static
                                          //静态修饰的方法,可以直接类名.调用,而类名.调用抽象方法是没有意义的
  
      final abstract void print();        //Demo7Abstract.java:14: 错误: 非法的修饰符组合: abstract和final
                                          //final是最终的,修饰的方法不让子类重写,而abstract修饰的方法就是为了让子类重写
  
  }
  
  class Zi extends Fu {
      public void print() {
          System.out.println("Hello World!");
      }
  }

```

### 接口的概述及其特点

- A:接口概述
  - 从狭义的角度讲就是指java中的interface
  - 从广义的角度讲对外提供规则的都是接口 
- B:接口特点
  - a:接口用关键字interface表示	
    - interface 接口名 {}
  - b:类实现接口用implements表示
    - class 类名 implements 接口名 {}
  - c:接口不能实例化
    - 那么，接口如何实例化呢?
    - 按照多态的方式来实例化。
  - d:接口的子类
    - a:可以是抽象类。但是意义不大。
    - b:可以是具体类。要重写接口中的所有抽象方法。(推荐方案)
- C:案例演示
  - 接口特点

```java
  class Demo1Interface {
      public static void main(String[] args) {
          Inter i = new Demo();
          i.print();
      }
  }
  
  interface Inter {
      public abstract void print();
      
      /*public void method() {                //错误: 接口方法不能带有主体
          System.out.println("aaa");          //接口中所有的方法都是抽象的
      }*/
  }
  
  class Demo implements Inter {
      public void print() {
          System.out.println("print");
      }
  }
  

```

###  接口的成员特点

- A:接口成员特点
  - 成员变量；只能是常量，并且是静态的。
    - 默认修饰符：public static final
      - 建议：自己手动给出。
  - 构造方法：接口没有构造方法。
  - 成员方法：只能是抽象方法。
    - 默认修饰符：public abstract
      - 建议：自己手动给出。
- B:案例演示
  - 接口成员特点

```java
  class Demo2Interface {
      public static void main(String[] args) {
          Demo d = new Demo();
          d.print();
          //System.out.println(Inter.num);
      }
  }
  
  interface Inter {
       public static final int num = 10;              //接口中所有的变量都是常量
  
       public abstract void print();                  //接口中所有的方法都是抽象的
  }
  
  class Demo implements Inter {
      public void print() {                           //重写接口中的方法,权限必须是public
          System.out.println("1111111111111");
      }
  }
  

```

###  类与类,类与接口,接口与接口的关系

- A:类与类,类与接口,接口与接口的关系
  - a:类与类：
    - 继承关系,只能单继承,可以多层继承。
  - b:类与接口：
    - 实现关系,可以单实现,也可以多实现。
    - 并且还可以在继承一个类的同时实现多个接口。
  - c:接口与接口：
    - 继承关系,可以单继承,也可以多继承。
- B:案例演示
  - 类与类,类与接口,接口与接口的关系

```java
  class Demo3Interface {
      public static void main(String[] args) {
          /*DemoE d = new DemoE();
          d.method();
          d.show();*/
  
          DemoG d = new DemoG();
      }
  }
  
  /*
  类与类,类与接口,接口与接口的关系
  */
  
  interface DemoA {
      public void show();
  } 
  
  interface DemoB {
      public void method();
  }
  
  abstract class DemoC {
      public abstract void print();
  }
  
  /*class DemoD extends DemoC {               //类与类之间是继承关系,只能单继承
  }*/
  
  /*class DemoE implements DemoA,DemoB {      //类与接口直接是实现关系,既可以单实现也可以多实现
      public void show() {
          System.out.println("show");
      }
  
      public void method() {
          System.out.println("method");
      }
  }*/
  
  interface DemoF extends DemoA,DemoB{        //接口和接口直接是继承关系,既可以单继承也可以多继承
  
  }
  
  class DemoG implements DemoF {
      public void show() {
          System.out.println("show");
      }
  
      public void method() {
          System.out.println("method");
      }
  }

```

###  抽象类和接口的区别(面试题)

- A:成员区别
  - 抽象类：
    - 成员变量：可以变量，也可以常量
    - 构造方法：有
    - 成员方法：可以抽象，也可以非抽象
  - 接口：
    - 成员变量：只可以常量
    - 成员方法：只可以抽象
- B:关系区别
  - 类与类
    - 继承，单继承
  - 类与接口
    - 实现，单实现，多实现
  - 接口与接口
    - 继承，单继承，多继承
- C:设计理念区别
  - 抽象类 被继承体现的是：”is a”的关系。抽象类中定义的是该继承体系的共性功能。
  - 接口 被实现体现的是：”like a”的关系。接口中定义的是该继承体系的扩展功能。

###  猫狗案例加入跳高功能分析及其代码实现

- A:案例演示
  - 动物类：姓名，年龄，吃饭，睡觉。
  - 猫和狗
  - 动物培训接口：跳高

```java
  class Demo5Interface {
      public static void main(String[] args) {
          Dog d = new Dog("八公",30);
          d.eat();
          d.sleep();
          d.jump();
  
          System.out.println(d.getName() + "," + d.getAge());
      }
  }
  
  /*
  * A:案例演示
      * 动物类：姓名，年龄，吃饭，睡觉。
      * 猫和狗
      * 动物培训接口：跳高
  */
  abstract class Animal {
      private String name;
      private int age;
  
      public Animal(){}
  
      public Animal(String name,int age) {
          this.name = name;
          this.age = age;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public void setAge(int age) {
          this.age = age;
      }
  
      public String getName() {
          return name;
      }
  
      public int getAge() {
          return age;
      }
  
      public abstract void eat();
  
      public abstract void sleep();
  }
  
  interface Jump {
      public void jump();
  }
  
  class Dog extends Animal implements Jump {
      public Dog(){}
  
      public Dog(String name,int age) {
          super(name,age);
      }
  
      public void eat() {
          System.out.println("吃肉");
      }
  
      public void sleep() {
          System.out.println("趴着睡");
      }
  
      public void jump() {
          System.out.println("跳高");
      }
  }
  
  
```