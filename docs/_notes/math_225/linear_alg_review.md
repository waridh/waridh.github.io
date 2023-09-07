---
title: "Linear algebra concept reviews"
topic: math225
---

## Matrix Operations

Matrix multiplication can be done with the following:

### Matrix Multiplication

You can multiply matrix $$A$$ with dimensions $$m\times n$$ with matrix $$B$$ with dimensions $$k\times l$$ when $$n$$ and $$k$$ are equal. It is also important to note that matrices are indexed with rows first, and the column second.

$$
\begin{bmatrix}
\cdots & R_1 & \cdots \\
\cdots & R_2 & \cdots \\
& \vdots & \\
\cdots & R_n & \cdots
\end{bmatrix} \times \begin{bmatrix}
\vdots & \vdots &  & \vdots \\
C_1 & C_2 & \cdots & C_m\\
\vdots & \vdots &  & \vdots
\end{bmatrix} \\
= \begin{bmatrix}
R_1\cdot C_1 & R_1\cdot C_2 & \cdots & R_1\cdot C_m \\
R_2 \cdot C_1 & R_2 \cdot C_2 & \cdots & R_2\cdot C_m \\
\vdots & \vdots & \ddots & \vdots \\
R_n \cdot C_1 & R_n \cdot C_2 & \cdots & R_n \cdot C_m
\end{bmatrix}
$$

### Matrix Addition

Matrix addition only works on matrices that are of the same dimensions. Otherwise, it is simple, as you simply add the values with the same index together.

$$
\begin{bmatrix}
A_{(1,1)} & A_{(1, 2)} & \cdots & A_{(1, n)} \\
A_{(2, 1)} & A_{(2, 2)} & \cdots & A_{(2, n)} \\
\vdots & \vdots & \ddots & \vdots \\
A_{(m, 1)} & A_{(m, 2)} & \cdots & A_{(m, n)}
\end{bmatrix} \times \begin{bmatrix}
B_{(1,1)} & B_{(1, 2)} & \cdots & B_{(1, n)} \\
B_{(2, 1)} & B_{(2, 2)} & \cdots & B_{(2, n)} \\
\vdots & \vdots & \ddots & \vdots \\
B_{(m, 1)} & B_{(m, 2)} & \cdots & B_{(m, n)}
\end{bmatrix} \\
 = \begin{bmatrix}
A_{(1, 1)}\times B_{(1, 1)} & A_{(1, 2)}\times B_{(1, 2)} & \cdots & A_{(1, n)}\times B_{(1, n)} \\
A_{(2, 1)} \times B_{(2, 1)} & A_{(2, 2)} \times B_{(2, 2)} & \cdots & A_{(2, n)}\times B_{(2, n)} \\
\vdots & \vdots & \ddots & \vdots \\
A_{(m, 1)} \times B_{(m, 1)} & A_{(m, 2)} \times B_{(m,2)} & \cdots & A_{(m, n)} \times B_{(m, n)}
\end{bmatrix}
$$

### Identity Matrix

The identity matrix is a matrix that when matrix multiplied with will return the first matrix. It is simply a diagonal matrix made of 1.

$$
\begin{bmatrix}
1 & 0 & \cdots & 0 \\
0 & 1 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1
\end{bmatrix}
$$

### Zero Matrix

A zero matrix is just a matrix entirely made up of zeros.

## Solving Linear Systems

Usually, you will have a system of linear equations that needs to be solved, and linear algebra provides a tool to solve this in a more manageable way. For example, you are provided with the following set of equations:

$$
6x + 3y + 8z = 13 \\
y + z = 0
$$

You can find out what many solutions there are to this system, and if what those solutions are. If there are more than one free variable, then you are able to find the representation using just free variables. This can all be done by finding the Reduced Row Echelon Form (RREF).

The first step here is to convert the system into an augmented matrix, which is simply laying out the equation as rows on a matrix. Here is an example:

$$
\begin{pmatrix}
6 & 3 & 8 & 13 \\
0 & 1 & 1 & 0
\end{pmatrix}
$$

There are three things that you can do to the rows in this matrix:

1. Multiply the row
2. Change the location of the row
3. Add multiple rows together

The goal is to change the matrix to a reduced row echelon form. The definition of row echelon form is a state where each row of the matrix is either all zeros or there is a leading entry. The zero rows are at the bottom of the matrix, and the rows with entries must have the leading entry to the right of the row above it. If a column contains a leading entry, then the rest of the column must be zero.

Converting our example to RREF, we get the following:

