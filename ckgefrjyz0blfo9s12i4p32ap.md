## Dynamic typing vs. static typing

Dynamic typing vs. static typing

This topic is provided for reverence only as it explains the differences between dynamic and static typing. Understanding the differences between dynamic and static typing is key to understanding the way in which transformation script errors are handled, and how it is different from the way Groovy handles errors. This will also help you interpret errors created by your transformation script.

Note: It is important to know that the Groovy implementation within Big Data Discovery enforces static typing. For information on exception handling in Transform, which uses a static parser overriding Groovy's dynamic typing behavior, see Exception handling and troubleshooting your scripts.
There are two main differences between dynamic typing and static typing that you should be aware of when writing transformation scripts.

First, dynamically-typed languages perform type checking at runtime, while statically typed languages perform type checking at compile time. This means that scripts written in dynamically-typed languages (like Groovy) can compile even if they contain errors that will prevent the script from running properly (if at all). If a script written in a statically-typed language (such as Java) contains errors, it will fail to compile until the errors have been fixed.

Second, statically-typed languages require you to declare the data types of your variables before you use them, while dynamically-typed languages do not. Consider the two following code examples:

```
// Java example
int num;
num = 5;
// Groovy example
num = 5
```

Both examples do the same thing: create a variable called num and assign it the value 5. The difference lies in the first line of the Java example, `int num;`, which defines num's data type as int. Java is statically-typed, so it expects its variables to be declared before they can be assigned values. Groovy is dynamically-typed and determines its variables' data types based on their values, so this line is not required.

Dynamically-typed languages are more flexible and can save you time and space when writing scripts. However, this can lead to issues at runtime. For example:

```
// Groovy example
number = 5
numbr = (number + 15) / 2  // note the typo
```

The code above should create the variable number with a value of 5, then change its value to 10 by adding 15 to it and dividing it by 2. However, number is misspelled at the beginning of the second line. Because Groovy does not require you to declare your variables, it creates a new variable called `numbr` and assigns it the value number should have. This code will compile just fine, but may produce an error later on when the script tries to do something with `number` assuming its value is 10.

