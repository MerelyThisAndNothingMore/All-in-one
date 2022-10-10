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
