---
layout: page
title: Intro to Linear Algebra
keywords: engineering, rutgers, spring 2016, undergraduate, math, science, 01:640:250, rutgers university, linear, algebra, introduction, notes, review, guide
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

A = (a_{ij})_{m\times n}

$$

but what if we have the expression:

$$

A = (a_{ij})_{m\times n} = B=(b_{ij})_{m\times n}

$$

This means that the matrices are equal at values $$(i, j)$$ in each matrix

### **Matrix Operations**

**Addition**

Given $$A=(a_{ij})_{m\times n}$$ and that $$ B=(b_{ij})_{m\times n}$$

Then when adding two matrices, they must be the same size. As long as they are the same size. You simple add the elements which are in the same position of each matrix,to form the new value in that location in the resultant matrix

So for addition in short notation$$ A + B = (a_{ij} + b_{ij})_{m\times n} $$

**Scalar Multiplication**

In scalar multiplication of a matrix you simply just multiply a scalar quantity by every entry of a matrix. In short notation this would be:

$$ r \cdot A = (r\cdot a_{ij})_{m\times n} $$

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

**Gaussian Elimination**

Gaussian elimination is the name method which we can use to reduce a matrix into RREF (Row-Reduced-Echelon-Form)

This basically consists of using the elementary row operations (noted above) to bring the matrix to a form which falls under the conditions for RREF stated above.

**Note on solving linear systems with augmented matrices and RREF**

> Whenever an augmented matric contains a row in which the only nonzero entry lies in the last column, the corresponding system of linear equations has no solution.

### Procuedure for solving a System of Linear Equations

1. Write the augmented matric $$[A\ b]$$ of the system
2. Find the reduced row echelon form $$[R\ c] \text{ of }\ [A\ b]$$
3. if $$[R\ c]$$ contains a row in which the only nonzero entry lies in the last column, the $$Ax=b$$ has no solution.
  - Otherwise, the system has at least one solution. Write the system of linear equations corresponding to the matrix $$[R\ c]$$ and solve this system for the basic variables in terms of the free variables to obtain a general solution of $$Ax = b$$
  
### Rank and Nullity of a Matrix
 
**Definition: Rank**: the **rank** of an $$m\times n$$ matrix A, denoted by $$rank(A)$$ is defined to be the number of nonzero rows in the reduced row echelon form of A
 
**Definition: Nullity**: The **nullity** of A, denoted by $$nullity(A)$$ is defined to be $$n - rank(A)$$, or we can say that $$rank(A) + nullity(A) = n$$, where $$n$$ is the number of columns of the matrix A.


If Ax = b is the matrix form of a consistent system of linear equations, then

1. The number of basic variables in a general solution of the system equals the rank of A
2. The number of free variables in a general solution of the system equals the nullity of A

Thus a consistent system of linear equations has a unique solution only if the nullity of its coefficient matrix equals 0. 
 
Equivalently we can say that a consistent system has infinite solution if the nullity of its coefficient matrix is positive.
 
In shorter terms:
 
|Nullity(A)| Number of solutions of the System with matrix A |
| $$= 0$$ | 1 |
| $$ > 0$$ | $$\infty$$ |

### Testing for Consistency of a Matrix

The following statements are all equivalent
 
1. The matrix equation $$Ax = b$$ is consistent
2. The vector $$b$$ is a linear combination of the columns of A.
3. The reduced row echelon form of the augmented matrix $$[A\ b]$$ has no rown of the form $$[0\ 0\ 0\ 0\ \dots\ d]$$ where $$d \neq 0$$


## The Span of a Set of Vectors
 
 
**Definition: Span**: For a nonempty set of vectors, $$S = \{u_1, u_2, \dots, u_k\}$$ in the space of $$R^n$$, we define the **span of S** to be the set of all linear combinations of $$u_1, u_2, \dots , u_k$$ in $$R^n$$. This set is denoted by Span S or Span \{u_1, u_2,\dots u_k\}

