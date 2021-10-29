[TOC]



参考1：[kotlin官方中文文档](http://www.kotlincn.net/docs/reference/coroutines/coroutines-guide.html)

参考2：第一行代码第三版

参考3：[菜鸟教程](https://www.runoob.com/kotlin/kotlin-tutorial.html)

- [Kotlin官网文档](https://kotlinlang.org/docs/reference/)
- [kotlin中文官网文档](http://www.kotlincn.net/docs/reference/)
- [Kotlin在线IDE](https://try.kotlinlang.org/)
- [Kotlin On Github](https://github.com/JetBrains/kotlin)

# Kotlin

# 基础语法

Kotlin 文件以 .kt 为后缀

##  包声明

代码文件的开头一般为包的声明：

```
package com.example.kotlindemoone
fun main() {
    val  a = 10
    println("a = "+a)
}
```

**注意：**

-  **打印日志尽量不要使用println()，而是应该使用Log，为什么这里却还是使用了println()呢？这是因为Log是Android中提供的日志工具类**，而我们现在是独立运行的Kotlin代码，和Android无关，所以自然是无法使用Log的
- **Kotlin每一行代码的结尾是不用加分号的**

##  变量和常量

###  变量

**Kotlin的变量没有默认值，java有默认值**，kotlin是静态语言

**参考上面的代码**

参数格式为：参数 : 类型                                比如：val  a:Int = 10

<img src="../media/pictures/KotlinBase.assets/Kotlin_变量.png" alt="image-20210910135828024" style="zoom:50%;" />

在Java中如果想要定义一个变量，需要在变量前面声明这个变量的类型，比如说int a表示a是一个整型变量，String b表示b是一个字符串变量。而Kotlin中定义一个变量，**只允许在变量前声明两种关键字：val和var。**关键字 <标识符> : <类型> = <初始化值>**

- val（value的简写）用来声明一个**不可变的变量**，这种变量在初始赋值之后就再也不能重新赋值，对应Java中的final变量。

- var（variable的简写）用来声明一个**可变的变量**，这种变量在初始赋值之后仍然可以再被重新赋值，对应Java中的非final变量。

但是Kotlin的类型推导机制并不总是可以正常工作的，比如说如果我们对一个变量延迟赋值的话，Kotlin就无法自动推导它的类型了。这时候就需要显式地声明变量类型才行，Kotlin提供了对这一功能的支持，语法如下所示

var a:Int= 10

### 常量和静态方法

[学习](https://www.jianshu.com/p/e8752c880088)

编译时常量

>- 只读变量并非绝对只读。
>- **编译时常量只能在函数之外（包含main方法）定义**，因为编译时常量必须在编译时赋值，而函数都
>  是在运行时才调用，函数内的变量也是在运行时赋值，编译时常量要在这些变量赋值前就已存在。
>- 编译时常量只能是常见的基本数据类型:String.Int、Double、Float、Long、
>  Short、Byte、Char、Boolean。

`val`关键字前面加上`const`关键字

```
const val NUM_A = 6
```

其特点：**`const`只能修饰`val`，不能修饰`var`**

1. 在顶层声明

2. 在`object`修饰的类中声明，在`kotlin`中称为**对象声明**，它相当于`Java`中一种形式的单例类

3. 在伴生对象中声明

   ```
   // 在顶层声明
    const val TEXT_NUM1 ="在顶层声明的常量" 
   fun main(args: Array<String>) {
       println("顶层声明：${TEXT_NUM1}")
       println("在object修饰的类中：${TestConst.TEXT_NUM2}")
       println("伴生对象中声明：${TestClass.TEXT_NUM3}")
   
   }
   object TestConst{
       const val TEXT_NUM2 ="在object修饰的类中"
   }
   class TestClass{
       /*在这里也涉及到了一个子知识点companion object，需要进行学习下，与object的区别又啥呢
         companion object 修饰为伴生对象,伴生对象在类中只能存在一个，类似于java中的静态方法 Java 中使用类访问静态成员，静态方法。
        */
       companion object {
           const val TEXT_NUM3 ="伴生对象中声明"
       }
   }
   
   输出结果：顶层声明：在顶层声明的常量
           在object修饰的类中：在object修饰的类中
           伴生对象中声明：伴生对象中声明
   ```

## 关键字

### open

在学习open的时候，可以在学习下java中的final在进行学习下

- 在 Java 开发中默认可以被继承的类不需要添加 final 关键字，如需不想被继承例如 String 类添加 final 修饰类。

- **在**Kotlin开发中类和方法默认不允许被继承和重写，等同于Java中用 final 修饰类和方法。
  如果在Kotlin 中类和方法想被继承和重写，需添加open 关键字修饰。

- ```
  open class Person
  open class Student : Person() 
  ```

## 数据类型



>Java有两种数据类型:引用类型与基本数据类型。
>Kotlin只提供引用类型这一种数据类型，出于更高性能的需要，Kotlin编译器会在Java字节码中改用基本数据类型。
>
>

### 基本数据类型

#### 基本数据类型的分类

在java中数据类型分为基本基本数据类型基和引用数据类型

- 在Kotlin中变量的首字母都是大写，注意java都是小写，因此，Kotlin都是对象数据类型，而java是基本数据类型

  | java基本数据类型 | Kotlin对象数据类型 | 数据类型说明 |
  | ---------------- | ------------------ | ------------ |
  | int              | Int                | 整型         |
  | long             | Long               | 长整型       |
  | short            | Short              | 短整型       |
  | float            | Float              | 单经度浮点   |
  | double           | Double             | 双经度浮点   |
  | boolean          | Boolean            | 布尔型       |
  | char             | Char               | 字符型       |
  | byte             | Byte               | 字节型       |

  - 常量与变量都可以没有初始化值,但是在引用前必须初始化，编译器支持自动类型判断,即声明时可以不指定类型,由编译器判断

    ```
    val a: Int = 1
    val b = 1       // 系统自动推断变量类型为Int
    val c: Int      // 如果不在声明时初始化则必须提供变量类型
    c = 1           // 明确赋值
    
    
    var x = 5        // 系统自动推断变量类型为Int
    x += 1           // 变量可修改
    ```

#### 比较两个数字大小

- 在 Kotlin 中，三个等号 === 表示比较对象地址，两个 == 表示比较两个值大小

- 在java中**== 比**较的是变量(栈)内存中存放的对象的(堆)内存地址，用来判断两个对象的地址是否相同，即是否是指相同一个对象。比较的是真正意义上的指针操作。**equals**用来比较的是两个对象的内容是否相等，由于所有的类都是继承自java.lang.Object类的，所以适用于所有对象，如果没有对该方法进行覆盖的话，调用的仍然是Object类中的方法，而Object中的equals方法返回的却是==的判断。

#### 类型转换

由于不同的表示方式，较小类型并不是较大类型的子类型，较小的类型不能隐式转换为较大的类型。 这意味着在不进行显式转换的情况下我们不能把 Byte 型值赋给一个 Int 变量。

每种数据类型都有下面的这些方法，可以转化为其它的类型：

```
toByte(): Byte
toShort(): Short
toInt(): Int
toLong(): Long
toFloat(): Float
toDouble(): Double
toChar(): Char
```

###  类型推断

```
 //var str:String ="Hello word"
  var str ="Hello word" //类型推断：对于已经声明并且赋值的变量，他允许你省略定义的类型
```



### 比较值的大小

在 Kotlin 中，三个等号 === 表示比较对象地址，两个 == 表示比较两个值大小。（==相当于java中的equal）

```kotlin
// TODO 比较两个值
fun main() {

    val name1: String = "张三"
    val name2: String = "张三"
    // --- 比较值本身
    // == 等价 Java的equals
    println(name1.equals(name2))
    println(name1 == name2) //kotlin推荐这种写法


    // ---  比较对象地址
    val test1:Int? =  10000
    val test2:Int? =  10000
    println(test1 === test2) // false
}
```

### **类型转换**

#### 显示转换

```
toByte(): Byte
toShort(): Short
toInt(): Int
toLong(): Long
toFloat(): Float
toDouble(): Double
toChar(): Char
```

### **位运算符**

- ```
  Kotlin中对于按位操作，和Java是有很大的差别的。
  ```

  ```
  Kotlin中没有特殊的字符，但是只能命名为可以以中缀形式调用的函数，下列是按位操作的完整列表(仅适用于整形（Int）和长整形（Long）)：
  ```

  1. `shl(bits)` => 有符号向左移 (类似`Java`的`<<`)
  2. `shr(bits)` => 有符号向右移 (类似`Java`的`>>`)
  3. `ushr(bits)` => 无符号向右移 (类似`Java`的`>>>`)
  4. `and(bits)` => 位运算符 `and` (同`Java`中的按位与)
  5. `or(bits)` => 位运算符 `or` (同`Java`中的按位或)
  6. `xor(bits)` => 位运算符 `xor` (同`Java`中的按位异或)
  7. `inv()` => 位运算符 按位取反 (同`Java`中的按位取反)

### 字符转义

- `\t` => 表示制表符
- `\n` => 表示换行符
- `\b` => 表示退格键（键盘上的Back建）
- `\r` => 表示键盘上的`Enter`键
- `\\` => 表示反斜杠
- `\'` => 表示单引号
- `\"` => 表示双引号
- `\$` => 表示美元符号，如果不转义在`kotlin`中就表示变量的引用了
- 其他的任何字符请使用Unicode转义序列语法。例：'\uFF00'

## 类型检测

### 运算符

#### is，!is，as，as?运算符

1. **is，!is**

-   在kotlin中通过is来进行类型的检测，**相当于java中instanceof**

- **is运算符可以检查对象A是否是特定的类型，还可以检查一个对象是否属于某种数据类型（Int、String等）**。

- Kotlin中我们可以在运行时通过 is 或者 !is 来检查对象是否符合所需的类型

- ```
  fun main(args: Array<String>) {
      println("你好" is String)// 输出：true
      println("你好" !is String)// 输出：false
  
  }
  ```

1. **as运算符和as?运算符**

   **as运算符用于执行引用类型的显式类型转换**。如果要转换的类型与指定的类型兼容，转换就会成功进行；如果类型不兼容，使用as?运算符就会返回值null。在Kotlin中，父类是禁止转换为子类型的。

   ```
   fun main(args: Array<String>) {
       val student =Student()
       val person = Person()
       println(student as Person ) // 输出Student@266474c2
       println(person as Student )// 报错 :Exception in thread "main" java.lang.ClassCastException: Person cannot be
       cast to Student
       println((person as? Student))// 输出：null
   }
   open class Person
   open class Student : Person() 
   
   ```

```csharp
JAVA

if(view instanceof TextView) {
    ((TextView) view).setText("text");
}

Kotlin

if(view is TextView) {
    (view as TextView).setText("text")
}
```

#### 全调用符（?.）

专门用于调用可空类型变量中的成员方法或属性，其语法格式为“变量?.成员”。其作用是判断变量是否为null，如果不为null才调用变量的成员方法或者属性。

#### Elvis操作符（?:）

在使用安全调用符调用可空变量中的成员方法或属性时，如果当前变量为空，则会返回一个null值，但有时即使当前变量为null，也不想返回一个null值而是指定一个默认值，此时该如何处理呢？为了解决这样的问题，Kotlin中提供了一个Elvis操作符（?:），通过Elvis操作符（?:）可以指定可空变量为null时，调用该变量中的成员方法或属性的返回值，其语法格式为“表达式?:表达式”。

####  非空断言（!!.）

除了通过使用安全调用符（?.）来使用可空类型的变量之外，还可以通过非空断言（!!.）来调用可空类型变量的成员方法或属性。使用非空断言时，调用变量成员方法或属性的语法结构为“变量!!.成员”。非空断言（!!.）会将任何变量（可空类型变量或者非空类型变量）转换为非空类型的变量，若该变量为空则抛出异常

## 容器

容器：存放数据的载体，容器分为数组和集合

### 数组

#### 数组定义

在学习的时候可以对比着java进行学习

数组：初始化时候指定容器的大小，不可以动态的调整大小。元素安顺序存储在一连串的内存段上

#### kotlin 数组的创建

**声明对象数组的三种形式：**

(1)使用arrayOf函数和指定的数组元素创建数组，必须指定数组的元素，可以为任意类型

```
//Java写法:
String[] params1 = {"str1", "str2", "str3"};
//kotlin写法:`
val params1 = arrayOf("str1", "str2", "str3")
```

(2).使用arrayOfNulls函数创建一个指定大小的并初始化每个元素为null的数组，但是必须指定元素的类型

```
//Java写法:
String[] params2 = new String[12];

//kotlin写法:
val params2 = arrayOfNulls<String>(12)
```

(3)Array构造方法指定数组大小和一个生成元素的lambda表达式

这种方法创建的数组，其中的每个元素都是非空的

```
//kotlin写法:
val params3 = Array<String>(3){i -> "str" + i }
// 也可以这么写
val params=Array(3){"str$it"}
```

#### 基本数据类型数组

- [] 运算符代表调用成员函数 get() 和 set()。数组是不可变的

- 除了类Array，还有ByteArray, ShortArray, IntArray，用来表示各个类型的数组，省去了装箱操作，因此效率更高，其用法同Array一样：

  ```
   val x: IntArray = intArrayOf(1, 2, 3)
   x[0] = x[1] + x[2]
   println(x[0])  //输出5
   
    val inArray = IntArray(5)
    inArray[0]= 2
    // 创建一个长度为5，值全部为100的整型数组intArray[100,100,100,100,100]
    val intArray2 = IntArray(5){100}
    //这里it是它的索引值，所以创建一个长度为5的intArray[0,2.4,8,16],it是lamble是专有表达式，这里代表数组的下标
    val intArray3 = IntArray(5){it*2 } //{i -> i*2}
  ```

|          | kotlin        | java        |
| -------- | ------------- | ----------- |
| 整型     | IntArray      | int[]       |
| 整型装箱 | Array<Int>    | Integer[]   |
| 字符     | CharArray     | char[]      |
| 字符装箱 | Array<Char>   | Character[] |
| 字符串   | Array<String> | String[]    |
|          |               |             |

#### 数组的遍历

**java遍历数组**

```
/*
java遍历数组的方法：fori,forEach,
 forEach：遍历数组中的每个元素，且不需要下标
 */
for (int forEach : a) {
   System.out.println("forEach=" + forEach);
}
for (int i = 0 ; i< a.length;i++){
   System.out.println("for遍历:"+a[i]);
}
```

**kotlin遍历数组**

方法一：for-in

```
val numbers2 = Array(10, { value: Int -> (value + 200) })
// 数组for循环遍历 for-in
for (value in numbers2) {
    println(value) //输出200 201 202 203 204 205 206 207 208 209
}
// for-in的加强版： 根据下标在取出数组中的元素
    for (i:Int in numbers2.indices){
    println(i.toString()+"->"+numbers2[i])//输出结果0->200 1->201 2->202 3->203 4->204 5->205 6->206 7->207 8->208 9->209
    }
```

方法2：forEach

```
numbers2.forEach { 
   println("forEach:$it") 
}
   // forEach加强版，同时遍历下标和元素 
    numbers2.forEachIndexed{index, i ->
        println("$index:$i")
    }
```



#### 数组的替换值

数组用类 Array 实现，并且还有一个 size 属性及 get 和 set 方法，由于使用 [] 重载了 get 和 set 方法，所以我们可以通过下标很方便的获取或者设置数组对应位置的值

#### 字符数组转换成字符串

    fun main() {
        // 第一种形式建立数组arrayOf
        val numbers = arrayOf(1, 2, 3, 4, 5, 6, 7, 8)
         println(numbers[0]) //输出1
         println(numbers[7]) //输出8
         // 数组遍历
        for (number in numbers) { 
             println(number) // 输出1 2 3 4 5 6 7 8
        }
    // 第二种形式 Array value=0     长度为10，然后根据value+200循环赋值
    val numbers2 = Array(10,  {value: Int -> (value + 200) })
    for (value in numbers2) {
        println(value) //输出200 201 202 203 204 205 206 207 208 209
    
    }
}

### 集合

#### 集合的分类



[参考学习1](https://www.jianshu.com/p/fa5abe312269)

[参考学习2](https://www.songyubao.com/book/primary/kotlin/kotlin-data-collection.html)

与数组不同的是集合的大小可以任意改变	，按照类型分类

- list:是一个有序列表，可以通过索引(下标)访问列表。元素可以在列表中出现多次，可以重复
- set:是唯一的元素集合，一组无重复数据的对象，一般来说set中元素的顺序并不重要
- map：（字典）键值对，键是唯一的，每个键刚好映射到一个值，值可以重复

按照可变性分类

- 可变集合
- 不可变集合

具体来说
 对于List

- List ——声明不可变List集合
- MutableList——声明可变List集合

对于Map

- Map——声明不可变Map集合
- MutableMap——声明可变Map集合

对于Set

- Set——声明不可变Set集合
- MutableSet——声明可变Set集合

除此之外还有四个基本接口

- Iterable ——所有集合的父类
- MutableIterable —— 继承于Iterabl接口，支持遍历的同时可以执行删除操作
- Collection —— 继承于Iterable接口，仅封装了对集合的只读方法
- MutableCollection ——继承于Iterable,Collection，封装了添加或移除集合中元素的方法

| 类型 | 数组创建方式                                           | 示例                                                         | 说明                                        | 是否可变 |
| ---- | ------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------- | -------- |
| list | arrayListOf<T>() mutableListOf<T> 相同元素类型的队列   | val array = arrayListOf<Int>(1, 2, 3) val array = mutableListOf<String>() | - 必须指定元素类型                          | 可变     |
|      | listOf<T>() 相同元素类型的集合                         | val array = listOf<Int>(1, 2, 3)                             | - 必须指定元素类型 - 必须指定初始化数据元素 | 不可变   |
| map  | arrayMapOf<K,V>() mutableMapOf<K,V> 相同元素类型的字典 | val array= arrayMapOf(Pair("key","value")) val array= mutableMapOf() | - 初始元素使用Pair包装                      | 可变     |
|      | mapOf<T>() 相同元素类型的字典                          | val array= mapOf(Pair("key","value"))                        | - 元素使用Pair包装 - 必须指定初始元素       | 不可变   |
| set  | arraySetOf<T>() mutableSetOf<T> 相同元素类型的集合     | val array= arraySetOf<TInt>(1,2,3) val array= mutableSetOf<Int>() | - 会对元素自动去重                          | 可变     |
|      | setOf<T>() 相同元素类型的集合                          | val array= arraySetOf<Int>(1,2,3)                            | - 对元素自动去重 - 必须指定元素类型。       | 不可变   |

##### list

**可变**

```
列表的创建---可变列表，必须指定元素类型
val arrayString = mutableListOf<String>()
arrayString.add("1")
arrayString.add("2")
arrayString.add("3")
arrayString.add(3, "4")
val arrayString2 = mutableListOf<String>("1", "2", "3", "4")
val iterator = arrayString2.iterator()
iterator.forEach { println("it:${it}") }// forEach循环遍历list中元素
println("add-index:${arrayString2.add(2, "33")}") // 指定位置插入元素
println("add:${arrayString2.add("34")}") //不指定位置插入元素，输出结果true
println("removeAt:${arrayString2.removeAt(3)}") //移除指定位置的元素
println("clear:${arrayString2.clear()}")// 集合中的元素会被清除
arrayString2.forEach {
    println("reverse:${arrayString2.reverse()}")//集合元素翻转  
}
arrayString2.sort()//排序。从小到大进行排序
```

**不可变**

```
//列表的创建---不可变列表，必须指定元素类型，必须指定初始化数据元素
val arrayInt = listOf<Int>(1, 2, 3, 4)
```

##### map

**可变**

```
//map--可变，使用Pair指定集合中初始化的元素
val arrayMap3 = mutableMapOf<String, String>()
arrayMap3["1"] = "1"
arrayMap3["2"] = "2"
arrayMap3["3"] = "3"
```

**不可变**

```
// map---不可变字典，不可动态添加，删除元素
val arrayMap = mapOf(Pair("key", "value"))
val arrayMap2 = mapOf<String, String>()
```

##### set

**可变**

```
// set:可变集合
val set = mutableSetOf<String>()
set.add("1")
set.add("2")
set.add("3")
set.add("4")
for (item in set) {
    println(item)
}
val set2 = mutableSetOf<String>("1", "2", "4")
print("isEmpty:${set2.isEmpty()}")//isEmpty
print("contain:${set2.contains("6")}")//contains
```

**不可变**

```
//set:不可变集合,元素唯一
val set3 = setOf<String>()
```

#### 集合的操作

add:添加元素确实改变了集合就返回true，如果集合没有发生变化就返回false。

![集合的操作](../media/pictures/KotlinBase.assets/集合的操作.png)

## 字符串

和 Java 一样，String 是不可变的。方括号 **[] 语法可以很方便的获取字符串中的某个字符**，也可以通过 for 循环来遍历：

```
val  str:String = "Hello Word"
 println(str[0]) //输出H(一个字符)
    for (c in str) {  // 可以用 for 循环迭代字符串
        println(c)  //输出 H e l l o W o r d（所有字符）
    }
```

- [更多字符串的操作请参考下面博主的连接](https://juejin.cn/post/6844903613869883405)

- Kotlin 支持三个引号 """ 扩起来的字符串，支持多行字符串

  ```
  fun main(args: Array<String>) {
      val text = """
      多行字符串
      多行字符串
      """.trimMargin()
      println(text)   // 输出有一些前置空格
  }
  ```

- String 可以通过 trimMargin() 方法来删除多余的空白。

###  字符串模板

- $ 表示一个变量名或者变量值
- $varName 表示变量值
- ${varName.fun()} 表示变量的方法返回值

字符串可以包含模板表达式 ，即一些小段代码，会求值并把结果合并到字符串中。 **模板表达式以美元符（$）开头，由一个简单的名字构成:**

```
fun main(args: Array<String>) {
    val a = 3
    val s = "a = $a" // 求值结果为 "a = 3"
    println(s)
}
```

或者用花括号扩起来的任意表达式:

```
fun main(args: Array<String>) {
    val s = "runoob"
    val str = "$s.length is ${s.length}" // 求值结果为 "runoob.length is 6"
    println(str)
}

```

### 字符串拼接

字符串的拼接可以使用+

### 字符串的转义

**字符串字面量**

> 在`Kotlin`中， 字符串字面量有两种类型：
>
> - 包含转义字符的字符串 转义包括（`\t`、`\n`等）,不包含转义字符串的也同属此类型
> - 包含任意字符的字符串 由三重引号（`""" .... """`）表示

## 注释

kotlin中的注释几乎和java中的没啥区别，唯一区别就是在kotlin的多行注释可以嵌套多行注释，这在java中是不可以的

```
/**
 * 第一个多行注释
 *
 * /**
 * 第二个多行注释
 * */
 */
```

# 条件控制结构

Kotlin中的条件语句主要有两种实现方式：if和when。

## if条件语句

- Kotlin中的if用法和Java中是几乎完全一样的，但是还是有不同点的

- Kotlin中的if语句相比于Java有一个额外的功能**，它是可以有返回值的**，返回值就是if语句每一个条件中最后一行代码的返回值

```
//方式1
fun Maxlength(num1: Int, num2: Int): Int {
    return if (num1 > num2) {
        num1
    } else {
        num2
    }
//方式2
    fun Maxlength(num1: Int, num2: Int): Int = if (num1 > num2)num1 else num2
}
```

- 使用区间 in  : 使用 in 运算符来检测某个数字是否在指定区间内，区间格式为 **x..y**

- ```
  fun main(args: Array<String>) {
      val x = 5
      val y = 9
      if (x in 1..8) {
          println("x 在区间内")  //输出x 在区间内
      }
  }
  ```

## when条件语句

Kotlin中的when语句有点类似于Java中的switch语句，但它又远比switch语句强大得多。

```
fun getScore(name: String) = when (name) {
    "A" -> 68
    "B" -> 34
    else -> 0
}
```

注意：在 when 中，else 同 switch 的 default。如果其他分支都不满足条件将会求值 else 分支。

​     when语句允许传入一个任意类型的参数，然后可以在when的结构体中定义一系列的条件，格式是：

匹配值->{执行逻辑}  当你的执行逻辑只有一行代码时，{ }可以省略

我们也可以检测一个值在（in）或者不在（!in）一个区间或者集合中：

```
when (x) {
    in 1..10 -> print("x is in the range")
    in validNumbers -> print("x is valid")
    !in 10..20 -> print("x is outside the range")
    else -> print("none of the above")
}
```

**另一种可能性是检测一个值是（is）或者不是（!is）一个特定类型的值。注意： 由于智能转换，你可以访问该类型的方法和属性而无需 任何额外的检测。**

```
fun hasPrefix(x: Any) = when(x) {
    is String -> x.startsWith("prefix")
    else -> false
}
```

when 也可以用来取代 if-else if链。 如果不提供参数，所有的分支条件都是简单的布尔表达式，而当一个分支的条件为真时则执行该分支：

```
when {
    x.isOdd() -> print("x is odd")
    x.isEven() -> print("x is even")
    else -> print("x is funny")
}
```

# 循环语句

Java中主要有两种循环语句：while循环和for循环。而Kotlin也提供了while循环和for循环	

Kotlin在for循环方面做了很大幅度的修改，Java中最常用的for-i循环在Kotlin中直接被舍弃了，而Java中另一种for-each循环则被Kotlin进行了大幅度的加强，变成了for-in循环，所以我们只需要学习for-in循环的用法就可以了

## 区间--range表达式

**in是闭区间**，until可以取消闭区间

```
 for (i in 0..5){ //相当于for(int i; i<=5;i++)
        println(i)
    }
    输出值：0 1 2 3 4 5 
```

解析：

- ..是创建两端闭区间的关键字，在..的两边指定区间的左右端点就可以创建一个区间了，相当于[0  ,5]

- for-in循环最简单的用法了，我们遍历了区间中的每一个元素

- **Kotlin中可以使用until关键字来创建一个左闭右开的区间（**默认情况下，for-in循环每次执行循环时会在区间范围内递增1）

  ```
  for (i in 0 until 5){
      println(i) 
  }
  输出：0 1 2 3 4
  ```

- 跳过一个元素step（）

  ```
  for (i in 0 until 10 step 2){
      println(i)
  }
  输出 0 2 4 6 8
  ```

- downTo 

  ```
  for (i in 0  downTo 1){
      println(i)
  }
  输出：10 9 8 7 6 5 4 3 2 1 0
  ```

# 面向对象编程

## 类和对象

### 类

File通常是用于编写Kotlin顶层函数和扩展函数

如何在Android studio 中建立Kolin的类

<img src="../media/pictures/Kotlin.assets/image-20210618153642370.png" alt="image-20210618153642370" style="zoom:67%;" />

<img src="../media/pictures/Kotlin.assets/image-20210618153713668.png" alt="image-20210618153713668" style="zoom: 67%;" />



#### 类定义

**Kotlin中也是使用class关键字来声明一个类的** ，

```
class Person {.....}
```

#### 类的组成

Kotlin 类可以包含：构造函数和初始化代码块、函数、属性、内部类、对象声明

- 在后面的文章说明汇总对于构造函数已经进行了说明，请查看1.1.4
- 属性

#### 类的属性

类的属性可以用关键字 **var** 声明为可变的，否则使用只读关键字 **val** 声明为不可变。

```
class Person {
   var name = ""
   var age = 0
}
```

我们可以像使用普通函数那样使用构造函数创建类实例：

```
var p = Person();//Kotlin 中没有 new 关键字
```

要使用一个属性，只要用名称引用它即可

```
p.age = 19
p.name = "WDD"
```

####  构造函数

**Koltin 中的类可以有一个 主构造器，以及一个或多个次构造器，主构造器是类头部的一部分**，位于类名称之后:

- 主构造函数

```
class Person constructor(age: Int) {}
--------等价于-------
/*
     因为是默认的可见性修饰符且不存在任何的注释符
     故而主构造函数constructor关键字可以省略
*/
class Person(age: Int){
      ...
}
```

如果主构造器没有任何注解，也没有任何可见度修饰符，那么constructor关键字可以省略。

```
class Person(age: String) {
}
```

> - 构造函数中不能出现其他的代码，只能包含初始化代码。包含在初始化代码块中。
> - 关键字：`init{...}`
> - 值得注意的是，`init{...}`中能使用构造函数中的参数

例：

```
fun main(args: Array<String>) {
    // 类的实例化，会在下面讲解到，这里只是作为例子讲解打印结果
    var test = Test(1)
}

class Person  constructor(var age : Int){
    init {
        age = 5
        println("age = $age")
    }
}
```

**注意：**

https://www.cnblogs.com/Jetictors/p/7758828.html

constructor省略条件

必须是在默认修饰符public修饰的条件下才可以省略，不然不可以省略，并且在修饰符的后面

- 构造函数

- `Kotlin`中支持二级构造函数。它们以`constructor`关键字作为前缀。

- ```
  class Person {
      constructor(){
          
      }
  }
  ```

#### 类的实例化

在kotlin中，调用构造方法进行实例化，与Java不同的是，kotlin实例化不需要进行new

```
var test = Test()
```

### 修饰符

- 可见修饰符合（注意和java的区别private，none(default)，protected，public）

  - `public`修饰符表示 *公有* 。此修饰符的范围最大。当不声明任何修饰符时，系统会默认使用此修饰符。
  - `internal`修饰符表示 *模块* 。对于`模块`的范围在下面会说明。
  - `protected`修饰符表示 *私有`+`子类*。值得注意的是，此修饰符不能用于`顶层`声明。
  - `private`修饰符表示 *私有* 。此修饰符的范围最小，即可见性范围最低。

- 在类中使用情况

  在类中都可以使用public，internal，protected，private进行修饰，也可以访问任意修饰符修饰的变量和，public，internal可以不同类中使用，private不可以，只能在本类中使用，protected不能那在顶层中修饰

- 在接口中

  只能由`public`修饰符修饰属性。方法可由`public`、`private`两个修饰符去修饰，但是，用`private`修饰符修饰符修饰的方法不能被实现该接口的类重写。

```
fun main() {
    val  a = 10
    println("a = "+a)
}
```

注意：fun用来修饰函数，方法名可以自己定义，方法里带的参数格式为

##  函数

![Kotlin_函数](../media/pictures/KotlinBase.assets/Kotlin_函数.png)

关于函数，[看一参考一下这位博主写的](https://juejin.cn/post/6975384870675546126)

**main函数是程序的入口函数，程序一旦运行，就是从main()函数开始执行的**

**fun（function的简写）是定义函数的关键字，无论你定义什么函数，都一定要使用fun来声明。**

- 定义格式为：`可见性修饰符 fun 函数名(参数名 ：类型,...) : 返回值{}`

函数可以分为：

- 从参数的2角度分为：有参函数，无参函数
- 返回值的角度：有返回值，没返回值

### 普通函数

### 标准函数

任何Kotlin代码都可以自由地调用所有的标准函数。

**let,with,runmapply,repeat**

- let:主要作用就是配合?.操作符来进行辅助判空处理
- with

### 静态函数

companion  object ，@JvmStatic

**Kotlin中的object 与companion object的区别**

[学习链接](https://www.jianshu.com/p/14db81e1576a)

`companion object` 修饰为伴生对象,伴生对象在类中只能存在一个，类似于java中的静态方法 Java 中使用类访问静态成员，静态方法。

### kotlin高阶函数

### 匿名函数

1，定义的时候不取名字的函数，称为匿名函数，匿名函数通常整体传递给其他函数，或者从其他函数返回，**变量有类型，变量可以等于函数，函数也会有类型，函数的类型，由传入的参数和返回值类型决定**

2，	和具名函数不一样，除了极少数情况，匿名函数不需要return关键字来返回数据，匿名函数会隐式或者自定返回函数体最后一行语句的结果

函数参数

和具名函数一样，匿名函数可以不带参数，也可以带一个或多个任何类型的参数，**需要带参数时，参数的类型放在匿名函数的类型定义中，参数名则放在函数定义中。**

### Unit函数



在kotlin中函数没有返回值的的函数叫unit函数，返回类型为unit，在java中void的没有返回值

### Nothing类型

TODO函数的任务就是抛出异常，返回Nothing类型

```
public inline fun TODO(): Nothing = throw NotImplementedError()
```

![反引号中的函数](../media/pictures/KotlinBase.assets/反引号中的函数.png)

### 函数的参数

函数的参数分为：具名函数，默认函数，可变函数

具名函数：指在调用函数的时候指定形参的名称

可变参数：可变参数，是指参数类型确定但个数不确定的参数，可变参数通过vararg关键字标识，我们可以将其理解为数组。可变参数通常声明在形参列表中的最后位置，如果不声明在最后位置，那么可变参数后面的其他参数都需要通过命名参数的形式进行传递。

Kotlin中的可变参数与Java中的可变参数的对比Kotlin中可变参数规则：• 可变参数可以出现在参数列表的任意位置；• 可变参数是通过关键字vararg来修饰；• 可以以数组的形式使用可变参数的形参变量，实参中传递数组时，需要使用“*”前缀操作符。Java中可变参数规则：• 可变参数只能出现在参数列表的最后；• 用“…”代表可变参数，“…”位于变量类型与变量名称之间；• 调用含有可变参数的函数时，编译器为该可变参数隐式创建一个数组，在函数体中以数组的形式访问可变参数。

## 继承/构造函数

### 继承

- **Kotlin 中所有类都继承该 Any 类，它是所有类的超类，对于没有超类型声明的类是默认超类**

  Any 默认提供了三个函数：

  ```
  equals()
  
  hashCode()
  
  toString()
  ```

- 在Kotlin中任何一个非抽象类默认都是不可以被继承的，相当于Java中给类声明了final关键字

- 默认所有非抽象类都是不可以被继承的
- 要让一个类可以被继承，在类的前面加上**open** 

```
open class Person {
....
}
```

- 子类，在java中用extents,kotlin 中用冒号：

  ```
  class Student : Person() {
  ..
  }
  ```

**注意：父类需要加上（）**

### 构造函数

#### 主构造函数

**解释父类需要加上（）的含义**

Kotlin将构造函数分成了两种：主构造函数和次构造函数。

- 主构造函数的特点是没有函数体，直接定义在类名的后面即可。比如下面这种写法：

```
class Student(val sno:String ,val grade:Int ) : Person() {}
```

**注意：**

- Person类后面的空括号表示要去调用Person类中无参的构造函数

- 当有参数的时候，当实例化对象的时候，必须传入要求类型的值，，所以我们可以在定义为val不可变的变量
- **主构造函数没有方法体，如何需要编写逻辑，这是可以用init结构体**（因为在绝大多数的场景下，我们是不需要编写init结构体的。）

```
class Student(sno:String ,grade:Int ) : Person() {
    
    init {
        println("sno is"+sno)
        println("grade is"+grade)
        
    }
}
```

#### 次构造函数

次构造函数是通过constructor关键字来定义的

当一个类中既有主函数，又有次构造函数的时候，，在次构造函数必须调用主构造函数

```
class Student(sno:String ,grade:Int,name:String,age:Int ) : Person(name,age) {
    init {
        println("sno is"+sno)
        println("grade is"+grade)
    }
    constructor(name: String,age: Int):this("",20,name,age){}
    constructor():this("",20){}
}
```

### 实例

```
/**用户基类**/
open class Person(name:String){
    /**次级构造函数**/
    constructor(name:String,age:Int):this(name){
        //初始化
        println("-------基类次级构造函数---------")
    }
}

/**子类继承 Person 类**/
class Student:Person{

    /**次级构造函数**/
    constructor(name:String,age:Int,no:String,score:Int):super(name,age){
        println("-------继承类次级构造函数---------")
        println("学生名： ${name}")
        println("年龄： ${age}")
        println("学生号： ${no}")
        println("成绩： ${score}")
    }
}

fun main(args: Array<String>) {
    var s =  Student("李四", 21, "12325", 129)
}
```

输出结果：

```
-------基类次级构造函数---------
-------继承类次级构造函数---------
学生名： 李四
年龄： 21
学生号： 12325
成绩： 129
```

## 接口

Java中继承使用的关键字是extends，实现接口使用的关键字是implements，而Kotlin中统一使**用冒号**，中间用逗号进行分隔

Java中有public、private、protected和default（什么都不写）这4种函数可见性修饰符。Kotlin中也有4种，分别是public、private、protected和internal，需要使用哪种修饰	

- **private：*在java中一样*，在类的内部使用
- public：public修饰符的作用虽然也是一致的，表示对所有类都可见，但是在Kotlin中public修饰符是默认项，而在Java中default才是默认项
- protacted:protected关键字在Java中表示对当前类、子类和同一包路径下的类可见，在Kotlin中则表示只对当前类和子类可见
- 只对同一模块中的类可见，使用的是internal修饰符

1，接口里面的所有成员和接口本身都是public open的

2，接口不能有主构造

3，实现类不仅仅要重写接口的函数，也要重写接口的成员

4，接口，var要是给接口的成员赋值的（但是与其他方法）

5，任何类，接口 等等，val代表只读，是不可以在后面动态改变

## 数据类与单例类

shuffled,val不能set

### 数据类

Kotlin 可以创建一个只包含数据的类，关键字为 **data**：

```
 data class Cellphone (val brand:String,val price:Double){ //当一个类中没有任何代码时，还可以将尾部的大括号省略
}
```

编译器会自动的从主构造函数中根据所有声明的属性提取以下函数：

- `equals()` / `hashCode()`
- `toString()` 格式如 `"User(name=John, age=42)"`
- `componentN() functions` 对应于属性，按声明顺序排列
- `copy()` 函数

如果这些函数在类中已经被明确定义了，或者从超类中继承而来，就不再会生成。

为了保证生成代码的一致性以及有意义，数据类需要满足以下条件：

- 主构造函数至少包含一个参数。
- 所有的主构造函数的参数必须标识为`val` 或者 `var` ;
- 数据类不可以声明为 `abstract`, `open`, `sealed` 或者 `inner`;
- 数据类不能继承其他类 (但是可以实现接口)。

```
data class Cellphone(val brand: String, val price: Double) {
}
fun main() {
    val cellphone1 = Cellphone("A", 1234.3)
    val cellphone2 = Cellphone("B", 12345.3)
    println(cellphone1)
    println("cellphone1 equals cellphone2"+(cellphone1 == cellphone2))

}
输出：
Cellphone(brand=A, price=1234.3)
cellphone1 equals cellphone2false
如果没有data,会输出cellphone1 equals cellphone2false
```

### open



### 单例类

在Kotlin中创建一个单例类的方式极其简单，只需要将**class关键字改成object关键字**即可。

```
object  Singleton {
    fun singleTonTest(){
        println("singleTonTest is called.")
    }
}
 Singleton.singleTonTest(); //调用单实例模式
```

## Lambda编程

### 集合的创建与遍历

list,set,map

```
/**
 * 集合list
 */
class List  {

}
fun main(){
   println("------list--------")
    // 方式一
    val  list = ArrayList<String>()
    list.add("A")
    list.add("B")
    list.add("C")

    // 方式二，并且用for进行遍历
    val  list2 = listOf("A","B","C")
    for (fruit in list2){
        println(fruit)
    }
    
       println("------map--------")
    //方法1
    val map  = HashMap<String ,Int>()
    map["A"]=1
    map["B"]= 2
    map["C"]= 3
    //方法二
    val  map2 = mapOf("A" to 1,"B" to 2 ,"C" to 3)
    for ((fruit,num)in  map2){
        println("fruit is "+fruit+",num is "+num)
    }
}
```

注意：listOf创建的是一个不可变得集合（只能进行读取，不能进行修改）

​           mutableListOf：创建一个可变的集合。可以进行修改

​            set和list的用法差不多一样，setOf,mutableSetof

### lambda

Lambda就是一小段可以作为参数传递的代码

语法：

{参数1：参数类型，参数2：参数类型 -> 函数体	}

```
val list2 = listOf("apple", "Banana", "Pear")
var maxLengthFruit = ""
for (fruit in list2) {
    if (fruit.length > maxLengthFruit.length) {
        maxLengthFruit = fruit
    }
    // 方式1
  var lamba = { fruit: String -> fruit.length }
  var  maxLengthFruit = list2.maxBy(lamba)
    //方式2
    var  maxLengthFruit = list2.maxBy({fruit:String->fruit.length})
    //方式3
    var maxLengthFruit = list2.maxBy(){ fruit: String -> fruit.length }//当Lambda参数是函数的最后一个参数时，可以将Lambda表达式移到函数括号的外面
    //方式3
    var  maxLengthFruit = list2.maxBy{fruit:String->fruit.length}
    //方式4
    var maxLengthFruit = list2.maxBy { it.length }//当Lambda表达式的参数列表中只有一个参数时，也不必声明参数名，而是可以使用it关键字来代替
    println("max length is " + maxLengthFruit)
}
```

# 空指针检查

Kotlin却非常科学地解决了这个问题，它利用编译时判空检查的机制几乎杜绝了空指针异常。

- **在kotlin中所有的参数和变量都不可为空**，**但是根据需要我们需要为空的情况，类名的后面加上一个问号**。比如，Int表示不可为空的整型，而Int?就表示可为空的整型；String表示不可为空的字符串，而String?就表示可为空的字符串。

## 判空辅助工具

### ?.操作符

当对象不为空时正常调用相应的方法，当对象为空时则什么都不做

```
if(a != null){
    a.eat()
}
//使用操作符
a?.eat()
```

### ?:操作符

这个操作符的左右两边都接收一个表达式，如果左边表达式的结果不为空就返回左边表达式的结果，否则就返回右边表达式的结果。

```
val c = if (a != null){
    a
}else{
    b
}
//使用操作符
 var c = a?:b
```

### let

let既不是操作符，也不是什么关键字，而是一个函数

let函数是可以处理全局变量的判空问题的

```
fun study(Study study?){
    study?.eat()
    study?.study()
}
fun study(Study study?){
    study？.let{stu->
        study.eat()
        study.study()
    }
    fun study(Study study?){
        study？.let{
        it.eat()
        it.study()
    }
```

## 字符串内嵌表达式

**学习：**

**空指针检测，val与var推断，高级函数，匿名函数，函数类型和隐式返回，函数参数的学习，it关键字 ,匿名函数类型推断，lamba**

# 泛型

Any是Kotlin中所有类的共同基类，相当于Java中的Object，而Any？则表示允许传入空值。
