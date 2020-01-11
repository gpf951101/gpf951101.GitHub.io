---
layout: article
title: Java面试(1)——JDK、JRE、JVM的区别与联系
sharing: true
show_author_profile: true
lang: zh
key: Cyber20200109_1
comment: true
pageview: true
date: 2020-01-09
modify_date: 2020-01-11
tags: [Java, 面试]

---

### 一. Java中JDK、JRE和JVM之间有什么区别和联系？

<!--more-->

#### 1.定义

**JDK**：(Java Development Kit)是Java开发工具包，是整个Java的核心，包括Java运行环境**JRE**，Java工具和Java基础类库。
**JRE**：(Java Runtime Environment)是Java的运行环境，包括JVM标准实现及Java核心类库。
**JVM**：(Java Virtual Machine)是Java虚拟机，是整个Java实现跨平台的最核心部分，能够运行以Java语言写作的软件程序。

#### 2.关系

JDK包含JRE，同时也包含了编译Java源码的编译器javac，还包含很多Java程序调试、分析打包的工具，如jar、javadoc等。JRE包括JVM。
如果只需要运行Java程序，安装JRE就可以，如果要编写java程序，需要安装JDK。
**注**：但是如果想要使用JSP部署web应用程序，则需要使用JDK，因为应用程序服务器会吧JSP转化为Java Servlet，需要JDK编辑servlet。

JDK可以使用javac命令编译java文件，生成class文件，class文件就可以在虚拟机上运行。class并不直接与及其的操作系统相对应，而是由虚拟机间接与操作系统交互，由虚拟机将class文件对应的字节码解释给本地系统执行，所以Java可以跨平台是因为JVM。

Java官网图如下：
![Java](/images/20200110135900.png)