**Generating Sets**

Imagine instead of trying to find the Span of a set $$S$$ we have a set of vectors $$V$$ and we want to find the span which generates the set $$V$$. We then call $$S$$ the generating set of $$V$$ or that $$S$$ generates $$V$$

**Theorem - Generating Sets**

The following statements are all equivalent:

1. The span of the columns of A is $$R^m$$
2. The equation $$Ax = b$$
3. The rank of A is $$m$$, the number of rows of A
4. Teh reduced row echelon form of A has no zero rows
5. There is a pivot position in each row of A


**Making Smaller Generating Sets**

Let $$S = \{u_1, u_2, \dots, u_k\}$$ be a set of vectors from $$R^n$$ and let $$v$$ be a vector in $$R^n$$. Then the Span $$\{u_1, u_2, \dots, u_k\}$$ if and only if v belongs to the span of $$S$$

## Linear Dependence and Independence

A set of $$k$$ vectors $$\{u_1, u_2, \dots, u_k\}$$ in $$R^n$$ is called **linearly dependent** if there exists a set of scalars $$c_1,c_2, \dots, c_k$$ such that 

$$ c_1u_1 + c_2u_2 + \dots + c_ku_k = 0$$

and that $$c_1, c_2, \dots, c_k \neq 0$$

In other words, if the only set of scalars which satisfy the equation $$ c_1u_1 + c_2u_2 + \dots + c_ku_k = 0$$ are scalars which are all equal to 0, then the set $$k$$ is **linearly independent**

The set $$\{u_1, u_2, \dots, u_k\}$$ is linearly dependent if an only if there exists a nonzero solution of $$Ax=0$$ where $$A = \left[ u_1 \  u_2\ \dots \  u_k  \right]$$

The following statements involving linear independence about a matrix are equivalent:

1. The columns of A are linearly independent
2. the equation $$Ax=b$$ has at most one solution for each $$b$$ in $$R^m$$
3. The nullity of A is zero
4. The rank of A is $$n$$, the number of columns of A
5. The columns of the RREF of A are distinct standard vectors in $$R^m$$
6. The only solution of $$Ax = 0$$ zero
7. There is a pivot position in each column of A

## Matrix Multiplication

Mulitplication of two matrices is different than normal multiplication by a scalar.

**Definition** Let A be an $$m\times n$$ matrix and B be an $$n\times p$$ matrix. We dfine the **matrix product** of $$AB$$ to be the $$m\times p$$ matrix who's jth column is $$Ab_j$$. That is 

> $$C = \left[ Ab_1\ Ab_2\ \dots\ Ab_p\right]$$

Another way of thinking is that the product matrix is equivalent to doing the dot product for all combinations of rows of matrix $$A$$ with all the columns of $$B$$

The product matrix will have a size of $$\left(m\times p\right)$$


**Properties of Matrix Multiplication**

- $$(AB)v = A(Bv)$$
- $$ AB \neq BA $$
- $$ (A+B)C = AC+AB$$
- $$ C(P+Q)=CP+CQ$$
- $$ I_kA = A = AI_m$$
- The product of any matrix and a zero matrix is zero
- $$(AC)^T = C^TA^T$$

## Invertibility and Elementary Matrices

**Definition** an $$n\times n$$ matrix A is called invertible if there exists an $$n\times n$$ matrix B such that $$AB = BA = I_n$$. In this case B is called the **inverse** of A.

If A is an invertible $$n\times n$$ matrix, then for every $$b$$ in $$R^n$$ $$Ax = b$$ has the unique solution $$A^{-1}b$$

**Useful Properties of Matrix Inverses**

1. If A is invertible, then $$A^{-1}$$ is invertible and $$(A^{-1})^{-1}=A$$
2. If A and B are invertible, then AB is invertible and $$(AB)^{-1}=A^{-1}B^{-1}$$
3. If A is invertible, then $$A^T$$ is invertible and $$(A^T)^{-1} = (A^{-1})^T$$


