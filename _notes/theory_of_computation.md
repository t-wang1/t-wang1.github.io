layout: archive
title: "CSE 303 / Introduction to The Theory of Computation"
permalink: /notes/theory_of_computation
date: 2024-09-10
---

These notes are for [CSE 303: Introduction to The Theory of Computation](https://www3.cs.stonybrook.edu/~cse303/) taught by Dr. Anita Wasilewska.

### Sets, Relations, and Languages

A binary relation $$R \subseteq A \bigtimes A$$ is an equivalence relation defined in the set A if and only if it's reflexive, symmetric, and transitive.  

Two sets A and B are **equinumerous** denoted **A~B** if and only if there's a bijection function $$f: A \rarr B$$ such that f is 1-1 and onto. A and B have the same cardinality denoted $$|A| = |B|$$ if and only if there is a function $$f: A \rarr B$$ such that f is 1-1 and onto.

A set A is finite if and only if there is $$n \in N and there is a function f: {0, 1, 2,..., n-1} \rarr A such that f is 1-1 and onto. In this case we have $$|A| = n$$. A set A is infinite if it isn't finite. 