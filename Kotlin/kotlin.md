@import "./image/Kotlin基础TOC.jpeg"


<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [编译](#编译)
  - [Kotlin文件](#kotlin文件)
  - [Kotlin脚本](#kotlin脚本)
  - [Kotlin REPL](#kotlin-repl)
- [字符串](#字符串)
- [可变参数](#可变参数)
- [spread运算符](#spread运算符)
- [解构](#解构)
- [其它](#其它)
- [外部迭代和参数匹配](#外部迭代和参数匹配)
  - [范围](#范围)
  - [迭代](#迭代)
  - [When](#when)
- [集合](#集合)
  - [Pair 与 Triple](#pair-与-triple)
  - [Array](#array)
  - [List](#list)
  - [Set](#set)
  - [Map](#map)

<!-- /code_chunk_output -->


## 编译

### Kotlin文件

```bash
kotlinc Main.kt
```

```kotlin
fun main() = println("hello world")
```

编译会生成 `MainKt.class` 

```bash
kotlin MainKt
```

```kotlin
object Main() {
    @JvmStatic
    fun main() = println("hello world")
}
```
编译会生成 `Main.class`

```bash
kotlin Main
```

### Kotlin脚本

运行Kotlin脚本：

```kotlin
java.io.File(".")
    .walk()
    .filter {
        file -> file.extension == "kts"
    }
    .forEach {
        println(it)
    }
```
可直接执行脚本：

```bash
kotlin KtScript.kts
```
> ./KtScript.kts

### Kotlin REPL

可直接在终端输入kotlin，进行小段代码调试：

```bash
kotlin
>>> val a = 1
>>> val b = 2
>>> a + b
```
> res2: kotlin.Int = 3

## 字符串

多行字符串

```kotlin
val str = """hello world,
                            world hello"""
```

格式化多行字符串

```kotlin
    val a = """hello world.
        world hello 
            hello world
    """
```
```
hello world.
        world hello 
            hello world
```

使用`trimMargin`格式化多行字符串

```kotlin
    val a = """hello world.
        |world hello 
            |hello world
    """.trimMargin()

    // or

        val a = """hello world.
        ~world hello 
            ~hello world
    """.trimMargin(~)
```

```
 hello world.
    world hello 
    hello world
```

## 可变参数

```kotlin
fun max(vararg msgs : String) {
    msgs.forEach {
        print("$it ")
    }
}
// a b c 
max("a", "b", "c")
```

## spread运算符

```kotlin
val msgs = listOf("a", "b", "c")
// a b c 
max(*msgs)
```

## 解构

```kotlin
fun getValues() = listOf("a", "b", "c")
val (x, _, z) = getValues()
```

以上忽略第二个值，给x，y分别赋值 “a” 和 “c”

## 其它

`::class` 与 `.javaClass`

```kotlin
fun main() {
    val a = "test"

    // class kotlin.String
    println(a::class)
    // class java.lang.String
    println(a.javaClass)
}
```

```kotlin
fun f1() = 2
fun f2() = {2}
fun f3(factor : Int) = { n : Int -> n * factor }

// 2
println(f1())
// () -> kotlin.Int
println(f2())
// 2
println(f2()())
// (kotlin.Int) -> kotlin.Int
println(f3(2))
// 6
println(f3(2)(3))
```

## 外部迭代和参数匹配

### 范围

```kotlin
val ranges = "a".."d"
// true
ranges.contain("c")
```

### 迭代

* 正向迭代

```kotlin
// for(r in 'a' until 'd')
for(r in 'a'..'d') {
    // a b c d
    println("$r ")
}
```

* 反向迭代

```kotlin
// for(r in 5 downTo 1)
for(r in 5.downTo(1)) {
    // 5 4 3 2 1
    println("$r ")
}
```

* 跳过范围内的值

```kotlin
for(r in 'a'..'e' step 2) {
    // a c e
    print("$r ")
}
```

* 过滤范围内的值

```kotlin
for(r in ('a'..'e').filter { it == 'b' }) {
    // b
    print("$r ")
}
```

* 遍历获取集合的索引

```kotlin
for(index in list.indices) {
    println("index: $index")
}
```

### When

* 用作表达式

```kotlin
fun is3(number : Int) = when {
    number == 1 -> false
    number == 2 -> false
    number == 3 -> true
    else -> false
}

fun is3(number : Int) = when(number) {
    1 -> false
    in 2..5 -> true
    6 -> false
    else -> false
}
```

必须要写else

* 用作语句

```kotlin
fun is3(number : Int) {
    when(number) {
        1 -> println("false")
        2 -> println("false")
        3 -> println("true")
        else -> println("false")
    }
}
```

when括号中写表达式

```kotlin
fun is3() = when(val number = getNumber()){
    number == 1 -> false
    number == 2 -> false
    number == 3 -> true
    else -> false
}
```

## 集合

### Pair 与 Triple

* Pair

```kotlin
fun getPairs() {
    val ls = listOf(1, 2, 3)
    val ps = ls.map {
        l -> l to toEnglish(l)
    }
    for (p in ps) {
        println("p: $p")
    }
}

fun toEnglish(number : Int) : String {
    return when (number) {
        1 -> "one"
        2 -> "two"
        3 -> "three"
        else -> "none"
    }
}

// (4, forth)
val ps = Pairs(4, "forth")
```

> p: (1, one)
p: (2, two)
p: (3, three)

* Triple

```kotlin
// (1, "one", "first")
Triple(1, "one", "first")
```

### Array

对象及基本类型数组，固定大小，不可变

```kotlin
val arrays = arrayOf("a", "b", "c")
for (index in arrays.indices) {
    print("$index ")
}
println()
for (value in arrays) {
    print("$value ")
}

// [1*1, 2*2, 3*3]，即：[1, 4, 9]
// 1 + 4 + 9，最终输出 14
println(Array(3) {i -> (i + 1) * (i + 1)}.sum())
```

> 0 1 2 
a b c 

### List

有序可重复集合

* 不可变集合

```kotlin
// [a, a ,b, c]
val ls = listOf("a", "a", "b", "c")
// [a, b, c]
// 生成一个全新的集合
ls - "a"
```

* 可变集合

```kotlin
val ls = mutableListOf("a", "a", "b", "c")
// [a, a, c]
ls.remove("b")
```

### Set

无序不可重复集合

```kotlin
// Set
val set = setOf("b", "a", "a", "c")
// HashSet，哈希表
val hashSet = hashSetOf("b", "a", "a", "c")
// LinkedHashSet，链表维护的哈希表
val linkedSet = linkedSetOf("b", "a", "a", "c")
// TreeSet，红黑树
val sortedSort = sortedSetOf("b", "a", "a", "c")
println(set)
println(hashSet)
println(linkedSet)
println(sortedSort)
```

>[b, a, c]
[a, b, c]
[b, a, c]
[a, b, c]

因为Set的无序性，无法通过index进行访问元素，只能用迭代器遍历

### Map

```kotlin
val map = mapOf(1 to "one", 2 to "two")
// {1=one, 2=two, 3=three}
// 生成一个全新的映射集合
map + (3 to "three")

map.containsKey(1)
map.containsValue("one")
// 判断是否有key为2的键值对，效果等同于containsKey
map.contains(2)
```

获取某个key的值

```kotlin
// error
val value = map.get(1);
// right
val value : String? = map.get(1)
// or
val value = map.getDefault(1, 0)

val value : String? = map[1]
```