Let $$A_1, A_2,\dots, A_k$$ be $$n\times n$$ invertible matrices. Then the product $$A_1A_2\dots A_k$$ is invertible and 

> $$(A_1A_2\dots A_k)^{-1} = (A_k)^{-1}(A_{k-1})^{-1}(A_1)^{-1}$$

## Chapter 3 - Determinants of a Matrix

Given the matrix $$A = \begin{bmatrix}
a & b \\
c & d \\
\end{bmatrix}$$ and the inverse of A, $$A^{-1}=C=\begin{bmatrix} d & -b \\ -c & a \\ \end{bmatrix}$$.

Now if we multiply these two together we get $$AC = (ad-bc)\cdot\begin{bmatrix} 1 & 0 \\ 0 & 1 \\ \end{bmatrix}$$

Thus if $$(ad-bc)\neq 0$$ then we know that it is possible $$C=A^{-1}$$

So then the matrix which is $$\frac{1}{ad-bc}\begin{bmatrix} d & -b \\ -c & a \\ \end{bmatrix} = A^{-1}$$

So then if $$(ad-bc)\neq 0$$ we then know it is possible for matrix $$A$$ to have an inverse. The quanitity $$(ad-bc)$$ is called the **determinant**.

**So then what about an $$n\times n$$ matrix?**

First we need to define **cofactor expansion** which is the the basic method which can be used to find the determinant of a larger matrix.

Given $$ A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 9 \\ \end{bmatrix}$$

We can label the rows and columns with alternating pluses and minuses. So if we replace the first row and column with pluses and minuses we get 

$$ \begin{bmatrix} + & - & + \\ - & 5 & 6 \\ + & 8 & 9 \\ \end{bmatrix}$$

So to find the determinant via **cofactor expansion** we must use the first row of the matrix and the alternating pluses and minuses to find the determinant.

We can define the determinant of the $$3\times 3$$ matrix as

$$det(A) = (+)a_{11}det(A_{11}) + (-)(a_{12}det(A_{12})) + (+)(a_{13}det(A_{13})) $$

We can define the matrices $$A_{11}$$, $$A_{12}$$, etc.. as the $$(n-1)\times (n-1)$$ matrix which appears when we remove the i-th row and j-th column

So the determinant in this case is actually recursively defined. It is actually composed of even more operations

We find the determinant of A is

$$det(A) = (+1)a_{11}det(\begin{bmatrix} 5 & 6 \\ 8 & 9 \\ \end{bmatrix}) + (-1)(a_{12}det(\begin{bmatrix} 4 & 6 \\ 7 & 9 \\ \end{bmatrix})) + (+1)(a_{13}det(\begin{bmatrix} 4 & 5 \\ 7 & 8 \\ \end{bmatrix})) $$

We can see that once we reduce the matrix to a $$ 2\times 2$$ matrix we will eventually get the scalar value.

Obviously this cofactor expansion is very tedious even for a $$3\times 3$$ so we will explore an even easier way to calculate a larger determinant.

So what is an easier method for this? We can simply find the **upper triangular** or **lower triangular** matrix from the original using **elementary row operations**

Take the following matrix:

$$ A = \begin{bmatrix} 1 & 2 & 3 & 6  \\ 
					4 & 2 & 9 & 5 \\ 
					4 & 2 & 9 & 6 \\
					4 & 2 & 3 & 8 \\
					\end{bmatrix} $$
					
Say we want to find the determinant of this matrix, we could use cofactor expansion to find it, but that would take us a _long_ amount of time.

We're going to use the following elementary row operations

- $$ (-4)R_1 + R_2 $$
- $$ (-4)R_1 + R_3 $$
- $$ (-4)R_1 + R_4 $$