$$
R_1 = -3R_2 + R_1 \rightarrow
\begin{pmatrix}
6 & 0 & 5 & 13 \\
0 & 1 & 1 & 0
\end{pmatrix} \\
R_1 = \frac{R_1}{6} \rightarrow \begin{pmatrix}
1 & 0 & \frac{5}{6} & \frac{13}{6} \\
0 & 1 & 1 & 0
\end{pmatrix}
$$

Which is equivalent to the following set of equations:

$$
x + \frac{5}{6} z = \frac{13}{6} \\
y + z = 0
$$

From looking at this, you can see that there are infinite solutions to this system of equations. You can change the definition of all the variable to be based on the free variable. Let's say that z is the free variable:

$$
z = s \\
x = \frac{13}{6} - \frac{5}{6}s \\
y = -s
$$

And the variables can now be described as follows:

$$
\begin{pmatrix}
x \\
y \\
z
\end{pmatrix} = \begin{pmatrix}
\frac{13}{6} - \frac{5}{6}s \\
-s \\
s
\end{pmatrix}
$$

### Types of solutions

There are three types of solution possible for a system of linear equations. These are no solutions, one unique solution, and infinite solutions. The following is how you can tell what type of solution a system has after finding the row reduced form:

- **No Solutions** - There are no solution when there is a row where all the variable columns are zero, but the final entry is not zero as well. Here is an example: $$ \begin{pmatrix}0 & 0 & \cdots & 0 & 1 \end{pmatrix} $$

- **One solution** - When every row of the REF or RREF has a leading entry.
- **Infinitely many solution** - When there is at least one leading entry/pivot, but there is also at least one row of complete zero.

### Rank

The rank of a matrix is the amount of rows with a leading entry/pivot.

## Inverse of Matrix

The property of the inverse of a matrix is that when they are multiplied, the resulting matrix is just an identity matrix. This definition only applied to square matrices. Here are some equations that demonstrates the properties of an inverse matrix, where $$A$$ and $$B$$ are matrices.

$$
A\times B = I; B \times A = I \therefore B = A^{-1}
$$

The inverse of a matrix can also be found by using RREF. Here is the setup for how it can be done:

$$
\begin{pmatrix}
A & | & I
\end{pmatrix} \rightarrow \begin{pmatrix}
I & | & A^{-1}
\end{pmatrix}
$$

Where $$A$$ is a square matrix, and $$I$$ is an identity matrix with a matching dimension.

## Subspaces

A subspace of $$\mathbb{R}^n$$ is a collection of vectors $$S \in \mathbb{R}^n$$ such that:

1. It has a zero vector: $$\begin{bmatrix}0 \\
0 \\
\vdots \\
0
\end{bmatrix}$$
2. It is closed under scalar multiplication meaning that the output of the scalar multiplication where $$c \in \mathbb{R}$$ is also a part of the subspace.
3. If $$\vec{w_1}, \vec{w_2} \in S$$, then $$\vec{w_1} + \vec{w_2} \in S$$.

### Null-space

For every $$n\times n$$ matrix $$A$$, the null-space $$null\left(A\right) =\left\{\text{all vectors }v\text{ s.t. }Av=0\right\}$$.

$$
\text{e.g. } A = \begin{pmatrix}4 & 2 \\
0 & 0
\end{pmatrix} 
$$
$$
null\left(A\right) = \left\{v | Av=\begin{pmatrix}0\\0\end{pmatrix}\right\} $$
$$
\downarrow \text{can be found with RREF} $$
$$
\begin{pmatrix}4 & 2 \\ 0 & 0\end{pmatrix}\begin{pmatrix}x \\ y\end{pmatrix}=\begin{pmatrix}0 \\ 0\end{pmatrix} $$
$$
\downarrow \text{Convert to RREF} $$
$$
\begin{pmatrix}1 & \frac{1}{2} \\ 0 & 0\end{pmatrix}\begin{pmatrix}x \\ y\end{pmatrix}=\begin{pmatrix}0 \\ 0\end{pmatrix} $$
$$
x + \frac{y}{2} = 0; x = -\frac{y}{2} $$
$$
y = u; x = -\frac{u}{2} $$
$$
\begin{pmatrix}x \\ y\end{pmatrix} = \begin{pmatrix}-\frac{u}{2} \\ u\end{pmatrix} = u\begin{pmatrix}-\frac{1}{2} \\ 1\end{pmatrix} $$
$$
null\left(A\right) = \left\{\forall v | u \begin{pmatrix}-\frac{1}{2} \\ 1\end{pmatrix}, u\in\mathbb{R}\right\} $$

You will need to check if this is a subspace

