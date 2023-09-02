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

Subtract $R_3$ with $R_1$ yields the following:

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
