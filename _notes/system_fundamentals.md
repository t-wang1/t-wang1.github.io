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

To obtain the negative of a value, find the _complement_ of all the bits. To circumvent the issue of having two representations for 0, we use the _two's complement_. To find the two's complement, complement all the bits to the left of the rightmost 1. 