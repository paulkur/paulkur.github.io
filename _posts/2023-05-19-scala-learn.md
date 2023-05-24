---
title: Scala Learn
date: 2023-05-19 10:56:00 +0100
categories: [software,learn,scala]
tags: [software,docs,scala,learn]     # TAG names should always be lowercase
pin: false
---

👩‍💻

## Menu

- [Menu](#menu)
  - [To set up an IntelliJ project properly using Scala, Spark and sbt](#to-set-up-an-intellij-project-properly-using-scala-spark-and-sbt)
  - [Learning](#learning)
    - [create "hello-world" demo](#create-hello-world-demo)

Getting started with Scala 👩‍💻: (Please use Spark 2.4.8, Scala 2.12.10 and JDK1.8)

Useful links:

- If you require an IDE (Integrated Development Environment) we suggest [getting started with IntelliJ Scala](https://docs.scala-lang.org/getting-started/intellij-track/getting-started-with-scala-in-intellij.html)

### To set up an IntelliJ project properly using Scala, Spark and sbt

- Download and install JDK 1.8 from [here](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

If you are on **Windows only** you can follow these optional steps to check you have installed JDK correctly.
**Note:** Software versions are important

- Add JAVA_HOME environment variable [steps here](https://confluence.atlassian.com/doc/setting-the-java_home-variable-in-windows-8895.html)

Check you have installed JDK correctly

```powershell
java -version
```

You should see something similar to this (your version number may not end in 333, but it should start with 1.8.0_):

```powershell
java version "1.8.0_371"
Java(TM) SE Runtime Environment (build 1.8.0_371-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.371-b11, mixed mode)
```

- Install IntelliJ Community Edition [from here](https://www.jetbrains.com/idea/download/) (any version should work)
- [follow this guide](https://docs.scala-lang.org/getting-started/intellij-track/building-a-scala-project-with-intellij-and-sbt.html). **Use Scala 2.12.10 when working through**
- It is likely you will want to use Spark code to read in and write out your data. To add Spark to
your project, you need to add Spark as a dependency. **Please use Spark version 2.4.8 as
specified in the following code snippet**. Add the following to your `build.sbt` in Intellij:

```scala
libraryDependencies ++= Seq(
"org.apache.spark" %% "spark-core" % "2.4.8",
"org.apache.spark" %% "spark-sql" % "2.4.8"
)
```

- To check your setup, hit the little green hammer in the top-right to build your project. Once
it has built, you should see a little green tick in the "Build" window at the bottom of the IDE.
If your build fails please check you have followed each of these steps correctly.

We suggest getting started with IntelliJ Scala. Scala is a compiled programming language, meaning
the code is built before runtime, unlike Python for example. This means a properly set up Scala
project requires a build tool. The most common build tool is sbt.

[Back to Top](#menu)

### Learning

from main sugestions:

- [Martin Odersky, "Working Hard to Keep It Simple"](https://www.youtube.com/watch?v=3jg1AheF4n0&t=371s)
- [Programming in Scala](http://www.artima.com/pins1ed/)
- [Scala Cookbook](http://scalacookbook.com/)
- [Programming in Scala](http://www.artima.com/pins1ed/)
- [Oreilly - Learning Scala](https://www.oreilly.com/library/view/learning-scala/9781449368814/)
- Coursera course on [Functional programming in Scala](https://www.coursera.org/learn/progfun1)

from new found sugestions:

{% include embed/youtube.html id='3jg1AheF4n0&t=371s' %}


#### create "hello-world" demo

```bash
sbt new scala/hello-world.g8
```

```bash
cd <project_name>
sbt test
```

on scala 3

```bash
sbt new scala/scala3.g8
```

[Back to Top](#menu)