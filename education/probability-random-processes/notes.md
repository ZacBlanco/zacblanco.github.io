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

<!--
Complete later
This can be proved h7 using the following logical steps
1. Suppose x belongs the the complement of the union of A and B
2. 1. implies that x does not belong to the union of sets A and B
3. 2. implies that 
-->

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












