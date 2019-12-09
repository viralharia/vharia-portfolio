---
title: NodeJs for Java developer
date: '2019-10-23 16:17'
template: "post"
draft: false
slug: "/posts/node-js-for-java-developer/"
category: "Random"
tags:
  - "java"
  - "nodejs"
description:
socialImage: "/media/image-2.jpg"
---
## Runtime environment
Runtime environment is an environment, which supports the execution of a program/process. 

A program, for being able to execute, requires runtime environment. 

Runtime environment provides following services to the program/process :-

 + Resident Memory
 + Resources such as File, Sockets, etc.
 + Environment variables
 + Proper initialization
 + Proper disposal.
****
## Java runtime && Node.js runtime
* Just like we have JRE in java, there is a node.js runtime. Node.js is not a framework or language.
**Java**

JRE = JVM (GC, JIT compiler, Interpreter, Class Loader, Thread Manager, Exception Handler) + class libraries (rt.jar) + other supporting files

**Node.js**

Node.js = Javascript virtual machine (V8 javascript engine) + libraries (Node API or Node modules)

****
## Execution
**Java**

Bytecode is given to the JVM for execution on a given target platform. The JVM converts the Bytecode into machine code specific to the target platform before execution.

**Node.js**

There is no intermediate code generated before giving it to JsVM, the V8 engine. The JsVM takes the javascript source code directly and compiles it to machine code specific to the given target platform for execution.

****
## Web App architecture
**Java**

Client(browser) --> Apache HTTP server --> Web container (Java Servlet implementation) (Tomcat, jetty) on alinux/windows server --> Database

**Node.js**

Client(browser) --> Nginx proxy server --> Node.js runtime on a linux/windows  server--> Database
++++++++++++++++++++++++++++++++++++++++++
Reference - [https://dzone.com/articles/what-is-nodejs-for-java-developers]()