1. $$ \text{Zero vector: } 0 = 0\begin{pmatrix}-\frac{1}{2} \\ 1\end{pmatrix}$$
2. $$\text{Is the scalar under closure?: for all vectors } v \in null\left(A\right) \\ Av=0 \therefore A(cv)=cAv=0$$
3. $$\vec{v}=\begin{pmatrix}1 \\ -2\end{pmatrix},\vec{w}=\begin{pmatrix}-2 \\ 4\end{pmatrix}\in null\left(A\right) \\ \vec{v}+\vec{w} = \begin{pmatrix} 0 \\ 0 \end{pmatrix} \in null\left(A\right)$$

And thus from this check, we know that $$null\left(A\right)$$ is a subspace.

### Row spaces

$$row\left(A\right) = \begin{cases}\text{Set of vectors whih are linear combinations of rows of A}\end{cases}$$

#### Steps

1. Put into $$RREF\left(A\right)$$
2. Basis of $$row\left(A\right)$$ is all rows of RREF with non-zero leading entries.

### Column space

$$col\left(A\right) = \begin{cases}\text{Set of vectors which are a linear combination of the columns of A}\end{cases}$$

#### Steps

1. Find the $$RREF\left(A\right)$$.
2. Take the columns of the original matrix that have a leading entry on the RREF.

## Determinant

Determinant: $$ det\left(A\right)$$ is a system that acts on in input of an $$n\times n$$ matrix that creates a number output that follows that following rules:

