---
title: Java Integer Cache - Why Integer.valueOf(127) == Integer.valueOf(127) Is True
date: '2019-09-18T06:44:48.872Z'
template: "post"
draft: false
slug: "/posts/java-integer-cache-why-integer-valueof-127-integer-valueof-127-is-true/"
category: "Java"
tags:
  - "coreJava"
  - "java"
  - "javaDataTypes"
description: 
socialImage: "/media/image-2.jpg"
---
**If we initialize two Integer objects with literal values, Integer a = 127; Integer b = 127; Why does a == b evaluate to true when both are holding two separate objects?**

* Direct assignment of an `int` literal to an `Integer` reference is an example of **_auto-boxing_** concept where the literal value to object conversion code is handled by the compiler, so during compilation phase, compiler converts `Integer a = 127;` to `Integer a = Integer.valueOf(127);`.
* The `Integer` class maintains an internal `IntegerCache` for integers which, by default, ranges from **_ -128 to 127_** and `Integer.valueOf()` method returns objects of mentioned range from that cache. 
* So a == b returns true because a and b both are pointing to the same object.

### Under the hood
* It is the `Integer.valueOf()` method that is returning these integer objects, which means this method must be doing something under the hood.
* And if we take a look at the source code of `Integer.valueOf()` method, we can clearly see that if the passed int literal i is greater than `IntegerCache.low` and less than `IntegerCache.high` ,then the method returns Integer objects from `IntegerCache`. Default values for `IntegerCache.low` and `IntegerCache.high` are -128 and 127 respectively.
* In other words, instead of creating and returning new integer objects, `Integer.valueOf()` method returns `Integer` objects from an internal `IntegerCache` if the passed int literal is greater than -128 and less than 127.
* Java caches integer objects that fall into -128 to 127 range because this range of integers gets used a lot in day-to-day programming, which indirectly saves some memory.
* The cache is initialized on the first usage when the class gets loaded into memory because of the static block. The max range of the cache can be controlled by the `-XX:AutoBoxCacheMax` JVM option.
* Note that the cache is pre-filled on startup rather than on demand, meaning that the memory footprint of the cache is constant no matter which Integers are actually used in the VM. Tweaking the cache to a very large size thus comes with a steep prize in terms of memory consumption.
* Also the parameter to set the size of the cache only affects the upper limit and never the lower, meaning that there is no simple way to get Integers smaller than -128 to be interned in the cache.

```java
public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
```

### ByteCache, ShortCache, LongCache, CharacterCache

* This caching behavior is not applicable to Integer objects only. Similar to `Integer.IntegerCache`, we also have `ByteCache`, `ShortCache`, `LongCache`, `CharacterCache` for `Byte`, `Short`, `Long`, `Character` respectively.
* `Byte`, `Short`, and `Long` have a fixed range for caching between -127 to 127 (inclusive), but for `Character`, the range is from 0 to 127 (inclusive). The range can be modified via argument only for Integer but not for others.

### Word of Caution
* This behaviour is exhibited only when using - `Integer.valueOf()`. when initializing the `Integer` object either by directly assigning the int literal to Integer object and compiler using `Integer.valueOf()` under the hood or manually initializing Integer object by calling `Integer.valueOf()` method.
* If we use 'new' operator to initialize the `Integer` object, then `IntegerCache` does not come into the picture and a new object is created every time. (This feature was supposed to be deprecated in Java 9. I will have confirm on this.)

```java
public class Code {
  public static void main(String[] args) {    
    Integer a = 127;
    Integer b = 127;
    System.out.println("Direct assignment of integer literal - "+(a == b));
    
    Integer c = new Integer(127);
    Integer d = new Integer(127);
    System.out.println("Creation of object by using 'new' - "+(c == d));
  }
}
```

**Output:**
```java
Direct assignment of integer literal - true
Creation of object by using 'new' - false
```
