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

**Note!**: Typically the $$\cap$$ is not used in problems and the following statements are equivalent:

> $$ P[A\cap B] = P[AB] $$

**Mutually Exclusive** set what we call two sets with no common elements

We call the **universal set** the set which contains everything. That is if we imagine all of our elements being made up of sets $$A_1 \dots A_n $$ then we get $$ A_1 \cup A_2 \cup \dots \cup A_n = S$$

A **partition** is a set which is mutually exclusive and collectively exhaustive part of S.

**Theorem 1.1**

De Morgan's law relates all three basic operations

> $$ (A \cup B)^C = A^c \cap B^c $$

<!--Complete later-->

This can be proved by using the following logical steps

1. Suppose x belongs the complement of the union of A and B
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

A **probability measure** P[.] id s function that **maps events in the sample space to real numbers**.

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

 A conditional probability measure $$P[A\|B]$$ has the following properties that correspond to the axioms of probability.
 
 1. $$P[A|B] \geq 0$$
 2. $$P[B|B] = 1 $$
 3. If $$A=A_1\cup A_2\cup \dots $$ with $$A_i \cap A_j = \varnothing$$ for $$i\neq j$$ then 
   - $$P[A|B] = P[A_1|B] + P[A_2|B] + \dots$$
   
**Partitions and the Law of Total Probability**

**Theorem 1.8**

For a partition $$B = {B_1, B_2, \dots}$$ and any event A in the sample space, let $$C_i = A\cap B_i$$ for $$i \neq j$$ , the events $$C_i \text{and} C_j$$ are mutually exclusive and:

> $$ A = C_1 \cup C_2 \cup \dots$$

**Theorem 1.9**

For any event A, and partition $${B_1, B_2, \dots , B_m}$$

> $$P[A] = \sum\limits^m_{i=1} P[A\cap B_i] $$


**Theorem 1.10**

For a partition $${B_1, B_2, \dots, B_m}$$ with $$P[B_i] > 0$$ for all i, 

> $$ P[A] = \sum\limits^m_{i=1} P[A|B_i] P[B_i]$$

 
## Lecture 3 - January 28th 2016

**Theorem 1.11:** Bayes' Theorem

> $$ P[B|A] = \frac{P[A|B] P[B]}{P[A]} $$

What this is saying is that the probability of an event B, given the event A, will be exactly equal to the probability of A divided by the probability of B, multiplied by the probability of A given B

**Section 1.6** Independence

**Definition**: Independence - Events A and B are independent if and only if:

> $$ P[AB] = P[A]P[B] $$


A comment on event independence and mutually exclusivity. The two are **not synonyms**. In some contexts they can have similar meanings. But in probability this is not the case. 

Mutually exclusive events A and B have no outcomes in common and therefore $$P[AB] = 0$$. **In most situations independent events are not mutually exclusive**.

Exceptions occur only when $$P[A] = 0 \text{ or } P[B] = 0$$. When we have to calculate probabilities, knowledge that events A and B are mutually exclusive is very helpful. Axiom 3 enables us to add their probabilities to obtain the probability of the _union_. Knowledge that events C and D are independent is also very useful. Definition 1.6 enables us to multiply their probabilities to obtain the probability of the _intersection_.

**Definition 1.7** - Three Independent Events

Three events are mutually independent if and only if:

- $$ A_1 \text{ and } A_2 $$ are independent
- $$ A_2 \text{ and } A_3 $$ are independent
- $$ A_1 \text{ and } A_3 $$ are independent

and finally as long as: $$ P[A_1 \cap A_2 \cap A_3] = P[A_1]P[A_2]P[A_3] $$



**Definition 1.8** More than two Independent Events

This definition is the same as above. All collections of events must be mutually exclusive of one another, 

The probability of intersections of these events must be equal to the product of their individual probabilities.


## Lecture 4 - February 1st, 2016

## Chapter 2 - Tree Diagrams

Below is an example problem which will introduce us to tree diagrams

---

![Tree Diagram Example Problem](/assets/images/probability/tree-diag-1.png)

---


From this we can see that tree diagrams help us to calculate probabilities of sequential events.

> Tree diagrams display the outcomes of the **subexperiments** in a _sequential experiment_. The labels of the branches are probabilities and conditional probabilities. The probability of an outcome of the entire experiment is the product of the probabilities of branches from the root of the tree to a leaf.


Many experiments will consist of what we call **subexperiments**. The procedure followed for each subexperiment may depend on the results of the previous subexperiments.

In doing this we will assmble outcomes into different partitions, starting at the root of the tree. From there we branch (and label according probabilities) based on the probabilities of each outcome.

###Section 2.2 - Counting Methods 

> In all application of probability theory, it should be important to understand the sample space of an experiment.

The methods in this section will help determine the number of outcomes in the sample space of an experiment.

**Theorem 2.1**

An experiment consists of two subexperiments. If one subexperiment has $$k$$ outcomes and the other subexperiment has $$n$$ outcomes, then the experiment has a total of $$nk$$ outcomes.


