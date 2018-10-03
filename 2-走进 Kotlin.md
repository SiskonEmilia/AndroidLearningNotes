走进 Kotlin
===

<!-- TOC -->

- [快速上手：基本语法 Basic Syntax](#快速上手基本语法-basic-syntax)
    - [定义包](#定义包)
    - [定义函数](#定义函数)
    - [定义变量](#定义变量)
    - [注释](#注释)
    - [字符串模板](#字符串模板)
    - [条件语句](#条件语句)
    - [可空值与 null 的检查](#可空值与-null-的检查)
    - [类型检查 与 自动类型转换](#类型检查-与-自动类型转换)
    - [for 循环](#for-循环)
    - [while 循环](#while-循环)
    - [when 语句](#when-语句)
    - [Range 区间](#range-区间)
    - [Collection 集合](#collection-集合)
    - [创建基本类的实例](#创建基本类的实例)
- [快速上手：惯用方法 Idioms](#快速上手惯用方法-idioms)
    - [DataTransferObject 创建数据传输对象](#datatransferobject-创建数据传输对象)
    - [函数默认值](#函数默认值)
    - [筛选列表](#筛选列表)
    - [映射 map](#映射-map)
    - [遍历一个映射/对偶表](#遍历一个映射对偶表)
    - [延迟属性](#延迟属性)
    - [扩展函数](#扩展函数)
    - [单例模式](#单例模式)
    - [判断非空的简写](#判断非空的简写)
    - [try-catch](#try-catch)
    - [if](#if)
    - [with](#with)
- [基础知识：基本类型](#基础知识基本类型)
    - [数字](#数字)
        - [数字的字面值常量](#数字的字面值常量)
        - [“相等”的坑](#相等的坑)
        - [操作与一些共识](#操作与一些共识)
    - [字符](#字符)
    - [布尔值](#布尔值)
    - [数组](#数组)
    - [字符串](#字符串)
- [小小拓展：返回与跳转](#小小拓展返回与跳转)

<!-- /TOC -->

**Kotlin** 是一个用于现代多平台应用的 **静态** 编程语言。其被设计来可以与 JAVA 和 JavaScript 进行互相转换，因而可以适用于任何使用  JAVA 或 JavaScript 的场景。随着 Google 将 Android 平台的官方指定语言由 JAVA 改为 Kotlin，这门语言正在受到越来越多的关注。

毫不夸张地讲，如果要开发 Android 应用，Kotlin 将会是你最好的编程语言的选择，没有之一。Kotlin 的一些特性能够避免 JAVA 对项目带来的隐患，还能更简单地实现很多功能。那么接下来，我们就一起步入 Kotlin 的世界吧！

*注：本文不针对没有编程基础的同学，你可能需要一些 JAVA 和 JS基础来理解本文。本教程仅是快速上手，更多特性请在官网查阅。*

## 快速上手：基本语法 Basic Syntax

### 定义包

```Kotlin
package my.demo
import java.util.*
```

几乎和 JAVA 一模一样啊……不过 **Kotlin 并不要求目录和包名匹配**，你可以更加随意地放置源文件。

### 定义函数

以两个 Int 为参数，返回一个 Int 值的函数

```Kotlin
fun sum(a: Int, b: Int) : Int {
    return a + b
}
```

以一个表达式为 body 的，自动推断返回类型的函数

```Kotlin
fun sum(a: Int, b: Int) = a + b
```

返回值无意义的函数

```Kotlin
fun printSum(a: Int, b: Int): Unit {
    println("sum of $a and $b is ${a+b}")
}
// Unit 可被省去
```

### 定义变量

```Kotlin
val a: Int = 1
val b = 2
val c: Int
c = 3
// val variables cannot be changed
// but theirs assignments could be defered
```

```Kotlin
var x = 5
x += 1
// var variables could be changed
```

### 注释

```Kotlin
// They are actually the same as C/C++

/**
* Both in-line ones and block ones
**/
```

官方还说块式的注释甚至可以嵌套……一脸懵逼……？

### 字符串模板

```Kotlin
var a = 1;
val s1 - "a is $a"
// s1 = "a is 1"

a = 2
val s2 = "${s1.replace("is", "was")}", but now is $a"
```

### 条件语句

```Kotlin
if (true) {
  // do A
} else {
  // do B
}

if (true) /* do A */ else /* do B */
```

### 可空值与 null 的检查

```Kotlin
fun parseInt(str: String): Int? {
  // return null if str is not a Int
}
```

### 类型检查 与 自动类型转换

由 `is`  和 `Any` 关键词实现的多类型支持：

```Kotlin
fun getStringLength(obj: Any): Int? {
  if (obj is String) {
    return obj.length;
  }

  return null;
}
```

`Any` 使得函数可以匹配所有类型的参数，在函数体中，`is` 检查参数是否为 `String` 类型的实例，如果是，则会在分支中将其自动转换为 `String` 类型（在分支外依旧是 `Any` 类型）。`is` 的否定形式是 `!is`。

### for 循环

for-in 循环：

```Kotlin
for (item in items) {
  // ...
}

for (item in items.indices) {
  // items[item]...
}
```

循环固定次数的 for 循环：

```Kotlin
for (i in 1..3) {
  // ...
}

for (i in 3 downto 0 steop 1) {
  // ...
}
```

甚至还能……

```Kotlin
for ((index, value) in items.withIndex()) {
  // items[index] == value
}
```

### while 循环

```Kotlin
while(true) {
  // ...
}
```

### when 语句

其实就是一个超级“三目运算符”：

```Kotlin
fun describe(obj: Any): String =
  when(obj) {
    1           -> "one"
    "Hello"     -> "Greeting"
    is Long     -> "Long"
    !is String  -> "Not a String"
    else        -> "Unknown"
  }
```

### Range 区间

```Kotlin
if (value in 1..3) {
  // judge if value is an integer in [1, 3]
}

if (value in 1..3 step 2) {
  // value is 1 or 3 => true
}

if (value in 3 downto 1 step 2) {
  // inverse order
}
```

### Collection 集合

```Kotlin
val fruits = listOf("banana", "avocado", "apple", "kiwfruit")
fruits
  .filter { it.startsWith("a") }
  .sortedBy { it }
  .map { it.toUpperCase() }
  .forEach { println(it) }

// Result:
// APPLE
// AVOCADO
```

### 创建基本类的实例

```Kotlin
val rectangle = Rectangle(5.0, 2.0)
var triangle = Triangle(3.0, 4.0, 5.0)
```

## 快速上手：惯用方法 Idioms

这一部分讲述一些 Kotlin 中常用的方法

### DataTransferObject 创建数据传输对象

DTO，或者更普遍的 PlianOrdinaryJavaObject，可以通过如下方式创建：

```Kotlin
data class Custormer(val name: Strintg, val email String)
// This is a DTO which support:
// getter & setter in case of vars for all properties
// equals()
// hashCode()
// toString()
// copy()
// component1(), ... for all properties
```
### 函数默认值

```Kotlin
fun foo(a: Int = 0, b: String = "") { ... }
```

### 筛选列表

```Kotlin
val positives = list.filter { x -> x > 0}
val positives = list.filter { it > 0 }
```

二者意思相同

### 映射 map

```Kotlin
var map = mapOf("a" to 1, "b" to 2, "c" to 3)
map["a"] // 1
map["b"] = -2
map["b"] // -2
```

### 遍历一个映射/对偶表

```Kotlin
for ((k,v) in map) {
  // do something
}
```

### 延迟属性

```Kotlin
val p: String by lazy {
  println("Say hello");
  "hello"
}
```

在变量被创建的时候，`p` 并未被赋值。在第一次调用时，`lazy` 中的方法将会被调用，`p` 真正获得初值，之后的调用只是简单地调用此初值。`lazy` 只用于常量。

### 扩展函数

Kotlin 允许对已有的类添加新的方法，比如：

```Kotlin
fun String.spaceToCamelCase() { ... }
```

### 单例模式

```Kotlin
object Resource {
  val name = "TheOne"
}
```

### 判断非空的简写

```Kotlin
val files = File("Text").listFiles()
println(files?.size ?: "empty")

val emails = getEmail()
val mainEmail = emails.firstOrNull() ?: ""
val mainEmail = emails["first"] ?: throw IllegalStateException("Email is missing!")

val value = ...
value?.let {
  // do something
} ?: // do other things
```

### try-catch

和其他语言一样，但是支持返回值

### if

和其他语言一样，但是支持返回值

### with

```Kotlin
class Turtle {
  fun method1()
  fun method2()
}

val turtle = Turtle()
with(turtle) {
  method1()
  method2()
}
```

## 基础知识：基本类型

### 数字

`Double`,`Float`,`Long`,`Int`,`Short`,`Byte` 均为定长数据类型。注意：字符在 Kotlin 中 **不是数字类型**。

#### 数字的字面值常量

- 十进制：123（Long 型为 123L）
- 十六进制：0x0F
- 二进制：0b00001011
- 八进制：不支持
- 双精度浮点：1.1
- 单精度浮点：1.1F/1.1f

数字字面值允许中间出现下划线来增加可读性

#### “相等”的坑

在 JVM 的语境下，非空的数据类型将被直接存储在 JVM 上，但是可空类型将会被“包装 boxed”起来。同时，Kotlin 不允许类型间的隐式转换。因而会出现这种情况：

```Kotlin
val a: Int = 1000
println(a === a) // true
val boxedA: Int? = a
val anotherBoxedA: Int? = a
println(boxedA === anotherBoxedA) // false
println(boxedA == anotherBoxedA) // true
println(boxedA.equals(anotherBoxedA)) // error! unsafe equal!
println(boxedA?.equals(anotherBoxedA)) // true

val boxedLongA: Long? = a // error!
val boxedLongA: Long? = a.toLong()
println(boxedLongA == boxedA) // error!
println(boxedLongA.equals(boxedA)) // false
println(boxedLongA.equals(boxedA.toLong())) // true
```

#### 操作与一些共识

- `shl`: 有符号的左移（<<）
- `shr`: 有符号的右移（>>）
- `ushr`: 无符号的右移（>>>）
- `and`
- `or`
- `xor`
- `inv`
- `NaN === NaN`
- `NaN > POSITIVE_INFINITY`
- `-0.0 < 0.0`

### 字符

在 Kotlin 中，字符以类型 `Char` 表示，该类型不能直接当作数字使用

```Kotlin
Char a = 'a'
a == 3 // error!
a == 'a' // true
a in 'a'..'z' // true
```

### 布尔值

和 JAVA 一致

### 数组

Kotlin 中，数组以 `Array` 类的实例呈现。

```Kotlin
val arr = Array(5, { i -> (i * i).toString() })
// ["0", "1", "4", "9", "16"]
val arr2 = arrayOf(1, 4, 9)
// [1, 4, 9]
```

### 字符串

Kotlin 中的字符串是 `String` 类的实例，字符串是不可变的，其中每个字符都可以通过类似数组的方式访问。

字符串的 + 操作符表示将操作数转换为字符串后拼接在一起。

三个引号可以构成 原始字符串 raw string，其可以包含任何字符而无需转义：

```Kotlin
var str = """
  This is a test for raw string
"""

var strWithoutPrefixSpace = """
  |This is a test for raw string
  |No space will remain!
""".trimMargin()
```

`trimMargin()` 方法去除指定字符之前的空格，默认是 `|`

原始字符串同样支持字符串模板。

## 小小拓展：返回与跳转

这部分要讲的是 Kotlin 中神奇的三个语句：

- return
- break
- continue

欸？等等……这些语句是个高级语言都有的吧！怎么就成 Kotlin 的神奇语句了？

莫慌莫慌，我们细细道来……

在 Kotlin 中，任何表达式都可以通过标签来标记。其格式为 标识符 + `@`，比如 `abc@`。他们可以成为返回和跳转的目标：

```Kotlin
loop@ for (i in 1..100) {
  for (j in 1..100) {
    if (...) break@loop
  }
}

fun foo() {
  listOf(1, 2, 3, 4, 5).forEach(fun(value: Int) {
    if (value == 3) return
    print(value)
  })
}

fun foo() {
  listOf(1, 2, 3, 4, 5).forEach(
    if (value == 3) return@forEach
    print(value)
  )
}

fun foo() {
  listOf(1, 2, 3, 4, 5).forEach@label(
    if (value == 3) return@label
    print(value)
  )
}
```

在 `foo` 的例子中，三者都相当于 `continue`，那么 `break` 要如何等价呢？创建一层嵌套的 lambda 即可：

```Kotlin
fun foo() {
  run @label{
    listOf(1, 2, 3, 4, 5).forEach(
      if (value == 3) return@label
      print(value)
    )
  }
}
```