Which then gives the matrix

$$\begin{bmatrix}		1 & 2 & 3 & 6  \\ 
					0 & -6 & -3 & -19 \\ 
					0 & -6 & -3 & -18 \\
					0 & -6 & -9 & -16 \\
					\end{bmatrix} $$
					
Then we can use these row operations to reduce further

- $$ (-1)R_2 + R_3 $$
- $$ (-1)R_2 + R_4 $$

$$ 	\begin{bmatrix} 	1 & 2 & 3 & 6  \\ 
					0 & -6 & -3 & -19 \\ 
					0 & 0 & 0 & 1 \\
					0 & 0 & -6 & 3 \\
					\end{bmatrix} $$

Then we can swap the 3rd and 4th rows of the matrix to make it the upper triangular form.


$$ 	\begin{bmatrix} 	1 & 2 & 3 & 6  \\ 
					0 & -6 & -3 & -19 \\ 
					0 & 0 & -6 & 3 \\
					0 & 0 & 0 & 1 \\
					\end{bmatrix} $$



So now we have the upper triangular matrix. We can simply multiply all of the diagonal entries of the matrix together to find the determinant

So $$det(A) = (-1)\cdot 1 \cdot -6 \cdot -6 \cdot 1 = -36$$

So our determinant is $$-36$$

**Wait wait wait, hold up! there's an extra $$-1$$ in the front of that product!**