1. $$det\left(I_n\right)=1$$
2. when A swaps rows $$ det\left(A\right) = - det\left(A'\right) $$
3. Swapping two columns $$det\left(A\right) = -det\left(A'\right)$$
4. Scaling one row by factor $$c$$: $$cdet\left(A\right)= det\left(A'\right)\therefore det\left(cA\right) = c^n det\left(A\right)$$
5. When you add one row to another: $$det\left(A\right) = det\left(A'\right)$$
6. If A has at least one row of zeros, $$det\left(A\right)=0$$

### Algorithm

Finding the determinant of a square matrix is a recursive process. The set-up is the following:

#### Base case

The base case of this process is the determinant of a $$2\times 2$$ matrix:

$$
\begin{pmatrix}a & b \\ c & d\end{pmatrix} = ad-bc
$$

#### Other cases

If bigger, the matrix will get reduced by doing the following demonstrated on a $$3\times 3$$ matrix.

1. The index of the matrix is mapped out with positives and negatives:
$$
\begin{pmatrix}
a & b & c \\
d & e & f \\
g & h & i 
\end{pmatrix} \rightarrow
\begin{pmatrix}
+ & - & + \\
- & + & - \\
+ & - & + 
\end{pmatrix}
$$

2. Pick a column or row with the largest amount of zeros, and then run this operation on all of them. In this example, we will be using $$\begin{pmatrix}a & b & c\end{pmatrix}$$: $$\begin{pmatrix} a & \cdots & \cdots \\
\vdots & e & f \\
\vdots & h & i \end{pmatrix} \rightarrow a\begin{vmatrix}e & f \\ h & i\end{vmatrix}$$

From here the determinant of the $$2 \times 2$$ matrix can be found, as it is the base case.

## Linear Independence

When two vectors are not scalar vectors, they are said to be independent. This applies to a set of more than two vectors as well. To check if a set of vectors are linearly independent, one can reduce the matrix of that set down to the identity matrix. If that is not possible, then the set of vectors are not linearly independent.

### Example

Consider the set of vectors $$
\left\{
  \begin{bmatrix}1 \\ 0 \\ 1\end{bmatrix},
  \begin{bmatrix}-2 \\ 1 \\ 4\end{bmatrix},
  \begin{bmatrix}0 \\ 3 \\ 1\end{bmatrix}
  \right\}$$.

We want to check if this set of vectors are linearly independent.

$$
\begin{bmatrix}
  1 & -2 & 0 \\
  0 & 1 & 3 \\
  1 & 4 & 1
\end{bmatrix}
$$

Subtract $$R_3$$ with $$R_1$$ yields the following:

$$
\begin{bmatrix}
  1 & -2 & 0 \\
  0 & 1 & 3 \\
  0 & 6 & 1
\end{bmatrix}
$$

Then, we do the following:

$$
R_3 = R_3 - 6R_2 \rightarrow \begin{bmatrix}
  1 & -2 & 0 \\
  0 & 1 & 3 \\
  0 & 0 & -17
\end{bmatrix}
$$

$$
R_3 = \frac{R_3}{-17} \rightarrow \begin{bmatrix}
  1 & -2 & 0 \\
  0 & 1 & 3 \\
  0 & 0 & 1
\end{bmatrix}
$$

$$
R_2 = -3R_3 + R_2 \rightarrow \begin{bmatrix}
  1 & -2 & 0 \\
  0 & 1 & 0 \\
  0 & 0 & 1
\end{bmatrix}
$$

$$
R_1 = 2R_2 + R_1 \rightarrow \begin{bmatrix}
  1 & 0 & 0 \\
  0 & 1 & 0 \\
  0 & 0 & 1
\end{bmatrix}
$$

From this, we know that the set is linearly independent. The starting matrix was
transformed into an identity matrix with three pivots. This means that we have
three linearly independent vectors.

### Second method

To check if the set of vectors are linearly independent, we can also find the determinant of the matrix, and if it is any value that is not zero, then the set is linearly independent, however, if the determinant of the matrix is zero, then the set is linearly dependent.

## Nullity of a Matrix

Just the complement of the rank of a matrix. To find this, you simply have to subtract the number of columns with the rank of the matrix, which is the same as saying subtracting the number of variables by the rank of the matrix.

The nullity of a matrix represents the number of free parameters in the system.

## Basis

### Representing Basis in $$\mathbb{R}^n$$.

- Since a basis in an $$\mathbb{R}^n$$ vector space will be able to represent any vector in that vector space through linear combination, it is necessary that there are n vectors in that basis set of vectors.
- Linear transforms that are done on a basis will leave the resulting set also be a basis.

### Finding the basis of a subspace

- Subspaces are a little different as it may not be necessary to be able to represent every vector in $$\mathbb{R}^n$$.
- To find a basis in a subspace, you just need to use the definition of the subspace to define an augmented matrix. Once this is done, just take the RREF and write all the solution in terms of the free variable.

## Eigenvalues and Eigenvectors

Both eigenvalues and eigenvectors are used in analysis of linear transformations. The linear transformation are represented as matrices.

Eigenvectors are vectors that do not change the direction when applied with a certain linear transform. When the linear transform is applied to them, only the scalar transformation is applied. When this scalar transformation is applied, the value of the scalar transformation is the same as the eigenvalue of the transformation matrix.

Where $$A$$ is the transformation matrix, $$x$$ is the eigenvector, and $$\lambda$$ is the eigenvalue, the following equation demonstrate the property of an eigenvector.

$$
Ax = \lambda x
$$

To find the eigenvector, the eigenvalue must be found first. The property which will help us find the eigenvalue is that the determinant of the characteristic matrix is 0. A zero vector is not an eigenvector, even though it satisfies all the properties required by the eigenvector. The characteristic matrix of a transformation matrix has the following equation.

$$
A - \lambda I
$$

Where $$A$$ is the transformation matrix, $$\lambda$$ is the eigenvalue, and $$I$$ is an identity matrix.

From here, write out the determinant equation, and solve for $$\lambda$$ such that the determinant of the characteristic equation is equal to zero.

Let's do an example:

Find the eigenvalue of the following transformation matrix:

$$
A = \begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix}
$$

$$
A - \lambda I = \begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix} - \begin{bmatrix}
\lambda & 0 & 0 \\
0 & \lambda & 0 \\
0 & 0 & \lambda
\end{bmatrix} = \begin{bmatrix}
1 - \lambda & 2 & 3 \\
4 & 5 - \lambda & 6 \\
7 & 8 & 9 - \lambda
\end{bmatrix}
$$

Find the determinant of the characteristic matrix.

$$
\begin{vmatrix}
1 - \lambda & 2 & 3 \\
4 & 5 - \lambda & 6 \\
7 & 8 & 9 - \lambda
\end{vmatrix} = 0
$$
$$
 = \left(1-\lambda\right)
\begin{vmatrix}
5 - \lambda & 6 \\
8 & 9 - \lambda
\end{vmatrix} - \left(2\right)
\begin{vmatrix}
4 & 6 \\
7 & 9 - \lambda
\end{vmatrix} + \left(3\right)
\begin{vmatrix}
4 & 5 - \lambda \\
7 & 8
\end{vmatrix} = 0
$$

$$
= \left(1-\lambda\right)\left(\left(5-\lambda\right)\left(9-\lambda\right)-48\right) - 2\left(\left(36-4\lambda\right)-42\right) + 3\left(32-\left(35 - 7\lambda\right)\right) = 0
$$

$$
= \left(1-\lambda\right)\left(\lambda^2-14\lambda-3\right) - 2\left(-6-4\lambda\right) + 3\left(7\lambda -3\right) = 0
$$

$$
= \lambda^2-14\lambda-3-\lambda^3+14\lambda^2+3\lambda + 8\lambda + 12 + 21\lambda - 9 = 0
$$

$$
= -\lambda^3 + 15\lambda^2 + 18\lambda = 0
$$

$$
= -\lambda \left(\lambda^2 -15\lambda-18\right) = 0
$$

What we have here is the characteristic polynomial, and it is used to find the eigenvalues. The eigenvalues can be obtained by finding the root of the characteristic polynomial.

Solving for the quadratic

$$
\frac{15\pm\sqrt{15^2-4\left(1\right)\left(-18\right)}}{2\left(1\right)} = \frac{15\pm\sqrt{297}}{2} \\
= \frac{3\sqrt{33} + 15}{2} ; \frac{-3\sqrt{33}+15}{2}
$$

$$
\therefore \lambda = 0 ; \frac{3\sqrt{33} + 15}{2} ; \frac{-3\sqrt{33}+15}{2}
$$

After finding the eigenvalue of a matrix, the eigenvector can be derived. Since we are looking for a vector that when multiplied with the characteristic matrix, would result in a value of 0. Unlike eigenvectors, eigenvalues are allowed to have a value of zero.

$$
\left(A-\lambda I\right)v = 0
$$

Where v is the eigenvector.

Let's continue the example that was started earlier in the section, and use the eigenvalue of 0 fir simplicity sake.

$$
Eigen = \begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix}
$$

We want to find the eigenvector for this characteristic matrix. This can be done with the following.

$$
\begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix} \begin{bmatrix}
x \\
y \\
z \\
\end{bmatrix} = \begin{bmatrix}
0 \\
0 \\
0 \\
\end{bmatrix}
$$

