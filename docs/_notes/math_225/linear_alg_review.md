---
title: "Linear algebra concept reviews"
topic: math225
---

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

## Basis

### Representing Basis in $$\mathbb{R}^n$$.

- Since a basis in an $$\mathbb{R}^n$$ vector space will be able to represent any vector in that vector space through linear combination, it is necessary that there are n vectors in that basis set of vectors.
- Linear transforms that are done on a basis will leave the resulting set also be a basis.

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
\frac{15\pm\sqrt{15^2-4\left(1\right)\left(-18\right)}}{2\left(1\right)} = \frac{15\pm\sqrt{297}}{2} = \frac{3\sqrt{33} + 15}{2} ; \frac{-3\sqrt{33}+15}{2}
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

$$
x - z = 0 \\
y + 2z = 0 \\
$$

$$
x = z \\
y = -2z \therefore z = -\frac{y}{2}
$$

If `x == 1` then:

$$
x = 1; z =1; y = -2 \\
\therefore v = \begin{bmatrix}
1 \\
-2 \\
1
\end{bmatrix}
$$

## Characteristic Polynomials (Eigenpolynomial)

When you have an $$n \times n$$ matrix called $$A$$, the characteristic polynomial of $$A$$ is denoted as $$f\left(\lambda\right)$$. The formula for the characteristic polynomial is given by $$f\left(\lambda\right) = det\left(A-\lambda I_n\right)$$ which when using the terms that we built up, would be the determinant of the characteristic function.

You find the characteristic polynomial to find the eigenvalue. This is very similar to the work that we have done earlier to find the eigenvalues. In fact, the characteristic polynomial is an intermidiate step before getting the resulting eigen values.

The eigenvalues can be extracted from the characteristic polynomial by finding the roots of the polynomial.

- If the polynomial is 0, then $$A$$ is an identity matrix $$I$$.
