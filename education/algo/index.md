---
layout: page
title: Algorithm Design and Analysis
permalink: /education/algo/
keywords: 01:196:344, rutgers, software, engineering, algorithms, design, analysis, big, O, notation, runtime
description: A page full of notes and useful links for succeeding in Algorithms Design & Analysis (Algo) at Rutgers University.
mathjax: true
---

These notes are for a version of the course taught by Prof. Bahman Kalantari in the spring of 2017.

Professor's Website - [http://www.cs.rutgers.edu/~kalantar/](http://www.cs.rutgers.edu/~kalantar/)

Grading Policy:

- Homework & Quizzes: 15%
- Midterm I & II: 20% each
- Final Exam: 45%

Exam 1 Date: Feb. 15th, 2017

## Lecture 1 - 1/18/2017

The aim of this course is to study algorithms - routines or sets of steps to solve problems.

In this course we wish to analyze the runtime and memory efficiency of algorithms. We will explore how to best go about designing algorithms which have optimal runtimes.


## Lecture 2 1/23/2017

In this lecture we explore the growth of functions and some asymptotic notation.

Given two functions which map natural numbers to all real numbers

$$f, g: N \rightarrow R_{\geq 0}$$

We write 

$$f(n) = O(g(n))$$ 

$$\text{iff }\exists\ c > 0\  \text{s.t.}\ f(n) \leq c\cdot g(n),\ \forall n \geq n_0, n_0 \in N$$

To put that into english we can that that, if and only if (double implication) there exists some constant $$c$$ which is greater than 0 such that the condition that $$f(n) \leq c\cdot g(n)$$ is satisfied for every $$n$$ which is greater than some reasonable $$n_0$$ then we can say that $$f(n) = O(g(n))$$. (f is big-oh of g)

Example: $$f(n) = n, g(n) = n^2$$, Our restriction is that $$n_0$$ is 1

**Q**: In this case, is $$f(n) =O(g(n))$$?

**A**: Yes, if we set $$c = 1$$ then we get $$n < 1\cdot n^2$$ which is true for any $$n$$ which is greater than or equal to 1.

There are two other definitions similar to big-oh as well.


**Definition 2**

$$f(n) = \Omega (g(n))$$

$$\text{iff }\exists\ c > 0\  \text{s.t.}\ f(n) \geq c\cdot g(n),\ \forall n \geq n_0, n_0 \in N$$

**Definition 3**

$$f(n) = \Theta (g(n))$$

$$\text{iff }\exists\ c_1, c_2 > 0\  \text{s.t.}\ c_1g(n) \leq f(n) \leq c_2\cdot g(n),\ \forall n \geq n_0, n_0 \in N$$

**Definition 4**

$$f(n) = o(g(n))$$

(Little-Oh)

$$\text{iff } \lim\limits_{n\rightarrow\infty} \frac{f(n)}{g(n)} = 0$$

**Review - The L'Hopital Rule**

Given two functions such that $$\lim\limits_{n\rightarrow\infty} f(n) = \lim\limits_{n\rightarrow\infty} g(n) = \infty$$ then we can also say that

$$\lim\limits_{n\rightarrow\infty} \frac{f(n)}{g(n)} = \lim\limits_{n\rightarrow\infty} \frac{f'(n)}{g'(n)}$$

**Theorem**

Suppose two functions: $$log(n)$$ and $$n^\alpha, \alpha > 0$$. Our claim is that

$$log(n) = o(n^\alpha), \alpha > 0 \\ n^k = o(2^n), k > 0$$

Also suppose we look at functions that are exponential in nature (vs polynomial functions). Given the two functions $$n^k$$ and $$2^n$$

$$\frac{d}{dn} (n^k) = kn^{k-1}$$ Then if we continue to differentiate we find that we get $$k!$$.

If we are then to continuously differentiate our other function $$2^n$$ we get $$log(2)^k 2^n$$

Then if we are to take the limit of the ratio (now that we have applied L'Hopital's rule) we get

$$\lim\limits_{n\rightarrow\infty} \frac{k!}{log(2)^k 2^n} = 0$$

We see from this that exponential function of a base raised to an input value power is infinitely worse than a high order polynomial function.

**Transitive Property of Big-Oh**

Given $$ f(n) = O(g(n)) \\ g(n) = O(h(n)) \\ \implies f(n) = O(h(n)) $$

**Fibonacci Sequence**

The sequence is defined as

$$ F_0 = 0, F_1 = 1 \\ F_n = F_{n-1} + F_{n-2}, n \geq 2$$

A naive algorithm might do something like

        def fib(n)
            if n == 0 return 0
            if n == 1 return 1
            if n > 1 return fib(n-1) + fib(n-2)

However this algorithm isn't efficient because we keep computing many of the same values over and over again for just a single calculation, while a simpler lookup array could be far faster.

