---
layout: post
title:  "Java Annotations"
date:   2015-05-06 1:25:09
categories: java programming
tags: java annotation
---

My notes on java annotations.

# Introduction
Annotation are just a way to give instructions. Instructions can be given at compile/deploy/run time. It allows developers to introduce metadata about methods/classes/variables etc. in code. It is better and much more than a comment. You can access this metadata via code and hence can o amazing stuff on it.

Annotations can have parameters/attributes.

When an annotation just contains a single element named value, you can leave out the element name, and just provide the value.

Sample annotation with attributes:

```java
@Author(
   name = "Mark Twain",
   DOB = "30/11/1835",
   books = {"What is man?", "Letters from Earth"}
)
```

`Author` Annotation definition:

```java
@interface Author {
   String name() default "Kevin";
   String DOB();
   String books[]
}
```

# Built-in Java Annotations
1. **@Deprecated** : used to mark a class, method or field as deprecated. If code uses deprecated classes/methods/fields, the compiler will give you a warning. `@deprecated` JavaDoc symbol is also present to give the reason of the deprecation in the docs.
2. **@Override** : used above methods that override methods in a superclass. If the method does not match a method in the superclass, the compiler will give you an error. It is not necessary in order to override a method in a superclass, but sometimes it helps in identifying case like *when someone changed the name of the overridden method in the superclass, your subclass method would no longer override it*.
3. **@SuppressWarnings** : makes the compiler suppress warnings for a given method.

# Retention Policy
Define annotation scope. Possible values: `SOURCE, CLASS, RUNTIME`. Example:


```java
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation {
    String value() default "";
}
```

# Target
To specify the java element on which the annotation should be available. Possible values `ANNOTATION_TYPE, CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE`.

**ANNOTATION_TYPE**: it means Java annotation definitions. Thus, the annotation can only be used to annotate other annotations. Like the @Target and @Retention annotations.

**TYPE**: it means any type. A type is either a class, interface, enum or annotation.

Example:

```java
@Target({ElementType.METHOD})
public @interface MyAnnotation {
    String value();
}
```

# @Inherited
It signals that a custom Java annotation used in a class should be inherited by subclasses inheriting from that class.

Consider the case:

```java
@Inherited
public @interface MyAnnotation {
}

@MyAnnotation
public class MySuperClass {}

public class MySubClass extends MySuperClass { }
```

In this example the class `MySubClass` inherits the annotation `@MyAnnotation`.

# @Documented
It can be used to signal to the JavaDoc tool that your custom annotation should be visible in the JavaDoc for classes using your custom annotation.

Example:

```java
@Documented
public @interface MyAnnotation {
}

@MyAnnotation
public class MySuperClass { }
```

When generating JavaDoc for the `MySuperClass` class, the `@MyAnnotation` is now included in the JavaDoc.

# When to use annotations

Whenever you want to generalize anything, which you want to do on multiple things but it's not a part of main logic. Some of the examples are:

1. Validation
2. Login based validation
3. Call monitoring
4. Index info
5. Db fieldName info

The one thing you always have to do is, you have to explicit write a handling code somewhere which checks if the annotation are present or not and take appropriate actions. There are multiple ways to do this also:

1. Method annotations:
    - Check on all implementation of a interface
        - Calling checking explicitly at consumer side (todo validation service example).
        - Check on all method calls of a interface via proxy.
    - Check on all method calls of spring beans.
2. Class annotations:
    - Explicit calling checking service
    - Using factory class
3. Variable annotation
    - In bean Factory
    - Explicit calling checking service

# Some Examples:

1. [Login validation](http://engineering.webengage.com/2012/03/12/a-peek-into-webengages-security-layer-super-cool-use-of-java-annotations/)
2. AutowiredSprLogger
3. Annotations to write documentation of your API at runtime.
4. @PreAuthorize
