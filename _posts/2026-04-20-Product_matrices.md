---
title: "Why is the matrix product the way it is?"
date: 2026-04-20
permalink: /posts/2026/04/product_matrices/
tags:
  - linear algebra
  - math
  - matrix
  - matrix multiplication
  - linear transformation
---
*Prerequisites*: A tiny bit of linear algebra, notions on matrices, product of matrices and its relationship with linear transformations.

## 1. Definition of the product

Matrix multiplication is a cornerstone of linear algebra, yet its definition, multiplying rows by columns and summing the result, can initially seem unintuitive. Why do we take the dot product of rows from one matrix with columns from another, instead of multiplying corresponding elements directly? The answer is rooted in the way matrices encode linear transformations: the standard multiplication rule ensures that composing two transformations corresponds exactly to multiplying their matrices. This preserves the structure and properties of linear maps, making matrix multiplication essential for applications in geometry, physics, and computer science. That is what we are going to be doing in this post.

It's worth noting that elementwise multiplication of matrices does exist and is called the **Hadamard product**. While not the same as standard matrix multiplication, the Hadamard product is useful in various contexts, such as in neural network backpropagation and other areas of computer science.

We begin by defining the product of two matrices. Given two matrices $$A$$ and $$B$$, where $$A$$ is of size $$m \times n$$ and $$B$$ is of size $$n \times p$$, their product $$C = AB$$ is defined as the $$m \times p$$ matrix whose $$(i, j)$$-th entry is:

$$
C_{ij} = \sum_{k=1}^n A_{ik} B_{kj}
$$

That is, to compute the entry in row $$i$$ and column $$j$$ of the product, take the $$i$$-th row of $$A$$ and the $$j$$-th column of $$B$$, multiply their corresponding entries, and sum the results.

In this post, we will demonstrate the stated property using explicit calculations with matrices and some basic linear algebra. However, it is worth mentioning that this property has a much broader context when viewed through the lens of category theory. In particular, the definition of matrix multiplication arises naturally from the structure of the categories of matrices (matrices are in fact the morphisms in the category we are mentioning) and vector spaces, and can be understood as a consequence of a functor between these categories.

That said, we will not delve into this level of abstraction here. My aim in this post is to keep the discussion concrete and accessible. In the future, I plan to introduce some ideas from category theory, at which point we can revisit this topic and explore how matrix multiplication fits into that broader mathematical framework.

## 2. How matrices act on vectors

Before thinking about multiplying two matrices, we should understand something more basic:

> Why does multiplying a matrix by a vector represent a linear transformation at all?

### 2.1: A linear map is determined by what it does to a basis

Let $$T: \mathbb{R}^n \to \mathbb{R}^m$$ be linear.
Let $$e_1, \dots, e_n$$ be the canonical basis of $$\mathbb{R}^n$$.

Any vector $$x \in \mathbb{R}^n$$ can be written as:
$$
x = \sum_{j=1}^n x_j e_j
$$

By linearity:
$$
T(x) = T\left(\sum_{j=1}^n x_j e_j\right)
= \sum_{j=1}^n x_j T(e_j)
$$

So everything reduces to knowing the vectors $$T(e_j)$$. This is a recurring result in linear algebra.

---

### 2.2: Encode the transformation as a matrix

Now define a matrix $$A$$ whose **columns** are exactly these images:
$$
A = \begin{bmatrix}
| & & | \
T(e_1) & \cdots & T(e_n) \
| & & |
\end{bmatrix}
$$

So the $$j$$-th column of $$A$$ is $$T(e_j)$$. $$A$$ is the matrix that encodes the linear transformation $$T$$. Sometimes it is denoted by $$[T]$$.

Now, $$Ax$$ should be the vector $$T(x)$$. Using 2.1 and 2.2:

$$
Ax = T(x) = \sum_{j=1}^n x_j T(e_j) = \sum_{j=1}^n x_j A_{\cdot j}
$$
(where $$A_{\cdot j}$$ is the $$j$$-th column). Or explicitly using indexes:

$$
(Ax)_i = \sum_{j=1}^n x_j A_{i j} \quad \forall i
$$

This is exactly the standard rule of multiplication of a matrix by a column vector! The one we see in high-school. Indeed the first component of $$(Ax)$$ is the first row of the matrix multiplied by the vector $$x$$ and so on.

Matrix–vector multiplication is not an arbitrary rule.

It is the **only natural way** to extend the action of a linear map from basis vectors to arbitrary vectors using linearity.

## 3. Matrix Product as Composition of Linear Maps

We denote the space of real $$m \times n$$ matrices by $$M_{m,n}(\mathbb{R})$$, and the space of linear transformations from $$\mathbb{R}^n$$ to $$\mathbb{R}^m$$ by $$L(\mathbb{R}^n, \mathbb{R}^m)$$. There is a natural correspondence between these two spaces: every linear transformation can be represented by a unique matrix (once bases are chosen) just like we did in section $$2$$, and every matrix defines a linear transformation.

Suppose $$A$$ is an $$m \times n$$ matrix and $$B$$ is an $$n \times p$$ matrix. Each matrix represents a linear map:

- $$B: \mathbb{R}^p \to \mathbb{R}^n$$
- $$A: \mathbb{R}^n \to \mathbb{R}^m$$

The composition $$A \circ B$$ is a map $$\mathbb{R}^p \to \mathbb{R}^m$$ defined by $$x \mapsto A(Bx)$$.

Let’s compute the result explicitly. Let $$x \in \mathbb{R}^p$$ be a column vector. Then:

1. **Apply $$B$$:** $$y = Bx$$, where $$y \in \mathbb{R}^n$$.
2. **Apply $$A$$:** $$z = Ay = A(Bx)$$, where $$z \in \mathbb{R}^m$$.

Let’s write this out in components:

- The $$k$$-th entry of $$y$$ is $$y_k = \sum_{j=1}^p B_{kj} x_j$$ as we did in Section $$2$$.
- The $$i$$-th entry of $$z$$ is:
  $$
  z_i = \sum_{k=1}^n A_{ik} y_k = \sum_{k=1}^n A_{ik} \left( \sum_{j=1}^p B_{kj} x_j \right )
  $$

Switch the order of summation:
$$
z_i = \sum_{j=1}^p \left( \sum_{k=1}^n A_{ik} B_{kj} \right ) x_j
$$

This shows that the matrix representing the composition $$A \circ B$$ is the matrix $$C$$ with entries $$C_{ij} = \sum_{k=1}^n A_{ik} B_{kj}$$—which is exactly the definition of the matrix product.

**Conclusion:**  
There are two main ideas I want to pass in this post. First, that Matrix–vector multiplication is forced by linearity. Second, Matrix–matrix multiplication is forced by composition. Therefore, Matrix multiplication is not a definition we chose—it is the only rule compatible with how linear transformations must behave.