$$
\begin{bmatrix}
x + 2y + 3z \\
4x + 5y + 6z \\
7x + 8y + 9z
\end{bmatrix} = \begin{bmatrix}
0 \\
0 \\
0 \\
\end{bmatrix}
$$

We can solve this by doing row reduction

$$
\begin{bmatrix}
1 & 2 & 3 & 0 \\
4 & 5 & 6 & 0 \\
7 & 8 & 9 & 0
\end{bmatrix}
$$

$$
R_2 = -4R_1 + R_2 \rightarrow \begin{bmatrix}
1 & 2 & 3 & 0 \\
0 & -3 & -6 & 0 \\
7 & 8 & 9 & 0
\end{bmatrix}
$$

$$
R_3 = -7R_1 + R_3 \rightarrow \begin{bmatrix}
1 & 2 & 3 & 0 \\
0 & -3 & -6 & 0 \\
0 & -6 & -12 & 0
\end{bmatrix}
$$

$$
R_3 = -2R_2 + R_3 \rightarrow \begin{bmatrix}
1 & 2 & 3 & 0 \\
0 & -3 & -6 & 0 \\
0 & 0 & 0 & 0
\end{bmatrix}
$$

$$
R_2 = \frac{R_2}{-3} \rightarrow \begin{bmatrix}
1 & 2 & 3 & 0 \\
0 & 1 & 2 & 0 \\
0 & 0 & 0 & 0
\end{bmatrix}
$$

$$
R_1 = -2 R_2 + R_1 \rightarrow -\begin{bmatrix}
1 & 0 & -1 & 0 \\
0 & 1 & 2 & 0 \\
0 & 0 & 0 & 0
\end{bmatrix}
$$

Since the matrix only has two pivots, and there are three variables, there exists a free variable.

$$
x - z = 0 \\
y + 2z = 0 \\
$$

$$
x = z \\
y = -2z
$$

If $$z = s$$ then:

$$
\begin{pmatrix}
x \\
y \\
z
\end{pmatrix} = \begin{pmatrix}
s \\
-2s \\
s
\end{pmatrix}
$$

That final answer is one of the three eigenvector as a function of the free variable, which also acts as a scalar.

## Characteristic Polynomials (Eigenpolynomial)

When you have an $$n \times n$$ matrix called $$A$$, the characteristic polynomial of $$A$$ is denoted as $$f\left(\lambda\right)$$. The formula for the characteristic polynomial is given by $$f\left(\lambda\right) = det\left(A-\lambda I_n\right)$$ which when using the terms that we built up, would be the determinant of the characteristic function.

You find the characteristic polynomial to find the eigenvalue. This is very similar to the work that we have done earlier to find the eigenvalues. In fact, the characteristic polynomial is an intermidiate step before getting the resulting eigen values.

The eigenvalues can be extracted from the characteristic polynomial by finding the roots of the polynomial.

- If the polynomial is 0, then $$A$$ is an identity matrix $$I$$.
- You can find out if the matrix is diagonalizeble if the eigenvectors make up a basis.
- If a matrix has an eigenvalue of 0, then that matrix is not invertible.
