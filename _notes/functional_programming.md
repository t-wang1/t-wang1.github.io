---
layout: archive
title: "Functional Programming"
permalink: /notes/functional_programming
date: 2023-11-28
author_profile: true
---

These notes are for CMU's 15-150, the introduction functional programming class taught by Brandon Wu. 

### Structural Induction and Tail Recursion 

Lists can be either $[]$ or $x :: xs$, and _nothing more_. 

Pattern matching is more powerful than conditionals. It lets you see what values really are, rather than simply querying them. It actually produces the goods, as opposed to just making claims. 

A function is **tail recursive** if it makes a single recursive call as the _last thing that it does_, in the recursive case. 

An **accumulator** is an additional argument to a function, which is meant to store the final answer, carrying it forward into future recursive calls. 

### Trees 

SML doesn't have an in-built notion of trees, but we can use a **datatype declaration** to achieve this: ```datatype tree = Empty | Node of tree * int * tree```

