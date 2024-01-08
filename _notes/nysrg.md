---
layout: archive
title: "NYSRG / Compilers Compiling Compilers"
permalink: /notes/nysrg
date: 2024-1-7
author_profile: true
---

These notes are an attempt to articulate the ideas that were discussed at NYSRG Week 13. 

## Relevant Material 
* [Reflections on Trusting Trust](https://www.cs.cmu.edu/~rdriley/487/papers/Thompson_1984_ReflectionsonTrustingTrust.pdf)
* [Futamura](https://static.aminer.org/pdf/PDF/001/006/665 partial_evaluation_of_computation_process_an_approach_to_a_compiler.pdf)
* [Essence of Incremental Computation](https://arxiv.org/pdf/2312.07946.pdf)

### Reflections on Trusting Trust

From what I glimmered, Thompson's thesis is that we should remain skeptical of the soundness of computer programs. Specifically, we should not trust a program is free of Trojan horses just because it claims to be. A "Trojan horse" is "any malware that misleads users of its true intent by disguising itself as a standard program." 

In Stage I he presents the idea of a self-reproducing program which, as the name suggests, outputs a copy of itself. In Stage II he introduces the idea of modifying the C compiler to alter how it interprets escape sequences (ex. adding '\v' to represent the vertical tab character). Once you modify the compiler to accept '\v', you're still left with a buggy program because the original compiler doesn't understand what '\v' is. In Stage III he says that an attacker could insert a Trojan horse into the modified compiler and then compile it using a legitimate, unaltered compiler and then propogate it. 

This is all to say, though we prefer to abstract things away in favor of readability it may come at a cost and that sometimes trusting the people who write the software may be more important that believing a program to be free of Trojan horses. 

### Futamura

Off the bat, I mistakened _partial evaluation_ to be _partial application_ which I belatedly realized is not the case, though they're similar concepts. Partial evaluation specializes a computation process with respect to certain inputs. 

$$int(s', r') = \alpha(int, s')(r')$$

Here $s$ represents variables that receive a source program as values and $r$ represents variables that aren't directly influenced by the source program. After the partial evaluation, you're left with a computation process that depends only on $r'$. This should theoretically speed up the compile time. 

$$\alpha(int, s')(r') = \alpha(\alpha, int)(s')(r')$$

After the partial evaluation, $\alpha(\alpha, int)$ can be thought of as a compiler. 

$\alpha$ should have the following properties:

1) $\alpha$ should simplify as much of the computation process $\pi$ as it can using the values provided for partial evaluation variables
2) $\alpha$ should avoid simplifying parts of $\pi$ that won't affect the final result when the generated computation process is eventually evaluated with the remaining variable values

### Essence of Incremental Computation 

Incremental computation aims to improve the efficiency of computations that deal with changing inputs by reusing previously computed results. These can be categorized in three main ways:

1) **Incremental Algorithms**: These are algorithms designed for specific tasks (ex. finding the shortest path)  
2) **Incremental Program-Evaluation Frameworks**: These are frameworks that can adapt to input changes using strategies like memoization and caching  
3) **Incremental Algorithm and Program Derivation Methods**: These methods handle changing inputs by building upon existing solutions like finite differencing and incrementalization

Incremental computation is closely related to partial evaluation, especially when we see how it connects compiliers to interpreters. 

1) $PE(interp, prog)$ yields $interp_(prog)$ which functions like a compiled program for $prog$  
2) $PE(PE, interp)$ yields $PE_(interp)$ which is like a compiler  
3) $PE(PE, PE)$ yields $PE_(PE)$ which is like a compiler generator

### Related Topics 
* Compiler v. Interpreter
* Ahead-of-Time Compilation v. Just-in-Time Compilation
* Partial Evaluation v. Partial Application 
* Automatic Differentiation
* Sketching Algorithms