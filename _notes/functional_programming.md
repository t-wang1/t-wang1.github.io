---
layout: archive
title: "15-150 / Principles of Functional Programming"
permalink: /notes/functional_programming
date: 2023-11-28
author_profile: true
---

These notes are for CMU's 15-150, the introduction functional programming class taught by Brandon Wu. 

###

Functional programming **avoids modification of state**. It can be characterized by three theses:
1. Recursive problems, recursive solutions
2. Programmatic thinking is mathematical thinking 
3. Types guide structure 

A function is pure if it doesn't have observable side effects, and always returns the same outputs, given the same inputs. 

We use $\Rightarrow$ to denote **stepping/reducing** of expressions, which means to simply an expression by one step. We use $\Rightarrow^* $ to denote the application of the $\Rightarrow$ relation an arbitrary number of times, usually until completion. So the expression $5 * 4 \Rightarrow 20$, and the expression $(2 + 3) * 4 \Rightarrow^* 20$  



### Structural Induction and Tail Recursion 

Lists can be either $[]$ or $x :: xs$, and _nothing more_. 

Pattern matching is more powerful than conditionals. It lets you see what values really are, rather than simply querying them. It actually produces the goods, as opposed to just making claims. 

A function is **tail recursive** if it makes a single recursive call as the _last thing that it does_, in the recursive case. 

An **accumulator** is an additional argument to a function, which is meant to store the final answer, carrying it forward into future recursive calls. 

### Trees 

SML doesn't have an in-built notion of trees, but we can use a **datatype declaration** to achieve this: ```datatype tree = Empty | Node of tree * int * tree```

The principle of **structural induction** on trees is as follows: 

Let $P$ be a theorem on values ```v : tree```. We would like to show that, for all ```v : tree```, $P(v)$ holds. 

It suffices to show that:
* $P(Empty)$ holds 
* Assuming that $P(L)$ and $P(R)$ hold for some ```L, R : tree```, show that $P(Node(L, x, R))$ holds for an arbitrary ```x : int```. 