Another way to calculate fibonacci values is via the following equation

$$\begin{bmatrix} 1 & 1 \\ 1 & 0  \end{bmatrix}^n \begin{bmatrix} 1 \\ 0 \end{bmatrix} = \begin{bmatrix} F_{n+1} \\ F_n \end{bmatrix}$$

Homework in the book. Pages 8 and 9.

- 1(a)-(j)
- 2
- 3
- 4(a)


### Lecture 3 - 1/25/2017

Review of last lecture:

Two functions $$f, g : N \rightarrow R_{\geq 0} \\ N = \{1, 2, \dots\} \\ R_{\geq 0} = \text{non-negative real #'s}$$

**Big Oh**  
$$f(n) = O(g(n)) \\ \text{iff } f(n) \leq cg(n),\ \forall n \geq n_0, c > 0$$

**Omega**  
$$f(n) = \Omega(g(n)) \\ \text{iff } f(n) \geq cg(n),\ \forall n \geq n_0, c > 0$$

**Theta**  
$$f(n) = \Theta(g(n)) \\ \text{iff } c_1 g(n) \leq f(n) \leq c_2g(n),\ \forall n \geq n_0, c_i > 0$$

**Little Oh**  
$$f(n) = o(g(n)) \\ \text{iff } \lim\limits_{n\rightarrow\\infty} \frac{f(n)}{f(n)} = 0$$


Given $$k^k$$, and $$2^{k^2}$$, which is larger (asymptotically)? 

Assume $$log = log_2$$

1. Take log of each
    - $$log(k^k) = k\cdot log(k)$$
    - $$log(2^{k^2}) = k^2\cdot log(2) = k^2$$

2. We immediately see that $$k^k$$ is smaller asymptotically smaller than $$2^{k^2}$$


Given the functions floor and ceil we know $$floor(\frac{n}{2}) + ceil(\frac{n}{2}) = n$$

We also know that $$ln(x) = log(e^x)$$

We should also say that $$\frac{n^n}{e^{n-1}} \leq n! \leq \frac{(n+1)^{n+2}}{e^n}$$

Which gives us that $$n!$$ is bounded.

Useful Summation Formulas

> $$\sum\limits_{i=1}^n i = \frac{n(n+1)}{2} = \Theta(n^2)$$

> $$\sum\limits_{i=1}^n i^2 = \frac{n(n+1)(2n+1)}{6} = \Theta (n^3)$$

> $$\sum\limits_{i=1}^n i^3 = \left(\frac{n(n+1)}{2}\right)^2 = \Theta (n^4)$$

> $$\sum\limits_{i=1}^n x^i = \frac{x^{k+1} - 1}{x-1}$$

What about the summation: $$ A(x) = \sum\limits_{i=1}^n i2^i$$ ??

We can generalize and make it $$ A(x) = \sum\limits_{i=1}^n ix^i$$

Say $$B(x) = \sum\limits_{i=1}^n x^i = \frac{x^{k+1} - 1}{x-1}$$

Then $$B'(x) = \sum\limits_{i=1}^n ix^{i-1} = \frac{(k+1)x^{k}(x-1) - (x^{k+1} - 1)}{(x-1)^2}$$

From this we can simply see that $$xB'(x) = A(x)$$

Moving on to algorithms - __searching for a key in a sorted array__

A sorted array is an array with the condition such that $$ L[1] \leq L[2] \leq \dots \leq L[n]$$

- One option is to run binary search.

Let $$w(n)$$ be the worst case number of comparisons for binary search. We can then say that

$$ w(n) = 1 + w(\left\lfloor \frac{n}{2} \right\rfloor)$$

$$w(1) = 1$$

$$w(n) = 1 + \left\lfloor log(n) \right\rfloor $$

We can say now that the worst case is $$log(n)$$, but what about the average case? Or the best case.

Let $$A(n)$$ be the average # of comparisons in binary search, assuming all numbers are distinct.

Given that $$n = 2^k - 1$$ and $$w(n) = k$$

For $$t=1,2,3\dots,k$$ Let $$s_t =$$ the number of positions where is takes $$t$$ comparisons to find the correct location.

Assume the following table for $$L_1 \rightarrow L_5$$

|  | lb | L1 | L2 | L3 | L4 | L5 | rb |
|loc| 3 | 2  |  3 |  1 |  3 |  2 | 3  |

The average time is then given by $$ \frac{\sum loc}{n + 2} $$

In more mathematical terms there are $$n$$ locations plus $$n+1$$ gaps which gives us $$2n+1$$ locations.

The Average time then becomes $$A(n) = \sum\limits_{t=1}^k t\cdot Pr(# of comparisons)

$$A(n) = \sum\limits_{t=1}^k t$$

$$S_k = (2n+1) - 












