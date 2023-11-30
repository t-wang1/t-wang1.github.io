---
layout: archive
title: "CSE 220 / System Fundamentals I"
permalink: /notes/system_fundamentals
date: 2023-09-02
author_profile: true
---

These notes for CSE 220: System Fundamentals 1 taught by Dr. Abid Malik

### Resources/Links
  * [Converting Between Decimal and Binary Floating Point Numbers](https://kyledewey.github.io/comp122-fall17/lecture/week_2/floating_point_interconversions.html)

### Machine Representation of Data

For _unsigned integers_ (0, $$\infty$$) there are 2<sup>n</sup> possible combinations for $$n$$ bits. In _fixed precision_ integer arithmetic it's possible for an operation to result in an _overflow_. 

For _signed/magnitude encoding_ the leftmost bit is used as a _sign bit_ (0 = positive, 1 = negative) while the remaining n - 1 bits are used to represent the _magnitude_. There are however a few problems with signed encoding: reduces the range of numbers, there are two separate encodings for 0, and subtraction is somewhat complicated. 

To obtain the negative of a value, find the _complement_ of all the bits. To circumvent the issue of having two representations for 0, we use the _two's complement_. To find the two's complement, complement all the bits to the left of the rightmost 1. Note that end-around carry is only done in one's complement. If you encounter a carry-out in two's complement, discard it. 

_Excess-k/bias-k_ encoding is important in representing real numbers. Integer $$i$$ is represented by the unsigned encoding of $$i+k$$. This preserves the order of positive and negative numbers so that comparing two values is more straightforward.

IEEE 754 is the industry standard way of approximating real numbers. There are typically three parts:
  * sign bit: 0 (positive) or 1 (negative)
  * exponent: stored in excess-127 for 32-bit version and excess-1023 for 64-bit version
  * fraction/mantissa: contains the digits to the right of the binary point (23 bits for 32-bit version and 52 bits for 64-bit version)

To convert from IEEE 754 to decimal you can use the following formula $$(-1)^s \times 2^{e-bias} \times (1+f)$$ where:
  * $$s$$ is the sign bit (0/1) 
  * $$e$$ is the decimal value of the exponent field
  * $$\text{bias}$$ is 127 for single precision, 1023 for double precision
  * f is the decimal value of the fraction field

### C Programming 

C doesn't define the order in which subexpressions are evaluated except those that include the logical and, or, conditional, or comma operator. For example, in the expression $$ (a + b) * (c - d)$$ either subexpression could be evaluated first. It also follows the rule that an else clause belongs to the nearest if statement that hasn't already been paired with an else. 

### Bitwise Shift Operators

Bitwise operators shift the bits in an integer to the left or right: 
  * $$i << j$$ shifts the bits in $$i$$ left by $$j$$ places
    * for each bit that is shifted off the left end, a zero bit enters at the right
  * $$i >> j$$ shifts the bits in $$i$$ right by $$j$$ places
    * if $$i$$ is unsigned/non-negative, zeros are added to the left as needed
    * if $$i$$ is negative, the result is implementation defined 

To modify a variable by shifting its bits, use the compound assignment operators <<= and >>=. There are four additional bitwise operators:
  * ~ bitwise complement
  * ^ bitwise exclusive or 
  * & bitwise and 
  * | bitwise inclusive or 

The shifting operators have the highest precedence followed by ~, &, ^, |. 

A _pointer_ is a variable that holds an **address** to another variable. The & operator gives the address of an object. The * operator is the _indirection_ or _dereferencing_ operator; when applied to a pointer, it accesses the object the pointer points to. Since C is call by value, meaning it only changes copies of the variables, the only way to alter the variable is to pass pointers to the values to be changed. 

### MIPS 

MIPS is a **reduced instruction set computer (RISC)**, with a small number of simple instructions. Other architectures, like x86, are **complex instruction set computers (CISC)**. Commonly used variables are kept in registers, while less frequently used variables will need to be copied from registers to memory for "safekeeping" when you run out of registers. 

  * A memory read is called a **load** $(lw)$
    * Ex. ```$lw, $s0, 16($t1)```
  * A memory write is called a **store** $(sw)$
    * 