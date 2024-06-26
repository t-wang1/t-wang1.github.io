---
layout: archive
title: "CSE 216 / Programming Abstractions"
permalink: /notes/programming_abstractions
date: 2023-09-02
---

These notes are for [CSE 216: Programming Abstractions](https://sites.google.com/cs.stonybrook.edu/cse216/) taught by Dr. Ritwik Banerjee and are supplemented by material from NYU's Programming Languages class taught by Dr. Thomas Wies.

### Resources/Links
  * [Folding and Tail Recursion](https://www.cs.cornell.edu/courses/cs3110/2014sp/recitations/5/folding.html)

### Syntax and Parsing

We distinguish between the _syntax_ and _semantics_ of a programming language. The syntax describes the structure of a program while semantics describes a program's meaning. 

We can further distinguish from a language's _concrete syntax_ and _abstract syntax_. The conrete syntax represents which sequences of characters represent programs. The abstract syntax describes the structure of a program as an abstract syntax tree. The _parsing problem_ arises from converting concrete syntax into abstract syntax trees.

### Formal Languages

To describe a language we use _grammar_. A grammar is composed by a set of rules, called _productions_. Productions tell us how the words of the language can be constructed from the symbols in the alphabet. For example, the following grammar describes the language of arithmetic expressions: 

$$
\text{Integer } n ::= \ldots -3 | -2 | -1 | 0 | 1 | 2 | 3 \ldots \\
\text{Binary Operator } \oplus ::= + | - | * | /
$$

$$
\text{Expression } e ::= n | e_1 \oplus e_2
$$

Symbols like $e_1$ and $e_2$ are _nonterminal symbols_ while symbols that are drawn from the alphabet of the language are called _terminal symbols_. 

### Context-Free Languages and Grammars

A _context-free grammar_ is a tuple $G = ({\Sigma}, N, P, S)$ where 

  * ${\Sigma}$ is a finite set of terminal symbols,  
  * N is a finite set of nonterminal symbols disjoint from ${\Sigma}$,
  * P $\subseteq$ (N, (${\Sigma}\cup N$)*)
  * S $\in$ N is the starting symbol.

### Eliminating Ambiguity

We call a grammar in which a word has more than one derivation _ambiguous_.

### Lambda Calculus

It's a language with three constructs:

  * functions, 
  * function applications, and
  * variables

A simple example of a function is $$\lambda x.x$$ where the $$\lambda$$ denotes a function, the first variable is the argument to the function, and the dot is a separator after which is the body of the function. In lambda calculus, a function can only have one input and one output but they can be nested. For example, $$\lambda x.\lambda y.(xy)$$ This is a function that takes in an input $x$, then returns a function which takes an input, $y$, then calls the function $x$ with $y$ as its argument. 

Syntax conventions of lambda calculus:
  1. function application is left-associative: $x y z$ is equivalent to $(x y) z$ and not $x (y z)$
  2. function application has higher precedence than function definition: $$\lambda x. \lambda y.xy$$ is equivalent to $$\lambda x.(\lambda y. (xy))$$

An occurrence of a variable $x$ that immediately follows a $\lambda$ as in $\lambda x.t$ is called a _binding occurrence_. All other occurrences of variables are called _using occurrences_. When a variable is used in an expression under a function with a variable of the same name, then the variable is _bound_. If a variable is not bound, it is _free_. For example, in $$\lambda x.x$$ the inner variable $x$ is bound to the function argument x. In $\lambda x.y$ the inner variable $y$ is free. A term that has no free variables is called _closed_. A _program_ is a closed term.

Formally, a variable $x$ is free in an expression under one of the following conditions:

  1. $x$ is free in $x$
  2. $x$ is free in $$\lambda y.x$$ if the name $$x \neq y$$ and $x$ is free in the expression e
  3. $x$ is free in $$e_1 e_2$$ if $x$ is free in either $$e_1$$ or $$e_2$$

Formally, a variable $x$ is bound under one of the two conditions:

  1. $x$ is bound in $$\lambda y.e$$ if the name $$x = y$$ or $x$ is bound in the expression e
  2. $x$ is bound in $$e_1 e_2$$ if $x$ is bound in either $$e_1$$ or $$e_2$$

Consider the term $t$ given by $(\lambda x.(\lambda y. x(z y)))y$:
  * there's one using occurrence of $z$ which is free so it's free in $t$
  * from left to right, there's one binding occurrence of $x$ and one using occurrence of $x$
    * the using occurrence of $x$ is within the scope of $\lambda x$ so it's bound and not free in $t$
  * from left to right, there's one binding occurrence of $y$ followed by two using occurrences of $y$
    * the leftmost using occurrence of $y$ is bound 
    * the right using occurrence of $y$ is free 
  * since $t$ has free variables, it's not a program

### OCaml

List Functions:

  * ```List.map f [a1; ...; an] = [f a1; ...; f an]```
    * Apply a function ```f``` to each element of a list to transform it
  * ```List.fold_left f a [b1; ...; bn] = f(...(f(f a1 b1) b2)...) bn)```
    * Apply a function ```f``` recursively to the current result together with an element of the list, to finally produce a single element
  * ```List.iter f [a1; ...; an] = begin f a1; ...; f an; () end```
    * Apply a function ```f``` to each element of a list, to produce unit as the result
  * ```List.rev [a1; ...; an] = [an; ...; a1]```
    * Reverses a list 

### Names, Scopes, and Binding

A _name_ is simply a string used to represent something. Names are really important for abstraction because otherwise you would have to refer to things through their addresses. Names for subroutines aid in _control abstraction_ while names for classes aid in _data abstraction_. _Binding_ is the association between a name and the thing it's binding. The time at which the binding is created is known as _binding time_.

Early binding time leads to greater efficiency while later binding time leads to greater flexibility. Things bound before runtime are considered _statically bound_ and are associated with compiled languages; things bound after runtime are considered _dynamically bound_ are are associated with interpreted languages. An object's lifetime and a binding's lifetime may be different. 

The textual region in which a binding is active is its _scope_. Languages where the scope of a binding is determined at compile-time are called _statically/lexically scoped languages_. A variable is said to be _shadowed_ when a variable declared within a certain scope has the same name as a variable declared in an outer scope.

### Data Types

There are multiple ways to think about data types. In the _denotational semantics approach_, we can implicitly picture values from a domain. In the _structural semantics approach_, complex structures are described in terms of simpler constituents. 

A **type system** of a programming language is a set of rules that assigns a data type to the constructs of a program in that language. It provides a set of rules for (a) type equivalence, (b) type compatability, and (c) type inference. **Type checking** is the process of verifying and enforcing the rules of type safety in a program. 

There are two possible approaches for determining type equivalence:
  1. Name equivalence: two types are equal if and only if they have the same constructor expression (ex. they are bound to the same name)
  2. Structural equivalence: two types are equal if and only if they have the same "structure"

Name equivalence can be 
  1. strict (aliased types are considered to be distinct) 
  2. loose (aliased types are considered to be equivalent)

A **derived type/subtype** is **type compatible** with its parent type. This way, we have a one-sided "is-a" relation instead of complete type equivalence. 

The function ```fun x y -> x * y``` has the type ```int -> (int -> int) ```. That is, a function that takes an ```int```, and returns a function of the type ```int -> int```. This type of evaluation of a function that takes multiple arguments as evaluating a sequence of functions, each with a single argument, is called **currying**. 

Types can be divided into scalar and composite types. A _scalar type_ is a type whose values occupy a fixed amount of memory and are atomic: not subdivided further in any way. A _composite type_ is a type whose values are composed of simpler component values. Each component can be scalar or composite. 

A _primitive type_ is a type that is not defined in terms of any other types. An _ordinal type_ is a type where the values belong to a finite range of integers values (i.e. integers, characters, boolean). An _enumeration_ type consists of a set of finite values explicitly enumerated by the programmer. It's considered an ordinal, scalar, and primitive type.

An _algebraic data type_/_variant type_ is a data type representing a value that has multiple possibilities. The syntax to define a variant type is 

$$\text{type t} =  c_1  \text{|}  c_2  \text{| ... |}   c_n$$

where the constant value $c_i$ are called _constructors_. 

The _sum type_ describes a variant that's derived from exactly from one constructor. The _product type_ describes a variant that's derived from a constructor that carries tuples or records; that's to say each component has subvalues. 

### Polymorphism 

There are two types of polymorphism: subtype polymorphism (Java relies heavily on this) and parametric polymorphism (functional PLs rely heavily on this). 

Overriding is associated with _dynamic binding_ since the method that's called is determined at runtime. Overloading is associated with _static binding_ since the compiler looks at the method signatures during compile time. 

Parametric polymorphism allows us to have a generic way of handling values without worrying about their type. This is done through _generic types_ and _generic functions_. 

Casting up along the type hierarchy is known as _upcasting/broadening conversion_. The opposite is known as _downcasting/narrowing conversion_. It's "easier" to upcast than downcast since the "is-a" relation only goes upward; downcasting can't be done implicitly since there's a possibility that the type might not downcast to the right subtype. 

The _diamond problem_ occurs when a method call results in ambiguity from the parent classes; this is only seen in languages that support multiple inheritance. 

### Java Functional Elements and Evaluations 

* The ```stream()``` method converts a traditional iterable object into a ```Stream<T>``` object
* The ```filter()``` method applies a boolean function to every element of a stream, letting through only those elements that evaluate to true 
* The ```map()``` method applies a transformation to every element of a stream 
* The ```reduce()``` method uses an initial value uses an initial value and accumulates elements from the stream as specified by a function that takes in two parameters
