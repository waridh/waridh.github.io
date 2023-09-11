---
title: "Introduction to Java"
topic: ece_325
---

## Setting up the environment

*Quick note*:

- Install a JDK on your platform, (most are supported)

## Running your first Java program

1. Create a file with a name followed by a `*.java` extension.
2. In this file, you must declare a class with the same name as the extension. Here is an example snippet for hello world, where the name of the file is `HelloWorld.java`.

```java
class HelloWorld {
    // Your program begins with a call to main().
    // Prints "Hello, World" to the terminal window.
    public static void main(String args[])
    {
        System.out.println("Hello, World");
    }
}
```

Every java file needs to have at least one class that has the same name as the file name.

## Java primitives

There are 8 java primitives, and they are showcased in the following table.

| Type | Size (bits) | minimum value | maximum value | examples |
| --- | --- | --- | --- | --- |
| Byte | 8 | $$-128$$ | $$127$$ | byte b = 100; |
| short | 16 | $$-2^{15}$$ | $$ 2^{15}-1$$ | short b = 10000; |
| integer | 32 | $$-2^{31}$$ | $$ 2^{31} -1$$ | int b = 100000000; |
| long | 64 | $$-2^{63}$$ | $$ 2^{63} - 1$$ | long b = 100_000_000_000L; |
| float | 32 | $$-2^-149$$ | $$\left(2-2^{-23}\right)\times2^{127}$$ | float f = 1.456f; |
| double | 64 | $$-2^{-1074}$$ | $$\left(2-2^{-52}\times2^{1023}\right)$$ | double f = 1.4523190 |
| char | 16 | $$0$$ | $$2^{16} - 1$$ | char c = 'c'; // UTF-16 |
| boolean | 1 | - | - | boolean b = true;

The implementation is in twos complement. This means that if the result of an operation is larger than the finite set, the value is going to invert around to the smallest representation.

## Type casting

When it comes to java, there are two types of casting.

- Widening casting - Happens implicitly on variable assignment on a variable of a greater sized type. Here is an example: `double b = 1.587f`
- Narrowing casting - Has to be specified by the user, as it can be a lossy transformation. Here is an example: `int a = (int) 10_000_000L`

Most java operations are 32-bit operations, so here are some interactions that happens on java.

- When working with a binary operator on integral types, the smaller type gets casted to the larger one before the operation.
- On the integral binary operations, if the type being operated on is smaller than 32-bits, they are widened to 32-bits before the operation is done on them.
- When there is an operation between a floating type and an integral type, the integral type gets converted into a floating type.

## Types of errors

In a basic scope, there are three types of errors.

- Compile-time errors
  - Errors that are detected by the compiler, which will halt the compilation. Something like a syntax error is a compile-time error.
- Run-time errors
  - Errors that are not detected by the compiler, but will present itself in run-time, and usually throw an exception.
- Logical errors
  - Functional errors caused by the designer implementing the system incorrectly. This causes the program to not match the design constraint.

## Anatomy of a java method

```java
class HelloWorld {
    // Your program begins with a call to main().
    // Prints "Hello, World" to the terminal window.
    public static void main(String args[])
    {
        System.out.println("Hello, World");
    }
}
```

- `public`/`protected`/`private` - Describes the restriction of the method.
- `static` - If the keyword is included, then the method or attribute will be accessible by all objects of the class. Like a global variable.
- `void` - This is just the return type of the method.
