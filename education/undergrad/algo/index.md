---
layout: page
title: Algorithm Design and Analysis
permalink: /education/algo/
keywords: 01:198:344, rutgers, software, engineering, algorithms, design, analysis, big, O, notation, runtime
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

> $$\sum\limits_{i=1}^n x^i = \frac{x^{k+1} - x}{x-1}$$

What about the summation: $$ A(x) = \sum\limits_{i=1}^n i2^i$$ ??

We can generalize and make it $$ A(x) = \sum\limits_{i=1}^n ix^i$$

Say $$B(x) = \sum\limits_{i=1}^n x^i = \frac{x^{i+1} - x}{x-1}$$

Then $$B'(x) = \sum\limits_{i=1}^n ix^{i-1} = \frac{(i+1)x^{i}(x-1) - (x^{i+1} - 1)}{(x-1)^2}$$

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

The Average time then becomes $$A(n) = \sum\limits_{t=1}^k t\cdot Pr(# of comparisons) $$

$$A(n) = \sum\limits_{t=1}^k t$$

### Lecture 4 - 1/30/2017

Given a list of $$n$$ elements, we want to determine whether a specific element is contained in a list of the elements. We define 

- $$W(n)$$ - The worst case run time
- $$A(n)$$ - The average case run time

In binary search we fine that $$W(n) = \lfloor log(n) \rfloor + 1 $$.

Representing the number of items as $$2^k - 1$$ giving $$W(n) = k$$

To calculate the average case we get 

> $$A(n) = \sum\limits_{t=1}^k t\cdot Pr(t) = \sum\limits_{t=1}^k t\frac{S_t}{2n + 1}$$

where $$S_t$$ is the number of position where it takes $$t$$ comparisons.

#### Divide and Conquer Algorithms

Many algorithms are recursive in nature and call themselves one or more times. This is called using a "divide and conquer" approach because the algorithms solve small subproblems before coming back together to complete the entire problem.

Steps

1. Divide the problem into smaller subproblems
2. Conquer: Solve the subproblems recursively. If the subproblem size is small we solve it with a straightforward approach.
3. Combine solutions for subproblems into solutions for the original problem.

One of the classic divide and conquer problems is **mergesort**.

The key problem in mergesort is combining two sorted lists. The function might be defined as $$\text{merge}(A, p, q, r)$$ where

- A is an array
- p, q, and r are indices.

Assume $$A[p] - A[r]$$ is sorted and $$A[q+1] to A[r]$$ is sorted

The best approach will take no more than $$r - p$$ comparisons. This works by using two pointers and incrementing one of them on the list until we find a value which is larger than the other pointer. We switch pointer comparisons until we reach the end of one list. Then we append the rest of the other list.

Example:

    Given the list 
    5  2  4  6  1  3  2  6

    Show the mergesort steps

    5  2  4  6 |  1  3  2  6
    5  2 |  4  6 |  1  3 |  2  6
    5 | 2 | 4 | 6 | 1 | 3 | 2 | 6

    2  5 | 4  6 | 1  3 | 2  6
    2  4  5  6 | 1  2  3  6
    1  2  2  3  4  5  6  6


The algorithm for mergesort can be written:

    margesort(A, p, r):
      if p < r
        q <-- floor( (p + r) / 2)
        mergesort(A, p, q)
        mergesort(A, q+1, r)
        mergesort(A, p, q, r)


To analyze this type of algorithm we can relate the overall runtime to the runtime of the subproblems.

For mergesort, given $$T(n) = $$ number of comparison to sort a list of $$n$$ numbers with mergesort

$$ T(n) = \begin{cases} 0 & n = 1 \\ 2T(n/2) + n-1 & n > 1 \end{cases} $$

This formula can be generalized to the form

$$ T(n) = \begin{cases} \Theta (1), & n\leq n_0 \\ \alpha T(n/b) + D(n) + C(n) & n > n_0 \end{cases} $$

Where

- $$\alpha = $$ number of subproblems
- $$\frac{n}{b} $$ = size of each subproblem
- $$D(n) = $$ time to divide
- $$C(n) = $$ time to combine


If we apply the mergesort algorithm to this to analyze the runtime we can first state that $$T(n) = 2^k)$$

$$T(n) = 2^k \\ = 2T(2^{k-1}) + 2^k - 1 \\ = 2^2T(2^{k-2}) + 2^k - 2 + 2^k - 1 \\ 2^2T(2^{k-2}) + 2^k - (1 + 2) \\ \dots \\ T(n) = 2^kT(1) + k2^k - (2^k - 1) \\ T(n) = k2^k - 2^k + 1 $$

Where $$2^k = n \rightarrow k = log_2(n)$$

If we plug in $$k = log(n)$$ into our final answer we find

$$T(n) = nlog(n) - n + 1 = \Theta (n)$$

In multiplying two numbers together we find that the way we normally do it on paper takes approximately $$O(n^2)$$ operations. Wheras the c algorithm implemented in computers are able to do it in approximately $$O(n^{1.59})$$