**Theorem 2.2**


The number of $$k$$-permutations of $$n$$ distinguishable objects is:

> $$(n)_k = n(n-1)(n-2)(n-3)\dots(n-k+1)=\frac{n!}{(n-k)!} $$


Something that might help to remember permutations is that a **permutation does not depend on the order in which the elements are in** whereas in contrast a combination depends on the order.

For example, think if we had 100 students who wanted to take a class. Imagine that the class could only take 50 students. This means that the number of _different_ **permutations** (arragements, groups) is equal to $$\frac{100!}{50!}$$


**Theorem 2.3**

The number of ways to choose k objects out of n distinguishable objects is:

> $$\begin{pmatrix} n\\k \end{pmatrix} = \frac{(n)_k}{k!} = \frac{n!}{k!(n-k)!}$$

Sometimes this is called a **combination** where the order in which items are picked matters, as opposed to the **permutation**.


**Definition 2.1**: $$n$$ choose $$k$$

For any integer $$n \geq 0 $$, we define

> $$\begin{pmatrix} n \\ k \end{pmatrix} = \begin{cases} \frac{n!}{k!(n-k)!} & k = 0, 1, \dots, n, \\ 0 & \text{otherwise}. \end{cases} $$

**Theorem 2.4**

Given $$m$$ distinguishable object, there are $$m^n$$ ways to choose with replacement an ordered sample of $$n$$ objects.

**Theorem 2.5**

for $$n$$ repetitions of a subexperiment with sample space $$S_{sub} = \lbrace s_0, \dots, s_{m-1} \rbrace$$, the sample space $$S$$ of the sequential experiment has $$m^n$$ outcomes


**Theorem 2.6**

The number of observation equencies for n subexperiments with sample space $$S = \lbrace 0, 1 \rbrace$$ with 0 apprearing $$n_)$$ times and 1 appearing $$n_1=n-n_0$$ times is $$\begin{pmatrix} n \\ n_1 \end{pmatrix} $$

## Lecture 5 - February 8th 2016

**Theorem 2.7**

For $$n$$ repetitions of a ubexperiment with sample space $$S = \{s_0,\dots,s_{m-1}\}$$ the number of length $$n = n_0 + \dots + n_{m-1}$$ obervation sequencies with $$s_i$$ appearing $$n_i$$ times is

> $$ \begin{pmatrix} n \\ n_0,\dots,n_{m-1} \end{pmatrix} = \frac{n!}{n_0!n_1!\dots n_{m-1}!} $$

### Independent Trials

**Theorem 2.8**

The probability of $$n_0$$ failures an $$n_1$$ successes in $$n=n_0+n_1$$ independent trials is:

> $$ P[E_{n_0}, n_1] = \begin{pmatrix} n \\ n_1 \end{pmatrix} = (1-p)^{n-n_1}p^{n_1} = \begin{pmatrix} n \\ n_0 \end{pmatrix} (1-p)^{n_0}p^{n-n_0} $$

## Chapter 3 - Discrete Random Variables

**Definition - Discrete Variables** - A random variable assigns numbers to outcomes in the sample space of an experiment.

A probability model always begins with an experiments. And each of the random variables is directly related to the experiment.

There are **3 types of relationships**

1. The random variable is the observation
2. The random cariable is a function of the observation
3. The random variable is a function of another random variable.

Throughout most of the book probability is examined in models that assign numbers to the outcomes of the sample space. 

When we observe the observation we call it a _random variable_. Typically in our notation the name of our random variable is a capital letter. e.g. $$X$$.

The set of possible values for $$X$$ is the **range of $$X$$**

Since we often consider multiple variables at a time, we denote the range of a random variable by the letter $$S$$ with a subscript that is the name of the random variable.

Thus, $$S_X$$ is the range of the random variable $$X$$, $$S_Y$$ is the range of the random variable $$Y$$ and so on...


**Definition 3.1** 

**Random Variable** consists of an experiment with a probability measure $$P[.]$$ defined on a sample space $$S$$ and a _function that assigns a real number to each outcome in the sample space of the experiment._

A note on notation:

- On occasion should we need to identify the random variable $$X$$ with respect to a sample outcome, then we define this as $$X(s)$$. (In other words we define a function which relates the value of X to the outcomes - X is a function of $$s$$, the outcome)

- Sometimes we will write $$\{ X = x\} $$to emphasize that there is a set of sample points $$s \in S $$ for which $$X(s) = x$$.

From these we can adopt the shorthand notation:

> $$\{X = x\} = \{s\in S \ \vert\  X(s) = x\} $$

**Defintion 3.2**

$$X$$ is a **discrete random variable** if the range of $$X$$ is a _countable set_

$$
S_X = \{x_1,x_2,\dots\}
$$

**Definition 3.3**

**Probability Mass Function** (PMF) - The probability mass function of the discrete random variable X is 

> $$ P_X(x) = P[X=x] $$



















































