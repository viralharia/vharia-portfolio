---
title: All about Maven
date: '2019-10-21 12:54'
template: "post"
draft: false
slug: "/posts/all-about-maven/"
category: "Random"
tags:
  - "maven"
description:
socialImage: "/media/image-2.jpg"
---
## Maven commands

****
## Maven Wrapper
* The Maven Wrapper is an excellent choice for projects that need a specific version of Maven (or for users that don't want to install Maven at all). Instead of installing many versions of it in the operating system, we can just use the project-specific wrapper script.

### Maven wrapper setup
* First, go to the main folder of the project and run the below command:
```
mvn -N io.takari:maven:wrapper
```
****
* After executing the goal, we'll have more files and directories in the project:
  * mvnw: it's an executable Unix shell script used in place of a fully installed Maven
  * mvnw.cmd: it's the Batch version of the above script
  * mvn: the hidden folder that holds the Maven Wrapper Java library and its properties file
* After that, we can run our goals like this for the Unix system

```
./mvnw clean install

OR for windows:
./mvnw.cmd clean install
```
