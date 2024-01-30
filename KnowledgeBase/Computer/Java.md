---
tags:
- coding
alias:
---
# 简介 

从广义上讲，[[Kotlin]]、Clojure、JRuby、Groovy等运行于Java虚拟机上的编程语言及其相关的程序都属于Java技术体系中的一员。如果仅从传统意义上来看，JCP官方[1]所定义的Java技术体系包括了以下几个组成部分:

- Java程序设计语言
- 各种硬件平台上的Java虚拟机实现
- [[class文件]]格式
- Java类库API
- 来自商业机构和开源社区的第三方Java类库


## Classes and Objects 
Three events occur as particular of the creation of a new instance of a class:
- A new [[对象]] is dynamically allocated  in memory, and all instance variables are initialized to standard default values. The default values are null form reference variables and 0 for all [[Base types in Java]] except boolean variables (which are false by default )
- The constructor for the new [[对象]] is called with the parameters specified.
- After the constructor returns, the new operator returns a reference (that is, a memory address) to the newly created [[对象]].
# Fundamental Programming Structures

## Data Types

Java is a strongly typed language.

| Type-Collection      | Content                                                                   | note                                                                                                             |
| -------------------- | ------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Integer Types        | <p>int - 4 bytes<br>short - 2bytes<br>long - 8 bytes<br>byte - 1 byte</p> | Under Java, the ranges of integer types do not depend on the machine on which you will be running the Java code. |
| Floating-Point Types | <p>float - 4 bytes<br>double - 8 bytes</p>                                |                                                                                                                  |
| Char Type            |                                                                           | Better not to use                                                                                                |
| Boolean Type         |                                                                           | cannot convert between integers and boolean values                                                               |

## Strings

### Substrings

```java
String s = "Hello World".substring(0, 5); // s = "Hello"
```

### Build Strings

To efficiently build string from words or keystrokes, we can use StringBuilder.

```java
StringBuilder builder = new StringBuilder();
builder.append(charType);
builder.append(StringType);
String result = builder.toString();
```

## Input and Output

### Read Input

```java
Scanner in = new Scanner(System.in)
String inputLine = in.nextLine();
String inputWord = in.next();
int inputInt = in.nextInt();
```

### File Input and Output

```java
// read from file
Scanner in = new Scanner(Path.of("myfile.txt"), StandardCharsets.UTF_8);
// write to a file
PrintWriter out = new PrintWriter("myfile.txt", StandardCharsets.UTF_8);
out.println("output a new line");
```

## Big Numbers

If the precision of the basic integer and floating-point types is no sufficient, you can turn to a couple of handy classes in the java.math package: BigInteger and BigDecimal.

```java
BigInteger bigInt = BigInteger.valueof(100);
BigInteger bigLong = new BigInteger("100000000")
```

## Arrays

An array is a data structure that stores a collection of values of the same types.

```java
int[] array = new int[10];
```

You can copy all values of one array to a new array.

```java
int[] newArray = Arrays.copyOf(oldArray, 2 * oldArray.length);
```

By which you can increase the size of your old array.

### Array Sorting

To sort an array of numbers, you can use one of the sort() method in the Array class:

```java
int[] a = new int[10000];
Arrays.sort(a);   // sort an array with QuickSort algorithm
```
