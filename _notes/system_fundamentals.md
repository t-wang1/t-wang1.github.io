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
  * [Declaraing Arrays in MIPS](https://www.cse.unsw.edu.au/~cs1521/17s2/lecs/week06/notes.html)

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
  * $\vert$ bitwise inclusive or 

The shifting operators have the highest precedence followed by ~, &, ^, $\vert$. 

A _pointer_ is a variable that holds an **address** to another variable. The & operator gives the address of an object. The * operator is the _indirection_ or _dereferencing_ operator; when applied to a pointer, it accesses the object the pointer points to. Since C is call by value, meaning it only changes copies of the variables, the only way to alter the variable is to pass pointers to the values to be changed. 

### MIPS 

MIPS is a **reduced instruction set computer (RISC)**, with a small number of simple instructions. Other architectures, like x86, are **complex instruction set computers (CISC)**. Commonly used variables are kept in registers, while less frequently used variables will need to be copied from registers to memory for "safekeeping" when you run out of registers. 

  * A memory read is called a **load** ```(lw)```
    * Ex. ```$lw, $s0, 16($t1)```
      * adds the **offset** (16) to the **base address** ```($t1)```
      * result: ```$s0``` holds the value at address ```($t1 + 16)```
  * A memory write is called a **store** ```(sw)```
    * Ex. ```$sw, $t4, 0x8($0)```
      * adds the offset ```0x8``` to the base address ```($0)```
      * result: writes the value in ```$t4``` to memory address 8

There are 3 instruction formats:
  1. R-Type: register operands
  2. I-Type: immediate operand
  3. J-Type: jump-type instruction 

Helpful Pseudoinstructions: 
  * ```li $v0, 4      # loads 4 into $v0```
  * ```la $a0, str    # loads address of str into $a0```
  * ```move $t1, $t2  # equivalent to add $t1, $t2, $0```
  * ```mul d, s, t    # d = s * t```

There are no if-statements or loops in MIPS. Instead there are **branch** instructions that direct the CPU to execute instructions out of sequential order. There's also the **program counter (PC)** which holds the address of the next instruction to execute. There are **conditional branches** in which the PC is updated if a condition is true and **unconditional branches** in which the PC is changed directly. 

The **callee** is the function being called. The **caller** is the function that calls the callee. S registers are callee saved. T registers are caller saved. The convention is to use the S registers first followed by T registers and finally the stack. 

Steps to call a function in MIPS:
  1. The caller places the parameter values in **argument registers** so the callee can access them
  2. The caller saves the return address and transfers control to the callee by jumping to its label 
  3. The callee allocates/reserves any memory it may need for local variables 
  4. The callee performs the task
  5. The callee places the result in **value registers** where the caller can access it and deallocates any memory
  6. The callee returns control to the caller

Instruction Support for Functions: 
  * To call a function: ```jal function_label```
    * saves PC + 4 in register ```$ra``` so that when the function returns, the next instruction executed is the one immediately following the ```jal``` instruction 
  * To return from a function: ```jr $ra```

The **run-time stack/call stack** is a region of memory used to temporarily save variables during function calls. The top of the stack is a memory address and stored in the ```$sp``` register ("stack pointer"). When a function is called ```$sp``` is saved in the sense that the callee deallocates its stack frame before returning by adding the same amount it substracted to ```$sp``` at the beginning of the function.

A **non-leaf function** is a function which calls another function. Non-leaf functions must save ```$ra``` along with ```$s``` registers before performing the ```jal```. A **leaf function** is a function that doesn't call another function - they should not preserve ```$ra```.

### Performance Problems

##### Question 1

Suppose you have a machine which executes a program consisting of 50% floating point multiplications, 20% floating point divisions, and the remaining 30% are from other instructions. Management wants the machine to run 4 times faster. You can make the divisions run at most 3 times faster and the multiplications run at most 8 times faster. Can you meet management's goal by making only one improvement, and if so, which one? 

1 = 0.5 + 0.3 + 0.2 
If you make division 3x faster: $0.5 + \frac{0.2}{3} + 0.3 = 0.8667 seconds$
$\frac{T_1}{T_2} = \frac{1}{0.8667} = 1.15$
If you make multiplication 8x faster: $\frac{0.5}{8} + 0.2 + 0.3 = 0.567 seconds$
$\frac{T_1}{T_2} = \frac{1}{0.567} = 1.76$

#### Question 2

Suppose that we can improve the floating point instruction performance of a machine by a factor of 15 (the same floating point instructions run 15 times faster on this new machine). What percent of the instructions must be floating point to achieve a speedup of at least 8? 

$\frac{100}{8} = \frac{x}{15} + (100-x)$

#### Question 3

A pipelined processor has a clock rate of 5.5 GHz and executes a program with 5 million instructions. The pipeline has 5 stages, and instructions are issued at a rate of one per clock cycle. Ignore penalties due to the branch instructions, and filling and emptying the pipelines. What is the speedup of the process for this program copared to a none-pipelined processor? 

<details>
  <summary> Answer
  <p> 5

#### Question 4

Computer A has an overall CPI of 1.3 and can be run at a clock rate of 3 GHz. Computer B has a CPI of 2.5 and can be run at a clock rate of 2.2 GHz. We have a particular program we wish to run. When compiled for computer A, this program has exactly 100,000 instructions. How many instructions would the program need to have when compiled for Computer B, in order for the two computers to have exactly the same execution time for this program? 

Machine A = $\frac{I_total \times CPI}{frequency}$
          = $\frac{100,000 \times 1.3}{3 \times 10^9}$ 
Machine B = $\frac{x \times 2.5}{2 \times $10^9}$