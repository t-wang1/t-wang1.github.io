---
layout: archive
title: "15-150 / Principles of Functional Programming"
permalink: /notes/functional_programming
date: 2023-11-28
---

These notes are for CMU's 15-150, the introduction functional programming class taught by Brandon Wu. 

### Resources/Links
* [Software Foundations Textbook](https://softwarefoundations.cis.upenn.edu/lf-current/toc.html) 

### Equivalence, Binding, and Scope

Functional programming **avoids modification of state**. It can be characterized by three theses:
1. Recursive problems, recursive solutions
2. Programmatic thinking is mathematical thinking 
3. Types guide structure 

A function is pure if it doesn't have observable side effects, and always returns the same outputs, given the same inputs. 

We use $\Rightarrow$ to denote **stepping/reducing** of expressions, which means to simply an expression by one step. We use $\Rightarrow^* $ to denote the application of the $\Rightarrow$ relation an arbitrary number of times, usually until completion. So the expression $5 * 4 \Rightarrow 20$, and the expression $(2 + 3) * 4 \Rightarrow^* 20$.   

If an expression $e$ eventually reduces down to value $v$, we can say $e \hookrightarrow v$. We then say $e$ is **valuable**. 

A **type** is a specification of the behavior of a piece of code. It predicts what a program is allowed to do. In a **statically typed** language, all typing rules are applied before the program is ever run. 

**Binding** is the act of producing a new association of a value to a variable name. Binding is not assignment. A function only knows about what was in the environment when it was bound. It doesn't see any bindings that happen later. 

```val x : int = 2```  
```fun foo (y : int) : int = x + y```  
```val x : int = 4```  

If we called ```foo 1 ``` the imperative answer would be 5, but the answer SML provides would be 3 since after the first binding of x, we have the environment [2/x]. Future bindings won't change this. 

Two expressions of the same type are said to be **extensionally equivalent** if they evaluate to the same value, both loop forever, or both raise the same kind of exception. For example, $2 + 2 \cong 4 \cong 1 + 3$. Reduction implies equivalence. 

We say that ```f : t1``` $\Rightarrow$ ```t2``` is **total** if for all values ```v : t1```, there is a value ```v' : t2``` such that ```f v ``` $\rhookrightarrow$ ```v'```

### Induction and Recursion 

Recursion is about using the answers to sub-problems to solve a bigger one. Induction is about using the inductive assumption to prove the $n + 1$ case. 

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

A **type constructor** is something which makes a type out of other types. 

### Asymptotic Analysis

**Parallelism** is when a process is able to execute some of its tasks at the same time. When computing in parallel, some tasks have **dependencies**, meaning they cannot be completed until other tasks are finished. 

The **work** of a process is the time expended using a single processor. The **span** of a process is the time expended using infinitely many processors. 

### Higher-Order Functions

A **higher-order function** is a function which takes in functions or returns functions. **Partial application** is the act of applying _some_ of the curried arguments to a curried function, but not all. 

### Combinators and Staging 

Each curried argument represents a specialization of a function. **Staging** describes the act of deliberately placing computations at certain points with respect to receiving curried arguments.

The pipe operator allows us to reverse the order of function application as is defined as ```fun x |> f = f x```. Thus, we could rewrite ```foo ( bar( qux( baz x)))``` as ```x |> baz |> qux |> bar |> foo```.

### Continuation-Passing Style

Instead of piping, we can write CPS functions. Rather than ```f x |> k``` we can do ```f' x``` which is extensionally equivalent to ```k(f x)```

A **continuation** is a function taken in as an argument, which denotes what to do _after_ the current computation. For instance, the declaration ```fun mapCool f L k = map f L |> k``` has the function ```k``` as a continuation, because it passes its return value directly to the continuation function. This is _not_ a continuation passing style function though. 

A function is in **continuation-passing style** if it fulfills the following criteria:
* it takes in and uses continuations
* it makes calls to other functions with continuations (including itself) as tail calls 
* it only calls continuations as tail calls
    * ex. you can't do ```k (x + y) + 2``` since it would constrain the output type of continuation ```k```

