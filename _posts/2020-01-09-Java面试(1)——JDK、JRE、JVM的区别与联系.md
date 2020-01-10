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
# modify_date: 
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

## 二. == 和 equals的区别是什么？

### 1. == 解读

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

### 2. equals 解读

equals 本质上就是 == ，只不过String等类重写了equals方法，把它变成了值比较。

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

### 3. 总结

== 对于基本类型来说是值比较，对于引用类型来说是比较的是引用；而 equals 默认情况下是引用比较，只是很多类重新了 equals 方法，比如 String、Integer 等把它变成了值比较，所以一般情况下 equals 比较的是值是否相等。

