---
layout: page
title: Intro to Linear Algebra
keywords: engineering, rutgers, spring 2016, computer, undergraduate, math, science, 01:198:250, rutgers university, linear, algebra, introduction, notes
permalink: /education/intro-linear-algebra/notes/
mathjax: true
author: Zac Blanco
---


--------------------------------

## Chapter 1. Matrices, Vectors, and Systems of Linear Equations

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

### Matrix Multiplication

Matrices can be made of of **m rows** and **n columns**.

That is to generalize for a Matrix A and B with sizes $$m_1\times n_1$$ and $$m_2 \times n_2$$ we have that product matrix of size $$m_1\times n_2$$ where to be able to multiply $$ n_1 = m_2$$

But how do we solve a system of equations using these matrices and the knowledge of linear combinations?

The answer is to utilize the **Reduced Row Echelon Form** or (RREF) for short.

Generally for a linear system we say that given a Matrix of coefficients A, a matrix of uknownes, $$x$$, and what the product of those two are equal to, $$b$$ we get the equation

> $$ A\cdot x = b $$


## Transpose of a Matrix

A matrix transpose is a special operation that we can perform on a matrix. It will turn all of the rows of a matrix into columns

Example: Given the matrix $$A$$

$$
A = \begin{bmatrix} 1 & 2 & 3 \\
					4 & 5 & 6 \\
	\end{bmatrix}
$$

Then $$A^T=$$

$$

A^T = \begin{bmatrix} 1 & 4 \\
					  2 & 5 \\
					  3 & 6 \\
		\end{bmatrix}

$$

Transposing also has a few special properties

> $$ (A^T)^T = A $$

> $$ (A + B)^T = A^T + B^T $$

> $$ r\cdot A^T = (r\cdot A)^T $$


## Solving Systems of Linear Equations

Now that we've gone over the basics of Matrices we can actually begin to utilize them to solve systems of linear equations.

The basics of a system of linear equations:

In a system there are three possible different outcomes:

1. No Solution
2. Infitite Solutions
3. A Single Solution

These outcomes depend on the system and what we are given.

If we recall the equation of A system for matrixes $$ A\cdot x = b$$

We can represent our system with something called an **Augmented Matrix** where we display the  right hand side of each equation in a matrix with the coefficients.


Example:


$$

x_1 + x_2 - x_3 = 0 \\
x_1 - x_2 - x_3 = 2 \\
x_1 + x_2 + x_3 = 5 \\

$$

Augmented Matrix:

$$

\begin{bmatrix}
1 & 1 & -1 & 0 \\
1 & -1 & -1 & 2 \\
1 & 1 & 1 & 5 \\
\end{bmatrix}

$$

Once we have an **augmented matrix** we can turn it into the **Reduced Row Echelon Form (RREF)**

We do this by multiplying the first row by a constant $$r$$, which when added to the 2nd row, will allow us to cancel out the value (sum to 0) the number immediately below the first number in the first row.

i.e. our first $$r$$ would be $$-1$$ for the 2nd and 3rd rows

So then our matrix becomes:

$$
\begin{bmatrix}
1 & 1 & -1 & 0 \\
0 & -2 & 0 & 2 \\
0 & 0 & 2 & 5 \\
\end{bmatrix}
$$

This is reduced row echelon form because we see that as we go down rows, all numbers below the first number in each row are zero.

From that matrix we can translate it back into equations which will allow us to solve the system. From the RREF matrix we find that:

$$
x_3 = \frac{5}{2} \\
x_2 = -1
$$

From here we then solve our final equation giving us: 

$$x_1 = -\frac{3}{2} $$

In order to obtain these RREF matrices we can perform what are called **ELementary Operations** on each matrix.

**Elemntary Operations on a Matrix**

1. Exchange the placement of two rows
2. Multiply a whole row with a non-zero scalar value
3. Add a multiple of one row to another.

All three of these operations can help simplify a matrix, but will not change the outcome or solution for a system.

**Conditions for an RREF Matrix**

There are actually two conditions which need to be met for a matrix to be considered to be in RREF form. They are:

1. The first **non-zero** number in each row must be equal to 1.
2. All numbers which are below and to the left of the first number in each row must be equal to zero.

<!--**Gaussian Elimination**-->





















