### Lecture 5 - 2/1/2017

*Review* 

In divide and conquer algorithms we can represent the runtime as the following function

> $$ T(n) = \begin{cases} \Theta(1), & n \leq n_0 \\ \alpha T(\frac{n}{b}) + D(n) + C(n), & \text{else} \end{cases} $$

- $$\alpha$$: Number of subproblems
- $$\frac{n}{b}$$: size of each subproblem
- $$D(n)$$: Cost to divide
- $$C(n)$$: Cost of combining

Proving that the non-base case for $$T(n)$$ is $$O(nlog(n))$$

$$T(n) \leq c\cdot n log_2(n) \\ T(2) = 2T(1) + 2 = 2 \\ c2log(2) = 2c, c \geq 1 \\ T(n) = 2T(n/2) + 2 \leq 2c\frac{n}{2} log(n/2) + n \\ cn(log(n) - log(2) + n = cn log(n) \\ -cn + n \leq c $$

More proofs for solving recurrence relations

$$T(n) = 2T(\lfloor \sqrt{n} \rfloor ) + log(n) \\ n = 2^m \\ T(2^m) = 2T(2^{m/2}) + m \\ S(m) = T(2^m) \\ S(m) = 2S(m/2) + m \\ S(m) = O(mlog(m)) \\ T(n) = O(log(n) \cdot log(log(n)))$$

Using recurrence relations we can find the runtime of divide and conquer algorithms.

**Master Theorem** - $$T(n) = \alpha T(\lceil \frac{n}{b} \rceil) + O(n^d)$$

Where:

$$\alpha > 0 \\ b > 1 \\ d \geq 0$$

and 

$$T(n) = \begin{cases} O(n^d), & d > lob_b(\alpha) \\ O(n^dlog(n)), & d = log_b(\alpha) \\ O(n^{log_b(\alpha)}), & d < log_b(\alpha) \end{cases}$$


### Lecture 6 - 2/6/2017

Quicksort

1. Split and compare elements of the from from left to right with A[1] starting with A[2]
2. If an element is greater than A[1] it stays in place
    - Otherwise, swap with the first key which is either greater than or equal to it in the greater section
3. Finally swap A[1] with the last of the smaller keys (n-1 comparisons)

[For a much better explanation with visuals see this link](https://interactivepython.org/runestone/static/pythonds/SortSearch/TheQuickSort.html)

In the worst possible case, quicksort is $$O(n^2)$$

With randomized recursion where we choose a random pivot index from 1 to n, then $$T(n)$$ which is the average number of comparisons in randomized recursion.

We get that $$A(n) = A(0) + A(n-1) + \dots + n-1$$
**Theorem**: $$A(n) \leq cn\cdot log(n)

$$A(n) = \sum\limits_{k=2}^{n-1} A(k) + n-1 $$

### Lecture 7 - 2/8/2017

*Sorry! No notes for today*

### Lecture 8 - 2/13/2017

Heaps

A structure which satisfies the heap property:

> $$k(x) \leq key(parent(x)) $$

In human terms this means that given a key $$k(x)$$ that we can say that a structure satisfies the heap property if the parent element of $$x$$ is always greater than or equal to its children.

This is different from a binary tree where we know the relationship between the left and right nodes must be that the left node is always less than the right. In a heap this isn't true. In a heap all we know is that the parent is always greater than the children.

So when popping a new item from a heap we must ensure that the heap property is still satisfied. In order to do this we remove the item from the heap, then we fix the heap by sifting the new largest value of the roots children as the new root. Then look at that nodes children and bring the larger one up, and so on.

The recurrence relation for heapsort is $$ T(n) \leq 2T(\frac{n}{2})+ c\cdot log(n)$$

If we break this down by setting $$n=2^k$$ we can get $$T(2^k) \leq 2T(\frac{2^k}{2}) + n\cdot log(2^k) = T(2^k) \leq 2T(2^{k-1}) + c\cdot k$$

Then breaking down the recurrence relation we

$$T(2^k) \leq 2(2(T(2^{k-2)} + k-1) + k $$

$$ T(2^k) \leq 2(2(2(T(2^{k-3}) + k-2) + k-1) + k$$

By going until $$ k =0 $$ we get

$$2^k +k + 2(k-1) + 2^2(k-2) + 2^3(k-3) + \dots = 2^k + \sum\limits_{i=0}^{k-1} 2^i(k-i)$$


## Lecture 10 - 2/27/17 - Graphs

Graphs are special constructs which include vertices and edges. We represent a graph $$G$$ by

> $$ G(V, E) $$

Where $$V$$ represents the set of all vertices in the graph. $$E$$ represents the edges in the graph. When drawing a graph, an edge is associated with two vertices and is typically represented by a line drawn between the two vertices which it connects.

### BFS (Breadth-First Search)

Breadth-first search means starting from a specific node and searching for a value within a graph by exploring all nodes within a similar distance of the first before moving outwards to search more distant nodes.












