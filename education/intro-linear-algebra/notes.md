---
layout: page
title: Intro to Linear Algebra
keywords: engineering, rutgers, spring 2016, computer, undergraduate, math, science, 01:198:250, rutgers university, linear, algebra, introduction, notes
permalink: /education/intro-linear-algebra/notes/
mathjax: true
author: Zac Blanco
---

## Lecture 1 - January 21st 2016

--------------------------------

Chapter 1. Matrices, Vectors, and Systems of Linear Equations

**Matrix**: is a rectangular array of scalar quantities

$$

\begin{bmatrix}
    x_{11}       & x_{12} & x_{13} & \dots & x_{1n} \\
    x_{21}       & x_{22} & x_{23} & \dots & x_{2n} \\
    ... \\
    x_{m1}       & x_{m2} & x_{m3} & \dots & x_{mn}
\end{bmatrix}

$$

We can see here that rows are numbered from 1 to $$m$$. and that we have rows from 1 to $$n$$.

Normally we say that a matric will be $$m\times n$$ in size where **m denotes rows** and **n denotes colummns**.

That is:

- **m** is rows
- **n** is columns

Matrices provide "tools" for calculations

There is a short notation for a matrix which looks like the following

$$

A = (aij)_{m\times n}

$$

but what if we have the expression:

$$

A = (aij)_{m\times n} = B=(bij)_{m\times n}

$$

This means that the matrices are equal at values $$(i, j)$$ in each matrix

### **Matrix Operations**

**Addition**

Given $$A=(aij)_{m\times n}$$ and that $$ B=(bij)_{m\times n}$$

Then when adding two matrices, they must be the same size. As long as they are the same size. You simple add the elements which are in the same position of each matrix,to form the new value in that location in the resultant matrix

So for addition in short notation$$ A + B = (aij + bij)_{m\times n} $$

**Scalar Multiplication**

In scalar multiplication of a matrix you simply just multiply a scalar quantity by every entry of a matrix. In short notation this would be:

$$ r \cdot A = (r\cdot aij)_{m\times n} $$

Example:

$$r = 2$$ and $$A = \begin{bmatrix} 1 & 2 \\
3 & 4 \end{bmatrix} $$

Then $$ r\cdot A$$ is equal to

$$

r \cdot A = \begin{bmatrix} 2 & 4 \\ 6 & 8 \end{bmatrix}

$$

**Properties of Matrix Addition and Scalar Multiplication**

- $$ A + B = B + A $$
- $$(A + B) + C = A + (B + C) $$
- $$ A + 0 = A $$
- $$ A + (-A) = 0 $$
- $$ (st)A = s(tA) $$
- $$ s(A + B) = sA + sB $$
- $$ (s + t)A = sA + tA $$


### Vectors

Vectors are simply **one-dimensional matrices**. All of the properties of matrices apply to vectors as well.

Example:

$$

u = \begin{bmatrix} a_1 \\
                    a_2 \\
                    \dots \\
                    a_n
\end{bmatrix}

$$

or

$$
u = \begin{bmatrix} a_1 &
                    a_2 &
                    \dots &
                    a_n
\end{bmatrix}
$$

**Geometry of Vectors**

Given $$ u = \begin{smallmatrix} a \\ b \end{smallmatrix} $$ we can say that we can graph this vector on a 2D coordinate system.

But what if there are 3? or more? So our vector might be $$ u = \begin{smallmatrix} a_1 \\ . \\ . \\ a_n \end{smallmatrix} $$ well then we can graph on an larger coordinate system with more dimensions. So 3 rows in a matrix might lead us to 3-dimensional coordinates and a 4th to 4, and so on...

### Linear Combinations - Matrix-Vector Products and Special Matrices

Let $$ u_1 \dots u_n $$be vectors. Let $$ c_1 \cdots c_k$$ be scalars

We call $$c_1u_1 + c_2u_2 + \cdots + c_ku_k $$. This is called a **linear combination**

In other words, it is the sum of all of our vectors $$u$$, each with their own respective coefficient.

But why do we call it linear?

