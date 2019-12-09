---
title: Understand JVM Loading, JVM Linking, and JVM Initialization
date: '2019-11-05 19:39'
template: "post"
draft: false
slug: "/posts/understand-jvm-loading-jvm-linking-and-jvm-initialization/"
category: "Java"
tags:
  - "java"
  - "jvm"
description:
socialImage: "/media/image-2.jpg"
---
## Overview
* The `loading`, `linking`, and `initialization` are the initial processes that JVM commences as soon as a byte code, called the class file, is loaded into JVM for execution. 
* Other processes—such as `instantiation`, `garbage collection`, and `finalization` — occur at the middle stages of the lifetime of the class life cycle. 
* And finally, the process of `unloading` occurs at the end of the life cycle. 
* JVM provides the environment where different processes take their course. It absolutely does not matter what language compiler is used to convert the source code in to a class file as long as it adheres to the standard that JVM understands.  Apart from Java, there are many well-known JVM languages, such as Clojure, Groovy, Scala, Jruby, Jython, and so forth. A program may be written in any of these languages and compiled by the specific language compiler. The compiled target code is created in a manner to run on JVM.

## `Loading`, `Linking`, and `Initialization`
* There are three processes that JVM follows at the initial stages of importing a class file into its domain. These three processes are called `loading`, `linking`, and `initialization`.

### The Process of Loading 
* As per the Java 8 Virtual Machine Specification, it is the process of finding the binary representation of a class or interface type with a particular name and creating a class or interface from that binary representation.  
* JVM provides two types of class loaders. One is called **_bootstrap class loader_** and another is the **_user-defined class loader._**
* The bootstrap class loader is rigidly defined in the JVM and loads class files according to the specification. The user-defined class loader is open for vendor-specific implementation and can custom load classes via the `java.lang.Class` instance.

* Therefore, in a nutshell, the loading process basically performs these three functions:  
  - Create a binary stream of data from the class file 
  - Parse the binary data according to the internal data structure                   
  - Create an instance of java.lang.Class

### The Process of Linking 
* As per Java 8 Virtual Machine Specification, it is the process of taking a class or interface and combining it into the run-time state of the JVM so that it can be executed

* The linking begins with the process of `verification` of the class, ensuring that it adheres to the semantics of the language and does not disrupt the integrity of JVM.

* Once the `verification` is done, JVM allocates memory for the class variables and initializes them to default values according to the type of the variable. The actual initialization (with user-defined initialization values), however, does not occur until the next initialization phase. This process is called `Preparation`.

* Finally, in the optional `Resolution` phase, JVM locates classes, interfaces, fields, and methods referenced in the constant pool (symbol table) and determines the concrete values from their symbolic reference.

* Therefore, in a nutshell, the linking process involves three functions:  
  - Verification 
  - Preparation 
  - Resolution (optional)

### The Process of Initialization 
* As per Java 8 Virtual Machine Specification, initialization of a class or interface consists of executing its class or interface initialization method.

* After the class or interface is linked through the process of `verification`, `preparing`, and optionally `resolving`, the `initialization` phase makes the class ready for its first active use. The process starts with initializing the class variables with the value that the program is expected to start off.

* Therefore, to summarize, the initialization process involves the following two functions:  
  - Initialize class variables with the routine specified by the programmer. 
  - Initialize its super classes if it is not already initialized.
