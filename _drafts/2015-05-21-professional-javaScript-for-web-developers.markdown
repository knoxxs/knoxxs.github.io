---
layout: post
title:  "Professional JavaScript for Web Developers - Notes"
date:   2015-05-21 6:27:14
categories: javascript programming notes
tags: wrox js javascript notes
---

These are my notes for book Professional JavaScript® for Web Developers, Third Edition.

# NoScript
it was created to provide alternate content for browsers without JavaScript. This element can contain any HTML elements, aside from `<script>`, that can be included in the document `<body>`. Any content contained in a `<noscript>` element will be displayed under only the following two circumstances:
- The browser doesn't support scripting.
- The browser’s scripting support is turned off.

Example:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Example HTML Page</title>
    <script type=”text/javascript” defer=”defer” src=”example1.js”></script>
    <script type=”text/javascript” defer=”defer” src=”example2.js”></script>
</head>
<body>
<noscript>
    <p>This page requires a JavaScript-enabled browser.</p>
</noscript>
</body>
</html>
```

## Basics

### Statement
Even though a semicolon is not required at the end of statements, it is recommended to always include one.

### Variables

- Loosely typed
- Default value undefined
- Recommended to treat variables as typed
- `var` keywords make variables local scoped.

### Data Types
There are five simple data types (also called primitive types) in ECMAScript: Undefined, Null, Boolean, Number, and String. There is also one complex data type called Object, which is an unordered list of name-value pairs. Because there is no way to define your own data types in ECMAScript, all values can be represented as one of these six.

#### typeof
Because ECMAScript is loosely typed, there needs to be a way to determine the data type of a given variable. Possible values:

- “undefined” if the value is undefined or variable is not declared
- “boolean” if the value is a Boolean
- “string” if the value is a string
- “number” if the value is a number
- “object” if the value is an object (other than a function) or null
- “function” if the value is a function

#### undefined
The Undefined type has only one value, which is the special value undefined. Even though uninitialized variables are automatically assigned a value of undefined, it is advisable to always initialize variables. That way, when typeof returns "undefined", you’ll know that it’s because a given variable hasn'’'t been declared rather than was simply not initialized.

#### Null
The Null type is the second data type that has only one value: the special value null. Logically, a null value is an empty object pointer. The value undefined is a derivative of null, so ECMA-262 defines them to be superficially equal as follows: `null == undefined; //true`

#### Boolean
It has only two literal values: true and false. These values are distinct from numeric values, so true is not equal to 1, and false is not equal to 0. Though there are just two literal Boolean values, all types of values have Boolean equivalents in ECMAScript.

![Boolean Values]({{ site.url }}assets/professional-javaScript-for-web-developers/booleanValues.png)

#### Number
It uses the `IEEE-754` format to represent both integers and floating-point values (also called double-precision values in some languages). Integers can also be represented as either octal (base 8) or hexadecimal (base 16) literals. For an octal literal, the first digit must be a zero (0) followed by a sequence of octal digits (numbers 0 through 7). If a number out of this range is detected in the literal, then the leading zero is ignored and the number is treated as a decimal.

**Floating Point**:

- To define a floating-point value, you must include a decimal point and at least one number after the decimal point. Because storing floating-point values uses twice as much memory as storing integer values, ECMAScript always looks for ways to convert values into integers. When there is no digit after the decimal point, the number becomes an integer. Likewise, if the number being represented is a whole number (such as 1.0), it will be converted into an integer.
- Floating-point values are accurate up to 17 decimal places but are far less accurate in arithmetic computations than whole numbers. For instance, `adding 0.1 and 0.2 yields 0.30000000000000004 instead of 0.3`. It’s important to understand that rounding errors are a side effect of the way floating-point arithmetic is done in `IEEE-754–based` numbers and is not unique to ECMAScript. Other languages that use the same format have the same issues.

**Range of values**:

- Not all numbers in the world can be represented in ECMAScript, because of memory constraints. The smallest number that can be represented in ECMAScript is stored in `Number.MIN_VALUE(5e-324)`and the largest number is stored in `Number.MAX_VALUE (1.7976931348623157e+308)`.
- If a calculation results in a number that cannot be represented by JavaScript’s numeric range, the number automatically gets the special value of `Infinity`. Any negative number that can’t be represented is `–Infinity(Number.NEGATIVE_INFINITY )` (negative infinity), and any positive number that can’t be represented is simply `Infinity(Number.POSITIVE_INFINITY)` (positive infinity).
- If a calculation returns either positive or negative Infinity, that value cannot be used in any further calculations, because Infinity has no numeric representation with which to calculate.
- To determine if a value is finite (that is, it occurs between the minimum and the maximum), there is the `isFinite()` function.

**NaN**:

- There is a special numeric value called NaN, short for Not a Number, which is used to indicate when an operation intended to return a number has failed (as opposed to throwing an error). For example, dividing any number by 0 typically causes an error in other programming languages, halting code execution. In ECMAScript, dividing a number by 0 returns NaN, which allows other processing to continue.
- Any operation involving NaN always returns NaN (for instance, NaN /10), which can be problematic in the case of multi-step computations.
- NaN is not equal to any value, including NaN i.e. `NaN == NaN //false`
- ECMAScript provides the `isNaN()` function. When a value is passed into isNaN(), an attempt is made to convert it into a number. Example:

    ```javascript
    isNaN(NaN);    //true
    isNaN(10);     //false - 10 is a number
    isNaN("10");   //false - can be converted to number 10
    isNaN("blue"); //true - cannot be converted to a number
    isNaN(true);   //false - can be converted to number 1
    ```
- Although typically not done, isNaN() can be applied to objects. In that case, the object’s valueOf() method is first called to determine if the returned value can be converted into a number. If not, the toString() method is called and its returned value is tested as well.

**Number Conversions**:

