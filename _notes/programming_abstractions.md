---
layout: archive
title: "CSE 216 / Programming Abstractions"
permalink: /notes/programming_abstractions
date: 2023-09-02
author_profile: true
---

These notes are for [CSE 216: Programming Abstractions](https://sites.google.com/cs.stonybrook.edu/cse216/) taught by Dr. Ritwik Banerjee and are supplemented by material from NYU's Programming Languages class taught by Dr. Thomas Wies.

### Syntax and Parsing

We distinguish between the _syntax_ and _semantics_ of a programming language. The syntax describes the structure of a program while semantics describes a program's meaning. 

We can further distinguish from a language's _concrete syntax_ and _abstract syntax_. The conrete syntax represents which sequences of characters represent programs. The abstract syntax describes the structure of a program as an abstract syntax tree. The _parsing problem_ arises from converting concrete syntax into abstract syntax trees.

### Formal Languages

To describe a language we use _grammar_. A grammar is composed by a set of rules, called _productions_. Productions tell us how the words of the language can be constructed from the symbols in the alphabet. For example, the following grammar describes the language of arithmetic expressions: 

$$Integer n ::= ... -3 | -2 | -1 | 0 | 1 | 2 | 3 ...$$
$$Binary Operator \oplus ::= + | - | * | /$$
$$Expression e ::= n | e<sub>1</sub> \oplus e<sub>2</sub>$$
