---
layout: post
title:  "Selecting Java Logger"
date:   2015-06-08 12:54:00
categories: programming java logging
tags: java logger programming slf4j "java logging api" log logging "apache commons logging" 
---

I am creating a new java project and need to set up its logging. In this post I will cover the steps I take to choose a logger.

## Introduction:

On first push to our mind, we could only think of following things a logger has to do:

1. Take any log message and write it to some files.
2. Also support different level of logging like debug, severe/fatal, error etc.

But there are many more things a logger has to do. [Key Features and Concepts of Java Logging Frameworks](http://www.java-logging.com/concepts/) has covered all of them in a very good way. Most of those things should be clear before going any further.

### Which implementation to  choose

When you start setting up logging, the first question comes to your mind is which logger library to use. There are many libraries available:

1. [log4j](http://logging.apache.org/log4j/2.x/)
2. [logback](http://logback.qos.ch/)
3. [jdk-logger](https://docs.oracle.com/javase/7/docs/api/java/util/logging/Logger.html)
4. [SmartInspect](http://www.gurock.com/smartinspect/)
5. [ObjectGuy](http://www.theobjectguy.com/javalog/)

A simple comparison is covered in [Comparison of Java Logging Frameworks and Libraries](http://www.java-logging.com/comparison/) and [Cover Story: Log4j vs java.util.logging](http://java.sys-con.com/node/48541)

### What if there is a broker in between

The simple solution is to use a facade or magic interface layer over multiple implementation of logger. But life is not that simple now there for facade there are multiple options too:

1. [Apache Commons Logging](http://commons.apache.org/proper/commons-logging/)
2. [Simple Logging Facade for Java](http://www.slf4j.org/)

With both these facades you can specify which implementation to use at runtime.

## commons-logging vs slf4j

![Class Hierarchy]({{ site.url }}assets/selecting-java-logger/commons-slf4j-hierarchy.png)

Slf4j does compile-time (dynamic) binding while commons does runtime(static) binding ([Dynamic v/s Static binding](http://geekexplains.blogspot.in/2008/06/dynamic-binding-vs-static-binding-in.html)). How can a java library which can log using different frameworks rely on compile time binding?  The answer is  that the *compile-time* binding only refers to the fact that SLF4J is compiled against an implementation of an SLF4J logger. However, you can still use a different binding at runtime. SLF4J doesn't use *classloaders*, instead, its very simple:  It loads `org.slf4j.impl.StaticLoggerBinder`.  Each implementation of SLF4J (i.e. the slf4j-log4j bindings) provides a class with this exact name. So there is no confusion.  At run-time, the same thing happens: The class is picked up from the classpath directly, without any runtime magic.  What if no slf4j-* implementations are on the classpath?  Well, then no logging will occur.

There are some issues with commons-logging which are very well covered in [What is the issue with the runtime discovery algorithm of Apache Commons Logging](http://stackoverflow.com/questions/3222895/what-is-the-issue-with-the-runtime-discovery-algorithm-of-apache-commons-logging). Shamelessly copying the summary:

1. Yes, JCL is known to be broken, better stay away from it.
2. If you want to use a logging facade (not all projects need that), use SLF4J.
3. SLF4J provides a JCL-to-SLF4J bridge for frameworks still using JCL like Spring :(
4. I find Logback, Log4J's successor, to be a superior logging implementation.
5. Logback natively implements the SLF4J API. This means that if you are using Logback, are actually using the SLF4J API.

If you really want to go in deep and eat your brains out then just read [Think again before adopting the commons-logging API](http://articles.qos.ch/thinkAgain.html), no questions asked.

## Which implementation to choose in slf4j


## Setting up slf4j:
The setup steps are very well defined in the slf4j docs: [SLF4J user manual](http://www.slf4j.org/manual.html).

Summary:

1. [Download](http://www.slf4j.org/download.html) slf4j distribution and add `slf4j-api-1.7.12.jar` to your classpath.
2. Compile and run sample example:

    ```
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    public class HelloWorld {
      public static void main(String[] args) {
        Logger logger = LoggerFactory.getLogger(HelloWorld.class);
        logger.info("Hello World");
      }
    }
    ```
3. Sample output

    ```
    SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
    SLF4J: Defaulting to no-operation (NOP) logger implementation
    SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
    ```
4. This warning is printed because no slf4j binding could be found on your class path. The warning will disappear as soon as you add a [binding](http://www.slf4j.org/manual.html#swapping) to your class path.
5. I tried using log4j. On testing I got following error.

    ```
    /Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/bin/java -Didea.launcher.port=7533 "-Didea.launcher.bin.path=/Applications/IntelliJ IDEA 14.app/Contents/bin" -Dfile.encoding=UTF-8 -classpath "/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/lib/ant-javafx.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/lib/dt.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/lib/javafx-doclet.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/lib/javafx-mx.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/lib/jconsole.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/lib/sa-jdi.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/lib/tools.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/charsets.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/deploy.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/htmlconverter.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/javaws.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/jce.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/jfr.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/jfxrt.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/jsse.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/management-agent.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/plugin.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/resources.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/rt.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/ext/dnsns.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/ext/localedata.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/ext/sunec.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/ext/sunjce_provider.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/ext/sunpkcs11.jar:/Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Home/jre/lib/ext/zipfs.jar:/Users/agupta/Projects/Sprinklr-clone/out/production/Sprinklr-clone:/Users/agupta/Downloads/slf4j-1.7.12/slf4j-api-1.7.12.jar:/Users/agupta/Downloads/slf4j-1.7.12/slf4j-log4j12-1.7.12.jar:/Applications/IntelliJ IDEA 14.app/Contents/lib/idea_rt.jar" com.intellij.rt.execution.application.AppMain Test
    Failed to instantiate SLF4J LoggerFactory
    Reported exception:
    java.lang.NoClassDefFoundError: org/apache/log4j/Level
        at org.slf4j.LoggerFactory.bind(LoggerFactory.java:141)
        at org.slf4j.LoggerFactory.performInitialization(LoggerFactory.java:120)
        at org.slf4j.LoggerFactory.getILoggerFactory(LoggerFactory.java:331)
        at org.slf4j.LoggerFactory.getLogger(LoggerFactory.java:283)
        at org.slf4j.LoggerFactory.getLogger(LoggerFactory.java:304)
        at Test.main(Test.java:6)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)

    ```
6. This error is coming because till not I have only added `slf4j-api-1.7.12.jar` and `slf4j-log4j12-1.7.12`. I still need to add `log4j` lib as explained in on [SO](http://stackoverflow.com/questions/10585656/java-lang-classnotfoundexception-org-apache-log4j-level).
7. After adding `log4j-1.2.17`, I got the following output:

    ```
    log4j:WARN No appenders could be found for logger (Test).
    log4j:WARN Please initialize the log4j system properly.
    log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
    ```
8. We need to add `log4j.properties` in resources folder.
9. As mentioned in a lot of places `logback` should be the one which one should use with `slf4j` as it natively implements `slf4j`, so no need to include any bindings. Then how will slf4j knows which logger implementation to use? May be it check for logback by default, lets run it and see.
10. Now my libs contain `slf4j-api-1.7.12.jar`, `logback-classic-1.1.3` and `logback-core-1.1.3.jar`.
11. On running I got the following output.

```
15:32:58.537 [main] INFO  Test - Hello World
```

It worked fine. Our assumption about no need to find the binding is correct which can be confirmed from slf4j binding architecture:

![slf4j binding architecture]({{ site.url }}assets/selecting-java-logger/concrete-bindings.png)


## Logback

Till now its clear that I will be using logback with slf4j. There are many things we need to learn about logback to configure it properly. I will be starting a complete new post regarding logback in which I will keep updating anything I will learn about logback while using it.

## Sample project:

![Here]({{ site.url }}assets/selecting-java-logger/sprinklr-clone.zip) you can download the sample intellij project in which all the things till now are set-up.

## References:

1. [Java Logging Tools and Frameworks](http://www.java-logging.com/)
2. [This is why everyone uses SLF4J nowadays](http://jayunit100.blogspot.in/2013/10/simplifying-distinction-between-sl4j.html)
3. [What is the issue with the runtime discovery algorithm of Apache Commons Logging](http://stackoverflow.com/questions/3222895/what-is-the-issue-with-the-runtime-discovery-algorithm-of-apache-commons-logging)
4. [Think again before adopting the commons-logging API](http://articles.qos.ch/thinkAgain.html)
5. [Difference between Dynamic Binding & Static Binding in Java](http://geekexplains.blogspot.in/2008/06/dynamic-binding-vs-static-binding-in.html)
6. [SLF4J user manual](http://www.slf4j.org/manual.html)