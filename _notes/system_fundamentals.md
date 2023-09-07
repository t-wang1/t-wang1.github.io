---
layout: archive
title: "CSE 220 / System Fundamentals I"
permalink: /notes/system_fundamentals
date: 2023-09-02
author_profile: true
---

These notes for CSE 220: System Fundamentals 1 taught by Dr. Abid Malik

### Machine Representation of Data

For _unsigned integers_ (0, $$\infty$$) there are 2<sup>n</sup> possible combinations for $$n$$ bits. In _fixed precision_ integer arithmetic it's possible for an operation to result in an _overflow_. 

For _signed/magnitude encoding_ the leftmost bit is used as a _sign bit_ (0 = positive, 1 = negative) while the remaining n - 1 bits are used to represent the _magnitude_. There are however a few problems with signed encoding: reduces the range of numbers, there are two separate encodings for 0, and subtraction is somewhat complicated. 

To obtain the negative of a value, find the _complement_ of all the bits. To circumvent the issue of having two representations for 0, we use the _two's complement_. To find the two's complement, complement all the bits to the left of the rightmost 1. Note that end-around carry is only done in one's complement. If you encounter a carry-out in two's complement, discard it. 

_Excess-k/bias-k_ encoding is important in representing real numbers. Integer $$i$$ is represented by the unsigned encoding of $$i+k$$. This preserves the order of positive and negative numbers so that comparing two values is more straightforward.

IEEE 754 is the industry standard way of approximating real numbers. To convert from IEEE 754 to decimal you can use the following formula $$(-1)^s \times 2^{e-bias} \times (1+f)$$ where:
  * $$s$$ is the sign bit (0/1) 
  * $$e$$ is the decimal value of the exponent field
  * $$\text{bias}$$ is 127 for single precision, 1023 for double precision
  * f is the decimal value of the fraction field

