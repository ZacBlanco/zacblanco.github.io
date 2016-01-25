---
layout: page
title: Probability & Random Processes
permalink: /education/probability-random-processes/
keywords: rutgers, university, electrical, computer, engineering, ece, 14:332:226, ECE226
permalink: /education/probability-random-processes/notes/
mathjax: true
author: Zac Blanco
---

<!--
Latex Reference
https://oeis.org/wiki/List_of_LaTeX_mathematical_symbols
$$
\in
\not\in
\subset
\not\subset
\not\cup
\not\cap
\cup
\cap
\not
\neq
\eq
\varnothing
$$
-->


These notes are for the version of the class which is taught by professor Yates in the spring of 2016.

## Lecture 1 - January 21st 2016

We're going to start first with **Set Theory**

### Set Theory

A **set** is a collection of things. We usually will use capital letters to denote sets

parts of a set are called **elements**. When we use mathematical notation to denote an **element** as being part of a **set** we use the following symbol **\in**

Example: $$ x \in A $$ - x belongs to A

**Set Unions**

The **union** of sets $$A$$ and $$B$$ is the set of all elements that are either in A or in B or in both The union of A and B $$\cup$$

**Set Intersections** 

The **intersection** of sets is set of elements where each elements present in the set of A is also contained in B. We use the symbol $$\cap$$ to show the intersection of two sets.

**Mutually Exclusive** set what we call two sets with no common elements

We call the **universal set** the set which contains everything. That is if we imagine all of our elements being made up of sets $$A_1 \dots A_n $$ then we get $$ A_1 \cup A_2 \cup \dots \cup A_n = S$$

A **partition** is a set which is mutually exclusive and collectively exhaustive part of S.

**Theorem 1.1**

De Morgan's law relates all three basic operations

> $$ (A \cup B)^C = A^c \cap B^c $$

<!--Complete later-->

This can be proved by using the following logical steps

1. Suppose x belongs the the complement of the union of A and B
  - $$x \in (A \cup B)^c$$
2. Step 1 implies that x does not belong to the union of sets A and B 
  - $$ x \not\in A \cup B $$
3. Step 2 implies that x does not belong to set A nor set B.
  - $$ x \not\in A \text{ and } x \not\in B$$
4. Step 3 implies that x must then belong to the complement of set A and the complement of set B. 
  - $$ x \in A^c \text{ and } x \in B^c$$
5. Step 4 Then must imply that x is contained within the intersection of sets $$A^c$$ and $$B^c$$. 
  - $$ x \in A^c \cap B^c$$

### Applying Set Theory to Probability

An experiment consists of a **procedure** and **observations**. The is uncertainty in what will be observed; otherwise, performing the experiment would be unnecessary

an **outcome** of an experiment is any possible observation of that experiment

The **sample space** of an experiment is the finest-grain, mutually exclusive, collectively exhaustive set of all possible outcomes.

- Sample space = universal set

an **event** is a **set of outcomes** of an experiment


### Probability Axioms

A **probability measure** P[.] id s gunction that **maps events in the sample space to real numbers**.

- **Axiom 1** - For any event A, $$P[A] \geq 0$$
- **Axiom 2** - $$P[S] = 1$$
- **Axiom 3** For any countable collection $$A_1, A_2, \dots $$ of mutually exclusive events: $$ P[A_1 \cup A_2 \cup \dots] = P[A_1] + P[A_2] \dots$$


**Theorem 1.3** 

if $$ A = A_1 \cup A_2 \cup \dots \cup A_m $$ and $$ A_i \cap A_j = \varnothing $$ for $$ i \neq j $$.

In other words

> $$ P[A] = \sum\limits^m_{i=1} P[A_i] $$

**Theorem 1.4**

The probability measure $$P[.]$$ satisfies the following:

- $$P[\varnothing] = 0$$
- $$P[A^c] = 1 - P[A]$$
- For any A and B (not necessarily mutually exclusive) 
  - $$P[A \cup B] = P[A] + P[B] - P[A \cap B]$$
- $$A \in B \text{ then } P[A] \leq P[B] $$.

**Theorem 1.5**

The probability of an event $$B = {s_1, s_2, ..., s_m}$$ is the sum of the probabilities of the outcomes contained in the event:

> $$ P[B] = \sum\limits^m_{i=1} P[{s_i}] $$


**Theorem 1.6**

For an experiment with sample space $$S = {s_1, \dots, s_n}$$ in which each outcome $$s_i$$ is equally likely:

> $$P[s_i] = \frac{1}{n}, 1\leq i\leq n$$



## Lecture 2 - January 25th 2016


**Section 1.4 - Conditional Probability**


Definition, **Conditional Probability**: The probability of the event A given the occurrence of event B is:

 > $$ P[A|B] = \frac{P[AB]}{P[B]} = \frac{P[A\cap B]}{P[B]}$$
 
 The vertical bar, "\|" stands for the word "given".
 
 What this equation does is "scale up" the probability of all events inside the event B. This effectively makes B the "new" sample space because all events that occur after B is given must be contained within B


**Theorem 1.7**

 A conditional probability measure $$P[A\|B]$$ haas the following properties that correspond to the axioms of probability.
 
 1. $$P[A|B] \geq 0$$
 2. $$P[B|B] = 1 $$
 3. If $$A=A_1\cup A_2\cup \dots $$ with $$A_i \cap A_j = \varnothing$$ for $$i\neq j$$ then 
   - $$P[A|B] = P[A_1|B] + P[A_2|B] + \dots$$
   
**Partitions and the Law of Total Probability**

**Theorem 1.8**

For a partition $$B = {B_1, B_2, \dots}$$ and any event A in the sample space, let $$C_i = A\cap B_i$$ for $$i \neq j$$ , the events $$C_i \text{and} C_j$$ are mutually exclusive and:

> $$ A = C_1\ cup C_2 \cup \dots$$

**Theorem 1.9**

For any event A, and partition $${B_1, B_2, \dots , B_m}$$

> $$P[A] = \sum\limits^m_{i=1} P[A\cap B_i] $$


**Theorem 1.10**

For a patition $${B_1, B_2, \dots, B_m}$$ with $$P[B_i] > 0$$ for all i, 

> $$ P[A] = \sum\limits^m_{i=1} P[A|B_i] P[B_i]$$

 













































