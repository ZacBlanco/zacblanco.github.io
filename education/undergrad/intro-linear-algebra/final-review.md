---
layout: page
title: Intro to Linear Algebra - Final Exam Topics Overview
keywords: engineering, rutgers, spring 2016, undergraduate, math, science, 01:640:250, rutgers university, linear, algebra, notes, final, exam, review, guide
permalink: /education/intro-linear-algebra/final-review/
mathjax: true
author: Zac Blanco
---

## Chapter 1 - Matrices, Vectors, and Systems of Linear Equations

Given a set of equations in the form of $$a_{11}x_1 + a_{12}x_2 + \dots + a_{1n}x_n = b_1$$ for $$m$$ equations.

Now in a system there are three different possible outcomes for the solutions of the system $$Ax = b$$

1. A Unique Solution
2. Many Solutions
3. No Solutions

If the rank of the augmented matrix (ex, number of non-zero rows after gaussian elimination) is the same as the rank of the coefficient matrix, then there is at least one set of solutions to the system.

How do we know whether a system has many or just a single solution?

We can look at the number of columns and the rank of A, the coefficient matrix.

If the **number of columns equals the rank** of the matrix then there is a single unique solution.

OR

The difference between the columns and the rank of A is equal to the number of free variables in the solution.

What about finding out all of the solutions?

We turn the matrix into a row-reduced form using gaussian eliminations and elementary row operations.

The three types of operations are:

1. Multiplying a row by a constant, adding it to another row, replacing that row.
2. Swapping two rows
3. Multiplying a row by a constant

## Chapter 2 - Matrices

Topics:

- Matrix multiplication
- Scalar Multiplication
- Invertible Matrices

How do we find an inverse matrix, say, $$A^{-1}$$

Use the augmented matrix of A and the identity matrix, perform elementary row operations until the side where the matrix A was present is the identity matrix. The right hand side is then the inverse matrix.

And inverse matrix is a matrix such that all of the diagonal entries have a value of 1, and all other entries are 0. The transpose of this matrix is the same. $$I_n = I_n^T$$

### LU Decomposition

LU Decomposition is a way to get the upper and lower triangles of a matrix.

Obtaining the Upper triangle is simple via the elementary row operations. We simply reduce the matrix until we get to a row-echelon form. However it is important to **make note of the operations required** to get to the echelon form.

Now, to get the lower (L) matrix first, start with an identity matrix.

To get the lower triangle simply perform those same operations on the original matrix, but in the **opposite order** and **negating** the operation on the first row.

Ex: 

If we needed 2 operations

1. $$-3r_1 + r_2 \rightarrow r_2$$
2. $$r_2 + r_3 \rightarrow r_3$$

Then the operations for the Lower (L) matrix are:

1. $$-r_2 + r_3 \rightarrow r_3$$
2. $$3r_1 + r_2 \rightarrow r_2$$


## Chapter 3 Determinants

Know how to calculate determinants. 

- Row-Factor expansion
- Row-Reduce and get the product of center entries.

Properties of Determinant

- $$ det(A) \neq 0 \rightarrow \text{ A is invertible }$$
- $$ det(A) = det(A^T) $$
- $$ det(AB) = det(A)\cdot det(B) $$
- $$ \text{det}(A^{-1}) = \frac{1}{\text{det}(A)} $$

Given an Upper (U) or Lower (L) triangular matrix, the determinant is simply 

$$ \text{det}(A) = u_{11}u_{22}u_{33}\dots u_{nn} $$

Given a 2x2 matrix with the values:

$$A =  \begin{bmatrix} a & b \\ c & d \\ \end{bmatrix}$$

The inverse of matrix $$A$$ given by $$A^{-1}$$ is:

$$A^{-1} =  \frac{1}{det(A)} \begin{bmatrix} d & -b \\ -c & a \\ \end{bmatrix} $$

where $$\text{det}(A) = ad - bc $$

### Geometric Applications of the Determinant

Given 2 vectors, $$ u = \begin{bmatrix} u_1 \\ u_2 \\ \end{bmatrix} $$, $$ v = \begin{bmatrix} v_1 \\ v_2 \\ \end{bmatrix} $$ and $$A = [u\ v]$$

The **area of a parallelogram** which is determined by $$u$$ and $$v$$ is given by **$$\vert\text{det}(A)\vert$$**

Similarly, given 3 vectors: 

$$ u = \begin{bmatrix} u_1 \\ u_2 \\ u_3 \\\end{bmatrix} $$

$$ v = \begin{bmatrix} v_1 \\ v_2 \\ v_3 \\ \end{bmatrix} $$

$$ w = \begin{bmatrix} w_1 \\ w_2 \\ w_3 \\ \end{bmatrix} $$

The **volume of the parallelepiped** created by the 3 vectors $$u$$, $$v$$, and $$w$$ is equal to $$\vert\text{det}(A)\vert$$, where $$A = [u\ v\ w]$$



## Chapter 4 - Subspaces

An area less than $$R^n$$, such that it follows the rules that for any two vectors $$u$$ and $$v$$ and any scalar $$r$$

1. $$u + v \in V$$ - Any two arbitrary vectors
2. $$u + v \in V$$ for any $$u$$ or $$v$$
3. $$r\cdot u \in V$$ for any r
4. The zero vector is contained within the space $$V$$.

What about the dimension of the subspace? 

We need to find a basis. It is the "length" of a basis. Basis is the smallest set of generators for a space.

The vectors in a basis are all linearly independent.

Other types of dimensions

- $$ Col(A) = Span\{u_1, \dots , u_k\} $$
- $$ Null(A) = \{x\in R^n\ ;Ax = 0\} $$
- $$ dim(Col(A)) = rank(A) $$
- $$ dim(Null(A)) = n - rank(A) = \text{nullity} (A)$$
- Orthogonal Complement of W
  - $$dim(W) + dim(W^\bot) = n $$
  
A few extra definitions:

- **Null Space** of A is the solution set of $$Ax = 0$$, denoted by $$\text{Null}(A) $$
- **Column Space** of a matrix, A, is the _span_ of the columns of matrix A


**Basis**

A basis is the smallest set of generators for a certain type of set.

Example:

Given a $$2\times 9$$ sized matrix, it is more than likely a single column is a linear combination of the other columns. That is in other words, one of the columns of the matrix can be defined by the equation $$c_1u_1 + c_2u_2 + \dots + c_iu_i$$.

A **basis** for the column space is the smallest number of columns which are linearly independent (none of the columns are linear combinations of one another).

The **dimension** of a basis is simply the number of vectors which are included in the basis.



## Chapter 5 - Eigenvalues, Eigenvectors, Diagonalization
 
We can determine eigenvectors by the equation $$Av = \lambda v$$ where v is an eigenvector and $$\lambda$$ is an eigen value of the eigenvector.
 
Diagonalizable Matrices
 
1. Solve $$A-\lambda I_n = 0$$
2. Solve $$(A-\lambda I_n)x = 0 $$
3. Get P and D; $$P = [ x_1 \dots x_k ]$$ and D is diagonal matrix of eigenvalues

### Solving Systems of Differential Equations

1. Find the eigenvalues of the matrix which are the coefficients for the different $$y$$ functions
2. Let P be a matrix whose columns consist of basis vectors for each eigenspace of A, and let D be the diagonal matrix whose diagonal entries are the eigenvalues of A corresponding to the respective columns of P.
3. Solve the system $$z' = Dz$$
4. The solution to the original system is $$y = Pz$$
 
## Chapter 6 - Orthogonality


 
 









