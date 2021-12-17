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