[链接](https://docs.oracle.com/javase/8/docs/index.html)

### 二. == 和 equals的区别是什么？

#### 1. == 解读

- 对于基本类型而言，比较的是值是否相同；
- 对于引用类型而言，比较的是引用是否相同。

```java

String x = "string";
String y = "string";
String z = new String("string");
System.out.println(x==y); // true
System.out.println(x==z); // false
System.out.println(x.equals(y)); // true
System.out.println(x.equals(z)); // true

```

x和y指向的是同一个引用，所以==判断是true。而new String方法重新开辟了内存空间，所以为false。equals比较的是值，所以结果都是true。

#### 2. equals 解读<div id="equals"></div>

equals 本质上就是 == ，只不过String等类重写了equals方法，把它变成了值比较。
对于Object类来说，equals()方法在对象上实现的是差别可能性最大的等价关系，即，对于任意非null的引用值x和y，当且仅当x和y引用的是同一个对象，该方法才会返回true。

equals有如下四个性质：
- **自反性**： 对于任意不为null的引用值x，x.equals(x)一定是true。
- **对称性**： 对于任意不为null的引用值x和y，当且仅当x.equals(y)是true时，y.equals(x)也是true。
- **传递性**： 对于任意不为null的引用值x、y和z，如果x.equals(y)是true，同时y.equals(z)是true，那么x.equals(z)一定是true。
- **一致性**： 对于任意不为null的引用值x和y，如果用于equals比较的对象信息没有被修改的话，多次调用时x.equals(y)要么一致地返回true要么一致地返回false。

equals源码：
```java

public boolean equals(Object obj) {
    return (this == obj);
}

```
String类中的equals方法：

```java
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
```
所以是String重写了equals方法。

#### 3. 总结

== 对于基本类型来说是值比较，对于引用类型来说是比较的是引用；而 equals 默认情况下是引用比较，只是很多类重新了 equals 方法，比如 String、Integer 等把它变成了值比较，所以一般情况下 equals 比较的是值是否相等。

### 三、 两个对象的hashCode()相同，则equals()也一定为true吗？

#### 1. equals解析

[equals解析](#equals)

#### 2. hashCode()解析

当equals()方法被override时，hashCode()也要被override。按照一般hashCode()方法的实现来说，相等的对象，它们的hash code一定相等

hashCode()方法给对象返回一个hash code值，这个方法被用于hash tables，如HashMap。
性质：
- 在一个Java应用的执行期间，如果一个对象提供给equals做比较的信息没有被修改的话，该对象多次调用hashCode()方法，该方法必须始终如一返回同一个integer。
- 如果两个对象根据equals(Object)方法是相等的，那么调用二者各自的hashCode()方法必须产生同一个integer结果。
- 并不要求根据equals(java.lang.Object)方法不相等的两个对象，调用二者各自的hashCode()方法必须产生不同的integer结果。即**equals不同，hashCode()可能相同**。然而，程序员应该意识到对于不同的对象产生不同的integer结果，有可能会提高hash table的性能。

String中的该方法：
```java
public int hashCode() {  
    int h = hash;  
    if (h == 0) {  
        int off = offset;  
        char val[] = value;  
        int len = count;  
  
        for (int i = 0; i < len; i++) {  
            h = 31 * h + val[off++];  
        }  
        hash = h;  
    }  
    return h;  
}  
```

#### 3. 总结
Java对象的eqauls方法和hashCode方法是这样规定的：
- 相等（相同）的对象必须具有相等的哈希码（或者散列码）。
- 如果两个对象的hashCode相同，它们并不一定相同。
- equals()相等的两个对象，hashcode()一定相等；
- equals()不相等的两个对象，却并不能证明他们的hashcode()不相等。

### 四、 final在java中有什么作用

1. final 修饰的类叫最终类，该类不能被继承。
2. final 修饰的方法不能被重写。
3. final 修饰的变量叫常量，常量必须初始化，初始化之后值就不能被修改。


### 五、 Java中Math.round(-1.5)等于多少

round向上取整，是在**原值**的基础上，而不是绝对值，所以是-1.

### 六、 Java的基本数据类型

基本数据类型共有八中：
- **byte**：8位，最大存储数据量是255，存放的数据范围是-128~127之间。
- **short**：16位，最大数据存储量是65536，数据范围是-32768~32767之间。
- **int**：32位，最大数据存储容量是2的32次方减1，数据范围是负的2的31次方到正的2的31次方减1。
- **long**：64位，最大数据存储容量是2的64次方减1，数据范围为负的2的63次方到正的2的63次方减1。
- **float**：32位，数据范围在3.4e-45~1.4e38，直接赋值时必须在数字后加上f或F。
- **double**：64位，数据范围在4.9e-324~1.8e308，赋值时可以加d或D也可以不加。
- **boolean**：只有true和false两个取值。
- **char**：16位，存储Unicode码，用单引号赋值。

**注：**String不是基本类型，String是类，声明的是对象。

long最后需要加`L`, float需要加`F`.
小数默认不加符号，默认是double类型。

### 七、 Java中操作字符串的都有哪些类

共有三种：`String`, `StringBuffer`, `StringBuilder`。

区别：
1. String声明的是不可变的对象，每次操作都会生成新的String对象，然后将指针指向新的String对象，而StringBuilder和StringBuffer可以在原有对象的基础上进行操作，所以在经常改变字符串内容的情况下最好不要使用String。
2. StringBuilder是非线程安全的，StringBuffer是线程安全的。(有buff加成是线程安全的)。但是StringBuilder的性能却高于StringBuffer，所以在单线程环境下推荐使用StringBuilder，多线程推荐使用StringBuffer。

### 八、 String str="i"与 String str=new String("i")

不一样。前者，JVM会将其分配到常量池中，后者会分配到堆内存中。此处涉及Java虚拟机的知识，以后讨论。

### 九、 String 类的常用方法都有那些
- indexOf()：返回指定字符的索引。
- charAt()：返回指定索引处的字符。
- replace()：字符串替换。
- trim()：去除字符串两端空白。
- split()：分割字符串，返回一个分割后的字符串数组。
- getBytes()：返回字符串的 byte 类型数组。
- length()：返回字符串长度。
- toLowerCase()：将字符串转成小写字母。
- toUpperCase()：将字符串转成大写字符。
- substring()：截取字符串。
- equals()：字符串比较

### 十、 抽象类和抽象方法

抽象类中可以声明一般方法和抽象方法，但是包含抽象方法的类一定是抽象类。
普通类不能包含抽象方法。
抽象类不能直接实例化。
抽象类不能用final修饰，因为定义抽象类就是为了让其他类继承的，用final修饰之后就不能被继承。

### 十一、 接口和抽象类的区别

1. 实现：抽象类的子类用extends来继承，接口必须使用implements老实现接口。
2. 构造函数： 抽象类可以有构造函数，接口不能有。
3. main方法： 抽象类可以有main方法，且可运行，接口不能有main方法。
4. 实现数量：类可以实现多个接口，但是只能继承一个抽象类。
5. 访问修饰符： 接口中的方法默认的使用public修饰，抽象类中的方法可以使任意修饰符。

### 十二、 Java中IO流分为几种
按照功能：输入流（input）、输出流（output）。
按照类型：字节流和字符流。
- 字节流和字符流的区别是：
    - 字节流按 8 位传输以字节为单位输入输出数据
    - 字符流按 16 位传输以字符为单位输入输出数据。

### 十三、 BIO、NIO、AIO之间的区别

- BIO：Block IO 同步阻塞式 IO，就是我们平常使用的传统 IO，它的特点是模式简单使用方便，并发处理能力低。
- NIO：New IO 同步非阻塞 IO，是传统 IO 的升级，客户端和服务器端通过 Channel（通道）通讯，实现了多路复用。
- AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO ，异步 IO 的操作基于事件和回调机制。

### 十四、 Files的常用方法

- Files.exists()：检测文件路径是否存在。
- Files.createFile()：创建文件。
- Files.createDirectory()：创建文件夹。
- Files.delete()：删除一个文件或目录。
- Files.copy()：复制文件。
- Files.move()：移动文件。
- Files.size()：查看文件个数。
- Files.read()：读取文件。
- Files.write()：写入文件。


### 参考链接：
[第一部分](https://mp.weixin.qq.com/s/IBbD3CmVWsTL9ymHg6gGGA)
[JDK](https://www.cnblogs.com/java-lzx/p/11641610.html)
[hashCode](https://www.cnblogs.com/qian123/p/5703507.html)