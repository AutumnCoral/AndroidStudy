[TOC]

# **`书籍：`**

Java核心技术 卷I，

# Java基础语法

java基础学习法，这个阶段我就不需要看视频了，可以看遍java基础的书,把书上的代码全部敲一遍

构造器:用来构造并初始化对象

toString:返回字符串的描述

## 字符串

### 常见方法

1. **substring:截取字符串**

```
public class Main {
   public static void main(String[] args) {
      String a  = "HelloWorld!";
      System.out.println(a.substring(0, 3)); //输出Hel
            System.out.println(a.substring(3)); //输出loWorld!
   }
```

**注释：**

- substring**方法的第二个参数是不想复制的第一个位置。这里要复制位置为0、1和2（从0到2，包括0和2）的字符。在substring中从0开始计数，直到3为止，但不包含3。

- substring可以计算长度，字符串s.substring(a, b)的长度为b-a。例如，子串“Hel”的长度为3-0=3。

  2. **拼接**

     String a  = "HelloWorld!"+2；

  3. **不可变字符串**

     String类中没有提供可以进行修改字符串的方法

     方式：可以先截取在拼接

      System.out.println(a.substring(0, 3)+”loWorld!“) ;// 输出HelloWorld

     参考： **[JavaGuide](https://github.com/Snailclimb/JavaGuide)**

###   1，String StringBuffer 和 StringBuilder 的区别是什么? String 为什么是不可变的?

- **可变性**

 String 类中使用 final 关键字修饰字符数组来保存字符串，`private final char value[]`，所以 String 对象是不可变的。

> 补充（来自[issue 675](https://github.com/Snailclimb/JavaGuide/issues/675)）：在 Java 9 之后，String 类的实现改用 byte 数组存储字符串 `private final byte[] value`

而 StringBuilder 与 StringBuffer 都继承自 AbstractStringBuilder 类，在 AbstractStringBuilder 中也是使用字符数组保存字符串`char[]value` 但是没有用 final 关键字修饰，所以这两种对象都是可变的。

StringBuilder 与 StringBuffer 的构造方法都是调用父类构造方法也就是 AbstractStringBuilder 实现的，大家可以自行查阅源码。

`AbstractStringBuilder.java`

```java
abstract class AbstractStringBuilder implements Appendable, CharSequence {
    /**
     * The value is used for character storage.
     */
    char[] value;

    /**
     * The count is the number of characters used.
     */
    int count;

    AbstractStringBuilder(int capacity) {
        value = new char[capacity];
    }
```

- **线程安全性**

String 中的对象是不可变的，也就可以理解为常量，线程安全。AbstractStringBuilder 是 StringBuilder 与 StringBuffer 的公共父类，定义了一些字符串的基本操作，如 expandCapacity、append、insert、indexOf 等公共方法。StringBuffer 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。StringBuilder 并没有对方法进行加同步锁，所以是非线程安全的。

- **性能**


     每次对 String 类型进行改变的时候，都会生成一个新的 String 对象，然后将指针指向新的 String 对象。StringBuffer 每次都会对 StringBuffer 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用 StringBuilder 相比使用 StringBuffer 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。
    
     **对于三者使用的总结：**
    
     1. 操作少量的数据: 适用 String
     2. 单线程操作字符串缓冲区下操作大量数据: 适用 StringBuilder
     3. 多线程操作字符串缓冲区下操作大量数据: 适用 StringBuffer

# java面向对象

## 1，java面向对象

### 1. 面向对象和面向过程的区别

面向对象和面向对象都是软件开发的思想，先有了过程再有了思想

- 编程思路不同： 面向过程以实现功能的函数开发为主，而面向对象要首先抽象出

类、属性及其方法，然后通过实例化类、执行方法来完成功能。

- 封装性：都具有封装性，但是面向过程是封装的是功能，而面向对象封装的是数据

和功能。

- 面向对象具有继承性和多态性，而面向过程没有继承性和多态性，所以面向对象优

势是明显

## 2，java封装

## 3，java继承

## 4，java单例模式

## 5，java多态