Correct! we had to change the sign of the value we found because in one of our elementary row operations, we switched the two rows, instead of just adding. When calculating the determinant you have to multiply the product of all the diagonal entried by $$(-1)^{\text{# of times rows switched}}$$

So in this case we multiplied the determinant by $$(-1)^1$$ which makes the determinant $$-36$$ which is the correct answer.

### Geometric Applications of The Determinant


**Area of a Parallelogram**

Given two vectors $$v$$ and $$u$$ in $$\!R^2$$ which create a parallelogram, the area of the parallelogram is the absolute value of the determinant of $$[ u \ v ]$$

So given a two vectors $$ u = [ 1, 2]^T$$ and $$v = [ -2, 3]$$ the area created by the parallelogram defined by these vectors is equal to 

$$ det(\begin{bmatrix} 1 & 2 \\ -2 & 3 \\ \end{bmatrix}) = (1(3) - (2)(-2)) = 7$$

**Volume of a Parallelepiped**

Given three vectors $$u, v, w$$ which create a parallelepiped we can find the volume of such a figure by taking the absolute value of the determinant of the matrix created by the three vectors which is $$det([u\ v\ w])$$

Example:

Given $$u = [3, 1, 2]$$, $$v = [1, 2, 3]$$ and $$w = [3, 3, 3]$$

Then the matrix becomes

$$\begin{bmatrix} 3 & 1 & 2 \\
			 1 & 2 & 3 \\
			 3 & 3 & 3 \\ \end{bmatrix}$$

So reducing with row operations we get

$$\begin{bmatrix} 3 & 1 & 2 \\
			 0 & 5/3 & 7/3 \\
			 0 & 2 & 1 \\ \end{bmatrix}$$

and then

$$\begin{bmatrix} 3 & 1 & 2 \\
			 0 & 5/3 & 7/3 \\
			 0 & 0 & -9/5 \\ \end{bmatrix}$$
			 
So then if we calculate the determinant we get $$-9$$, and the volume is the absolute value of the determinant, so the volume of the figure is $$9$$

**Rules of Calculating the determinant of a Matrix**

Let A be an $$n\times n$$ matrix

1. if B is a matrix obtained by interchanging two rows of A, then $$det(B) = -det(A)$$
2. If B is a matrix obtained by multiplying each entry of some row of A by a scalar k, then $$det(B) = k\cdot det(A)$$
3. If B is a matrix obtained by adding a multiple of some row of A to a different row, then $$det(B) = det(A) $$
4. For any $$n\times n$$ matrix $$E$$ then $$det(EA) = det(E)det(A)$$

Below are a few properties of the determinant:

- A is an invertible matrix is $$det(A) \neq 0$$
- $$det(AB) = det(A)det(B)$$
- $$det(A^T) = det(A)$$
- If A is an invertible matrix, then $$det(A^{-1}) = \frac{1}{det(A)}$$


## Chapter 4 - Subspaces and Their Properties

Suppose we have an $$n\times m$$ sized matrix, A which has solutions $$u, v$$ to $$Ax=0$$

Then we can say that 

> $$A(u+v) = Au + Av = 0 + 0 = 0$$

### Subspaces

**Definition**: A set of vectors in $$\!R^n$$ is called a **subspace** of $$\!R^n$$ if it has the following three properties

1. The zero vector belongs to W
2. Whenever any two vectors $$u$$ and $$v$$ beloing to W, then $$u + v$$ must also belong to W
3. Whenever $$u$$ belongs to W and c is a scalar, then $$cu$$ belongs to W

**Subspaces Associated with Matrices**

There are many different types of subspaces associated with a matrix.

**Null Space**

The **null space** of a matrix is the solution set of $$Ax = 0$$. This subspace is denoted by $$Null(A)$$

from an $$m\times n$$ matrix A, the null space of A is the set

> $$Null(A) = \{ v \in \!R^n : Av = 0\} $$

**Column Space**

The **column space** of a matrix A is the span of its columns. It is denoted by $$Col(A)$$

For example, given the matrix $$A = \begin{bmatrix} 1 & -5 & 3 \\ 2 & -9 & -6 \\ \end{bmatrix}$$

Then the $$Col(A) = Span\bigg\{ \begin{bmatrix} 1 \\ 2 \\ \end{bmatrix}, \begin{bmatrix} -5 \\ -9 \\ \end{bmatrix}, \begin{bmatrix} 3 \\ -6 \\ \end{bmatrix} \bigg\} $$

**Row Space**

he **row space** of a matrix is defined to be the span of its rows. The row space of a matrix, A, is denoted by $$Row(A)$$ So then given the matrix 

 $$A = \begin{bmatrix} 1 & -5 & 3 \\ 2 & -9 & -6 \\ 1 & 2 & 3 \\ \end{bmatrix}$$

The row space is then 

$$Row(A) = Span\bigg\{ \begin{bmatrix} 1 \\ -5 \\ 3 \\ \end{bmatrix}, \begin{bmatrix} 2 \\ -9 \\ -6 \\ \end{bmatrix}, \begin{bmatrix} 1 \\ 2 \\ 3 \\ \end{bmatrix} \bigg\} $$

### Basis and Dimension

Let $$V$$ be a nonzero subspace of $$\!R^n$$. Then a **basis** for $$V$$ is a linearly independent generating set for $$V$$.

Or in other words, none of the vectors in the set are allowed to be a linear combination of one another.

To find the basis for $$Col(A)$$, we reduce the matrix A into its reduced echelon form. The columns which have pivots are the columns from the original matrix which we then put into the basis (or minimal generating set) for $$Col(A)$$.

This method can then be generalized for any set of vectors, $$W$$ which span a set where we can find the basis.

Let S be a finite subset of $$\!R^n$$. Then we have the following properties.

- If S is a generating set for $$\!R^n$$ then S contains at least $$n$$ vectors
- If S is linearly independent then S contains at most n vectors
- If S is a basis for $$\!R^n$$, then S contains exactly n vectors.

A basis is a linearly independent subset of a subspace that is as large as possible

Another way to describe a basis is that it is a minimal set of generators for a specific space, whether is be a matrix, $$A$$, or $$\!R^n$$

## Chapter 5 - Eigenvalues, Eigenvectors, and Diagonalization

This section explores different representations of matrices which are **diagonalizable**. We first need to explore **eigenvalues** and **eigenvectors** before we can understand how to diagonalize a matrix.

**Definition**: Let T be a linear operator on $$\!R^n$$. A nonzero vector, v, in $$\!R^n$$ is called an **eigenvector** of T is $$T(v)$$ is a multiple of $$v$$. That is, $$Tv = \lambda v$$ for some scalar $$\lambda$$. The scalar value $$\lambda$$ is valled the eigenvalue of $$T$$ which corresponds to $$v$$

In other words, v is an **eigenvector** if it satisfies the equation $$Av=\lambda v$$. Where $$\lambda$$ is a scalar value called an **eigenvalue** of the matrix $$A$$ which corresponds to $$v$$

**How to determine if a vector is an eigenvector of a matrix**

Given $$v = \begin{bmatrix} 1 \\ -1 \\ 1 \\ \end{bmatrix}$$ and $$A = \begin{bmatrix} 5 & 2 & 1 \\ -2 & 1 & -1 \\ 2 & 2 & 4 \\ \end{bmatrix}$$

How do we tell if $$v$$ is an eigenvector of A?

We can simply do the multiplication $$Av$$ first, to find the resultant vector 

$$ Av = \begin{bmatrix} 4 \\ -4 \\ 4 \\ \end{bmatrix}$$ 

We can see from this result that we can factor out a 4 from the resultant matrix

That is $$Av = \begin{bmatrix} 4 \\ -4 \\ 4 \\ \end{bmatrix} = 4\begin{bmatrix} 1 \\ -1 \\ 1 \\ \end{bmatrix}$$

We see the remaining matrix is actually $$v$$, so then we determine that $$v$$ **is indeed an eigenvector**. Also note that this eigenvector corresponds to the **eigenvalue**, 4

**Finding the Eigenvectors of a Matrix**

We can find the eigenvalues for a given matrix by the following:

1. $$Av = \lambda v$$
2. $$Av - \lambda v = 0$$
3. $$Av - \lambda I_nv = 0$$
4. $$(A - \lambda I_n)v = 0$$

> $$ (A - \lambda I_n)v = 0$$

From (4) we can find the eigenvectors of a matrix by simply solving that equation for any eigenvalue, $$\lambda$$, and all of the solutions from that equation will be the result of that solution

**Finding the EigenValues of a Matrix**

So then how can we just find the eigenvalues of a matrix?

We can find the **characteristic equation** of the matrix to get the eigenvalues which correspond to it. The equation for the matrix is simply the solutions for $$t$$ of:

> $$det(A - tI_n) = 0$$

Sometimes the **characteristic equation** is also referred to as the **characteristic polynomial**

For example, let's find the eigenvalues of $$A = \begin{bmatrix} -4 & -3 \\ 3 & 6 \\ \end{bmatrix}$$

Given the equation $$det(A - tI_n) = 0$$

$$det(\begin{bmatrix} -4 & -3 \\ 3 & 6 \\ \end{bmatrix} - t\begin{bmatrix} 1 & 0 \\ 0 & 1 \\ \end{bmatrix}) = 0$$

$$det(\begin{bmatrix} -4 & -3 \\ 3 & 6 \\ \end{bmatrix} - \begin{bmatrix} t & 0 \\ 0 & t \\ \end{bmatrix}) = 0$$

$$det(\begin{bmatrix} -4 - t & -3 \\ 3 & 6 - t \\ \end{bmatrix}) = 0$$

Now we learned how to find the determinant of this matrix before. It is equal to

$$(-4-t)(6-t) - (-3)(3)$$

So then the equation becomes 

$$(-4-t)(6-t) + 9 = 0$$

$$t^2 -2t -24 + 9 = 0$$

$$t^2 -2t -15 = 0$$

$$(t+3)(t-5) = 0$$

This then gives us the solution of $$t = \{-3, 5\}$$

Thus our eigenvalues $$\lambda = \{-3, 5\}$$


Here's another quick definition

> Let $$\lambda$$ be an eigenvalue of a matrix, A. The dimension of the eigenspace of A corresponding to $$\lambda$$ is less than or equal to the multiplicity of $$\lambda$$


### Diagonalization of Matrices

Now that we've learned how to find **eigenvectors** and **eigenvalues** we can now determine whether or not the matrices are diagonalizable. 

Diagonalization means turning a matrix, A, into the form of 

> $$A = PDP^{-1}$$.

This has many unique applications and can make certain operations very easy

First review that for a matrix, such as $$I_n$$ which has all diagonal entries it is very easy to calculate the matrix to any power.

Given a matrix $$ A = \begin{bmatrix} 2 & 0 & 0 \\  0 & 3 & 0 \\ 0 & 0 & 4 \\ \end{bmatrix}$$

Now recall

$$A^5 =  \begin{bmatrix} 2 & 0 & 0 \\  0 & 3 & 0 \\ 0 & 0 & 4 \\ \end{bmatrix}^5 =  \begin{bmatrix} 2^5 & 0 & 0 \\  0 & 3^5 & 0 \\ 0 & 0 & 4^5 \\ \end{bmatrix}$$

Obviously this is very easy to calculate.

But what if our matrix isn't diagonal like that?

Well if we can transfer it to the from of $$A = PDP^{-1}$$ it actually is possible (if the matrix is determined as diagonalizable)

Because if $$A = PDP^{-1}$$

Then $$A^{100} = (PDP^{-1})(PDP^{-1})(PDP^{-1})\dots = PD^{100}P^{-1}$$

> **Definition**: An $$n\times n$$ matrix is diagonalizble if and only if there is a basis for $$\!R^n$$ consisting of eigenvectors of A. Furthurmore, $$A = PDP^{-1}$$ where $$D$$ is a diagonal matrix and P is an invertible matrix, if and only if the columns of P are a basis for $$\!R^n$$ consisting of eigenvectors of A and the diagonal entries of D are the eigenvalues of the respective columns of P

**Theorem**, Every $$n\times n$$ matrix having $$n$$ distinct eigenvalues is diagonalizable

**Testing to see if a matrix is Diagonalizable**

For an $$n\times n$$ matrix A:

- There must be at least $$n$$ distinct **eigenvalues**
- For each **eigenvalue**, $$\lambda$$ of A, the dimension of the corresponding eigenspace which is equal to $$n-rank(A-\lambda I_n)$$ is equal to the multiplicity of $$\lambda$$

## Chapter 6 - Orthogonality

The **norm** of a vector, $$v$$ is: 

$$ \lVert v \rVert = \sqrt{v_1^2 + v_2^2 +\dots + v_n^2} $$

A vector with a **norm** of 1 is called a **unit vector**

The distance between two vectors is simply the norm of the difference of the two vectors.

The distance between $$u$$ and $$v$$ is $$\lVert u - v \rVert $$

We also define the **dot product** of any two n-length vectors to be:

$$ u\cdot v = u_1v_1 + u_2v_2 + \dots + u_nv_n $$

If the **dot product** is **zero**, then the vectors are said to be **orthogonal**

You might have heard pythagorean theorem before, with respect to finding the sides of a triangle. Well, it turns out that the formula holds as well for any vectors of size $$n$$

$$\lVert u + v \rVert^2 = \lVert u \rVert^2 + \lVert v \rVert^2 $$

The **Cauchy-Schwarz Inequality**

For any two vectors $$u$$ and $$v$$, it holds true that:

$$\vert u\cdot v \vert \leq \lVert u \rVert \cdot \lVert v \rVert $$

In other words, the absolute value of the dot product, must always be equal to or less than the norm of both vectors multiplied together.

### Orthogonal Sets of Vectors

Any set of non-zero vectors is always going to be linearly independent.





















































