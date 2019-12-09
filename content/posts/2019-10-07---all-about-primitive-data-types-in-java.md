---
title: All about Primitive data types in Java
date: '2019-10-07 12:40'
template: "post"
draft: false
slug: "/posts/all-about-primitive-data-types-in-java/"
category: "Java"
tags:
  - "coreJava"
  - "java"
  - "javaDataTypes"
description: 
socialImage: "/media/image-2.jpg"
---
* 6 Numeric data types
   * 4 int types varying on number of bits and therefore range (byte, short, int, long)
     * byte - 8 bits, Range - -127 to 128, default value - 0
     * short - 16 bits, Range - -32,768 to 32,767, default value - 0
     * int - 32 bits, Range - -2^31 to 2^31-1, default value - 0
     * long - 64 bits, Range - -2^63 to 2^63-1, default value - 0
     * Both `byte` and `short` are typically used when memory and disk space is low. Otherwise, it is safe to use `int` as it is the default type when declaring integers.
   * 2 floating point (float, double)
     * float - 32 bits, 7 decimal places, 0.0f
     * double - 64 bits, 14 decimal places, 0.0d
     * In general, `double` is the default choice for floating point values in Java
* Char datatype - 16 bits, unsigned, Range - ‘\u0000’ or 0 to ‘\uffff’ or 65535, default value - '\u0000'
  * the `char` type is the only unsigned type in Java. In other words, a character can range from 0 to 65,535 where each number maps to a specific character.
* Boolean - size not specified

###  Widening Primitive Conversions
* When we need to convert from a primitive that is simpler or smaller than the destination type, we don't have to use any special notation for that.
* During widening conversion, the smaller primitive value is placed over a larger container, which means that all the extra space, on the left of the value, is filled with zeros.
```java
int myInt = 127;
long myLong = myInt;
```

### Narrowing Primitive Conversion
* Sometimes we need to fit a value that is larger than the type used in the variable declaration. This may result in information loss since some bytes will have to be discarded.
* In this case, we have to explicitly express that we are aware of the situation and we agree with that, by using a cast.

```java
int myInt = (int) myDouble;
byte myByte = (byte) myInt;
```

### String Conversions
All the primitive types may be converted to String through their Wrapper Classes, which override the toString() method.
```java
String myString = myIntegerReference.toString();
```

* If we need to go back to a primitive type, we need to use a parse method defined by the corresponding Wrapper Class:
```java
byte  myNewByte   = Byte.parseByte(myString);
short myNewShort  = Short.parseShort(myString);
int   myNewInt    = Integer.parseInt(myString);
long  myNewLong   = Long.parseLong(myString); 
float  myNewFloat  = Float.parseFloat(myString);
double myNewDouble = Double.parseDouble(myString);
boolean myNewBoolean = Boolean.parseBoolean(myString);
```

### Numeric Promotions
* To execute a binary operation, it is necessary that both operands are compatible in terms of size.
There is a set of simple rules that apply:

  * If one of the operands is a `double`, the other is promoted to `double`
  * Otherwise, if one of the operands is a `float`, the other is promoted to `float`
  * Otherwise, if one of the operands is a `long`, the other is promoted to `long`
  * Otherwise, both are considered `int`

## Why does it happen when we add two bytes, result is an int ?
* A compiler encodes loads of literal values of types `byte` and `short` using Java Virtual Machine instructions that sign-extend those values to values of type `int` at compile-time or run-time. 
* Loads of literal values of types `boolean` and `char` are encoded using instructions that zero-extend the literal to a value of type `int` at compile-time or run-time.
* Thus, most operations on values of actual types `boolean`, `byte`, `char`, and `short` are correctly performed by instructions operating on values of computational type `int`.
* The reason behind this is the Java Virtual Machine's one-byte opcode size.
* encoding types into opcodes places pressure on the design of its instruction set. If each typed instruction supported all of the Java Virtual Machine's run-time data types, then there would be more instructions than could be represented in a byte. 
* When an operator applies binary numeric promotion to a pair of operands, each of which must denote a value that is convertible to a numeric type, the following rules apply, in order, using widening conversion to convert operands as necessary: 
    * If any of the operands is of a reference type, unboxing conversion is performed. Then: 
    * If either operand is of type `double`, the other is converted to `double`. 
    * Otherwise, if either operand is of type `float`, the other is converted to `float`. 
    * Otherwise, if either operand is of type `long`, the other is converted to `long`. 
    * Otherwise, both operands are converted to type `int`.
