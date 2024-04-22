---
layout: post
usehighlight: true
tags: [math, deep learning, algebra]
title: Revisiting Foundamental Mathematics in Machine Learning
---


This post is the note for reviewing the fundamental mathematics in machine learning, especially, deep learning area. We will briefly revisit the basic concepts and useful tools in the analysis of ML algorithms.

Here are some excellent materials you may find helpful:

1. [Algebra, Topology, Differential Calculus, and Optimization Theory For Computer Science and Machine Learning](https://www.cis.upenn.edu/~jean/math-deep.pdf)
2. [The matrix cookbook](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf)
3. [Matrix theory- Basic Results and Techniques](https://link.springer.com/book/10.1007/978-1-4614-1099-7)
  
In fact, most of the contents in this post derive from [1].

# Table of Contents
- [Table of Contents](#table-of-contents)
  - [Preliminaries](#preliminaries)
    - [Matrices and Linear Algebra](#matrices-and-linear-algebra)
    - [Curves, Scalar Fields, and Gradients](#curves-scalar-fields-and-gradients)

## Preliminaries

**Note**: This post assumes you have the preliminary knowledge of simple calculus in univariate case and linear algebra. Here provides some concepts which help us see the things in an abstract way (meaning that we then know where can or cannot we generalize our analysis to).

### Matrices and Linear Algebra

Before we proceed, the matrices are useful tools in every aspect in practice. Intuitively, matrices denote the linear mappings between two vector spaces (with appropriate basis defined). You would definitely use the matrix notations when dealing with the problems involve with transformations between vector space, and the perspective of vector space would definitely help when memorizing the concepts.

A set $V$ is called a **vector space** over a field $\mathbb{F}$ if the following hold:

* $u+v$, $u,v\in V$ and $cv$, $c\in\mathbb{F}$, $v\in V$ are defined.
* $u+v \in V$ for all $u, v \in V$.
* $c v \in V$ for all $c \in \mathbb{F}$ and $v \in V$.
* $u+v=v+u$ for all $u, v \in V$.
* $(u+v)+w=u+(v+w)$ for all $u, v, w \in V$.
* There is an element $0 \in V$ such that $v+0=v$ for all $v \in V$.
* For each $v \in V$ there is an element $-v \in V$ so that $v+(-v)=0$.
* $c(u+v)=c u+c v$ for all $c \in \mathbb{F}$ and $u, v \in V$.
* $(a+b) v=a v+b v$ for all $a, b \in \mathbb{F}$ and $v \in V$.
* $(a b) v=a(b v)$ for all $a, b \in \mathbb{F}$ and $v \in V$.
* $1 v=v$ for all $v \in V$.

Some examples of vector space are:

* $\mathbb{F}_n(x)$, the collections of polynomials over a field $\mathbb{F}$ with degrees at most $n$, w.r.t the ordinary addition and scalar multiplication.
* $\mathbb{F}^k_n$, the set of all polynomials of degree at most $n$ in $k$ variables.
* $C[a,b]$, the set of all (real-valued) continuous functions on $[a,b]$.
* $C'(x)$, the set of all functions of continuous derivatives on $\mathbb{R}$.
* The set of all even functions.
* The set of all odd functions.
* The set of set of all functions $f$ such that $f(0)=0$.

Let $S$ be a nonempty subset of a vector space $V$ over a field $\mathbb{F}$. Denote by **Span**$S$ the collection of all finite linear combinations of the vectors in $S$; that is, Span$S$ consists of all vectors of the form
$$
c_1 v_1+c_2 v_2+\cdots+c_t v_t, \quad t=1,2, \ldots, c_i \in \mathbb{F}, v_i \in S,
$$

Note that:

* Span $S$ is also a vector space.
* $S$ spans the vector space $V$ if Span$S=V$.

A set $S=\lbrace v_1, v_2, \ldots, v_k\rbrace$ is said to be **linearly independent** if
$$
c_1 v_1+c_2 v_2+\cdots+c_k v_k=0
$$
holds only when $c_1=c_2=\cdots=c_k=0$. If there are also nontrivial solutions, i.e., not all $c$ are zero, then $S$ is **linearly dependent**.

A basis of a vector space $V$ is a linearly independent set that spans $V$. The number of elements in a basis of $V$ is called the **dimension** of $V$, denoted as dim$V$. We write dim$V=0$ if $V=\lbrace 0\rbrace$. We write dim$V=\infty$ if no finite set could span $V$.

If $\lbrace u_1, u_2, \ldots, u_n\rbrace$ is a basis for a vector space $V$, then every $x$ in $V$ can be uniquely expressed as a linear combination of the basis vectors:
$$
x=x_1 u_1+x_2 u_2+\cdots+x_n u_n,
$$
where the $x_i\in\mathbb{F}$. The $n$-tuple $\left(x_1, x_2, \ldots, x_n\right)$ is called the **coordinate** of vector $x$ w.r.t the basis.

Let $W$ be a subset of a vector space $V$. If $W$ is also a vector space under the addition and scalar multiplication for $V$, then $W$ is called a **subspace** of $V$. We have $W$ is a subspace iff $W$ is closed under the addition and scalar multiplication.

The **sum of subspaces** $V_1$, $V_2$ is defined as $V_1+V_2=\lbrace v_1+v_2\mid v_1\in V_1, v_2\in V_2\rbrace$. This sum is also a subspace, and also the intersection of $V_1$ and $V_2$ is a subspace. We have $V_1\bigcap V_2\subseteq V_i\subseteq V_1+V_2$. The sum $V_1+V_2$ is called a **direct sum**, symbolized by $V_1 \oplus V_2$, if $v_1+v_2=0, v_1 \in V_1, v_2 \in V_2 \Rightarrow v_1=v_2=0$. $V_1+V_2$ is a direct sum iff $\dim(V_1+V_2)=\dim V_1 +\dim V_2$. If $\lbrace u_1, \ldots, u_s\rbrace$ is a basis for $V_1$ and $\lbrace v_1, \ldots, v_t\rbrace$ is a basis for $V_2$, then $\lbrace u_1, \ldots, u_s, v_1, \ldots, v_t\rbrace$ is a basis for $V_1 \oplus V_2$.

To study the properties of the vector spaces, we have the following:

* Let $\lbrace v_1, v_2, \ldots, v_k\rbrace$ be a linearly independent subset of $V$, if $\dim V=n$, then $k\leq n$, and if $k<n$, then there exists a vector $v_{k+1}\in V$ such that $\lbrace v_1, v_2, \ldots, v_{k+1}\rbrace$ is linearly independent.
* Let $V_1$ and $V_2$ be subspaces of $V$. Then $\dim V_1 + \dim V_2 = \dim (V_1+V_2)+\dim (V_1\bigcap V_2)$.

A **matrix** $A$ with $m$ rows and $n$ columns is an $m\times n$ rectangular array $(a_{ij})$ whose entries are in some field $\mathbb{F}$. All the $m\times n$ matrices over field $\mathbb{F}$ constitute a vector space $\mathbb{M}_{m\times n}(\mathbb{F})$, whose basis are the matrices with only one entry's value equals to 1 and 0 for all the other entries. Some basic matrix operations are:

**Addition:** $A+B=(a_{ij}+b_{ij})$

**Multiplication:** $AB=(a_{i1}b_{1j}+a_{i2}b_{2j}+...+a_{in}b_{nj})$ or in column vectors form: $AB=(Ab_1, Ab_2, ...,Ab_n)$

**Transpose:** $A^T=(a_{ji})$

**Conjugate:** $\bar{A}=(\bar{a}_{ij})$

**Conjugate Transpose:** $A^*=(\bar{a}_{ji})$

**Adjoint:** $\text{adj}A=((-1)^{j+i}\det A(j\mid i))$

A square complex matrix $A=\left(a_{i j}\right)$ is said to be

* diagonal if $a_{i j}=0, i \neq j$
* upper-triangular if $a_{i j}=0, i>j$
* symmetric if $A^T=A$
* orthogonal if $A^T A=A A^T=I$
* Hermitian if $A^*=A$
* normal if $A^* A=A A^*$
* unitary if $A^* A=A A^*=I$

**Partition:** 
$$A=\left(\begin{array}{ll}
B & C \\
D & E
\end{array}\right)
$$
and if the shapes match, we have: 
$$
\left(\begin{array}{cc}
A & B \\
0 & C
\end{array}\right)\left(\begin{array}{cc}
X & Y \\
U & V
\end{array}\right)=\left(\begin{array}{cc}
A X+B U & A Y+B V \\
C U & C V
\end{array}\right)
$$

**Elementary row(column) operations** are:

* Interchange two rows(columns).
* Multiply a row(column) by a nonzero constant.
* Add a multiple of a row(column) to another row(column).

The elementary matrices are those can be obtained from $I_n$ by one elementary row (column) operation. The elementary operations on the other matrix could be represented by multiplying the elementary matrices. Let $A$ be an $m\times n$ matrix, let $E$ be $m\times m$ elementary matrix and $F$ be $n\times n$ matrix, then $EAF$ denotes the transformation of both row operation of $E$ and the column operation of $F$ on $A$. Then we have the following conclusions about the rank decomposition.

* Let $A$ be an $m \times n$ matrix over a field $\mathbb{F}$. Then there exist an $m \times m$ matrix $P$ and an $n \times n$ matrix $Q$, both of which are products of elementary matrices with entries from $\mathbb{F}$, such that 
  $$P A Q=\left(\begin{array}{cc}
  I_r & 0 \\
  0 & 0
  \end{array}\right)
  $$
  we have $r=\text{rank}(A)$.
* Let $A$ be an $m \times n$ (real or complex) matrix of rank $r$. Let $\operatorname{Ker} A$ be the **null space** of $A$, i.e., $\operatorname{Ker} A=\{x: A x=0\}$. Then $\operatorname{dim} \operatorname{Ker} A=n-r$.

The **determinant** of matrix $A$, $\det A$ or $\mid A\mid$, is a number associated with $A$. Here list two ways of defining the determinant:

* Let $p$ be a permutation on $\{1,2,...,n\}$, we say $p$ is even if $\{1,2,...,n\}$ could be restored from $p$ in even number of interchanges, odd otherwise. Define $\text{sign}(p)=1$ if $p$ is even and $\text{sign}(p)=-1$ if $p$ is odd. Then we have $\operatorname{det} A=\sum_{p \in S_n} \operatorname{sign}(p) \prod_{t=1}^n a_{t p(t)}$.
* We can also define the determinant by the Laplace formula as $\operatorname{det} A=\sum_{j=1}^n(-1)^{1+j} a_{1 j} \operatorname{det} A(1 \mid j)$, where $A(1 \mid j)$ is a submatrix of $A$ deleting the $1$-st row and the $j$-th column.

The determinant has the following properties:

* The determinant changes sign if two rows(columns) are interchanged.
* The determinant is unchanged if a constant multiple of one row(column) is added to another row(column).
* The determinant is a linear function of any row(column) when all the other rows(columns) are held fixed.
* For $A,B\in\mathbb{M}_n$, we have $\det AB=\det A\det B$.
* For $A\in\mathbb{M}_n$ and $C\in\mathbb{M}_m$, we have 
  $$\left|\begin{array}{ll}
  A & B \\
  0 & C
  \end{array}\right|=\operatorname{det} A \operatorname{det} C
  $$

A squared matrix is said to be **invertible**, or **nonsingular** if there exists a matrix $B$ such that $AB=BA=I$, then $B$ is called the inverse of $A$, $B=A^{-1}$. Define the **adjoint** of $A$ as $\text{adj}A=((-1)^{j+i}\det A(j\mid i))$, we could also have $A^{-1}=\frac{1}{\operatorname{det} A} \operatorname{adj}(A)$. We could conclude that if $E_1E_2...E_k A=I$, then $A^{-1}=E_1E_2...E_k$, which means we can apply row operations on $(A,I)$, then we obtain $A^{-1}$ from $(I, A^{-1})$.

Two square matrices $A$ and $B$ are said to be **similar** if for some invertible matrix $P$, we have $P^{-1}AP=B$.

The following statements are equal:

* $A$ is invertible, i.e., $A B=B A=I$ for some $B \in \mathbb{M}_n$.
* $A B=I$ (or $B A=I$ ) for some $B \in \mathbb{M}_n$.
* $A$ is of rank $n$.
* A is a product of elementary matrices.
* $A x=0$ has only the trivial solution $x=0$.$A x=b$ has a unique solution for each $b \in \mathbb{C}^n$.
* $\operatorname{det} A \neq 0$.
* The column vectors of $A$ are linearly independent.
* The row vectors of $A$ are linearly independent.

Let $V$ and $W$ be vector spaces over a field $\mathbb{F}$. A map $\mathcal{A}: V \mapsto W$ is called a **linear transformation** from $V$ to $W$ if for all $u, v \in V, c \in \mathbb{F}$, $\mathcal{A}(u+v)=\mathcal{A}(u)+\mathcal{A}(v)$, and $\mathcal{A}(c v)=c \mathcal{A}(v)$.

Let $\mathcal{A}$ be a linear transformation from $V$ to $W$. The subset in $W$, $\operatorname{Im}(\mathcal{A})=\lbrace\mathcal{A}(v): v \in V\rbrace$ is a subspace of $W$, called the image of $\mathcal{A}$, and the subset in $V$. $\operatorname{Ker}(\mathcal{A})=\{v \in V: \mathcal{A}(v)=0 \in W\}$ is a subspace of $V$, called the kernel or null space of $\mathcal{A}$. If $V$ is of dimension $n$, then $\operatorname{dim} \operatorname{Im}(\mathcal{A})+\operatorname{dim} \operatorname{Ker}(\mathcal{A})=n$.

A matrix of shape $m\times n$ is always a linear transformation from $\mathbb{F}^n$ to $\mathbb{F}^m$. A linear transformation can also be implemented by a matrix: $\mathcal{A}(x)=Ax$. Under different bases the matrix $A$ may have different forms, but they are similar.

**"Given a linear transformation on a vector space, it is a central theme of linear algebra to find a basis of the vector space so that the matrix of a linear transformation is as simple as possible, in the sense that the matrix contains more zeros or has a particular structure. In the words of matrices, the given matrix is reduced to a canonical form via similarity"**

-- Matrix theory- Basic Results and Techniques

Let $\mathcal{A}$ be a linear transformation on a vector space $V$ over $\mathbb{C}$. A nonzero vector $v \in V$ is called an **eigenvector** of $\mathcal{A}$ belonging to an **eigenvalue** $\lambda \in \mathbb{C}$ if $\mathcal{A}(v)=\lambda v, \quad v \neq 0$. The eigen vectors belonging to different eigen values are linearly independent.

If $A$ has $n$ linearly independent eigenvectors belonging to eigen values $\lambda_1, \lambda_2,...\lambda_n$, then $A$, under the basis formed by the corresponding eigenvectors, has a diagonal matrix representation:
$$
\left(\begin{array}{cccc}
\lambda_1 & & & 0 \\
& \lambda_2 & & \\
& & \ddots & \\
0 & & & \lambda_n
\end{array}\right)
$$
In other words, the eigenvalues of $A$ are those $\lambda\in \mathbb{F}$ such that $\det (\lambda I-A)=0$. The eigenvectors then are the vectors who serve as the bases of converting $\mathcal{A}$ to $A$. Furthermore, suppose $A$ is $n\times n$ complex matrix, the polynomial in $\lambda$: $p_A(\lambda)=\det(\lambda I_n-A)$ is called the characteristic polynomial of $A$, the zeros of the polynomial are the eigenvalues of $A$. It follows that every $n$-square matrix has $n$ (possibly repeatedly) eigenvalues.

The **trace** of an $n$-square matrix $A$ is defined as $\text{tr}(A)=\sum_{i=1}^{n}\lambda_i$.

It follows that:

* $\text{tr}A=\sum_{i=1}^n a_{ii}$
* $\det A = \prod_{i=1}^n \lambda_i$












### Curves, Scalar Fields, and Gradients

The overall goal is to generalize the analysis on the function $f:\mathbb{R}\rightarrow\mathbb{R}$ to $f:\mathbb{R}^m\rightarrow\mathbb{R}^n$. Some useful concepts are:

**(Derivative of the univariate scalar-valued function)** A function is called *differentiable* at a point $a$ of its domain, if its domain contains an open interval containing $a$, and the limit:
$$
\lim_{h\rightarrow 0}\frac{f(a+h)-f(a)}{h}
$$
exists. The $\epsilon-\delta$ definition of the derivative of $f(x)$ w.r.t its input $x$ states, $\forall \epsilon\in\mathbb{R}^+$,  there exists $\delta\in\mathbb{R}^+$, such that, $\forall |h|<\delta$ and $f(a+h)$ is defined, and
$$
|L-\frac{f(a+h)-f(a)}{h}|<\epsilon
$$

**(Curve)** The curves take the form $f:\mathbb{R}\rightarrow\mathbb{R}^n$. We could use $\mathbf{x}(t)$ to denote the curve with $\mathbf{x}\in \mathbf{R}^n$ and $t\in\mathbb{R}$. The curve is said to be *differentiable* if as $\Delta t\rightarrow 0$, we have $\mathbf{x}(t+\Delta t)-\mathbf{x}(t)=\mathbf{x}'(t)\Delta t+\mathcal{O}(\Delta t^2)$. This serves as the definition of the derivative $\mathbf{x}'(t)$:

$$
\frac{d\mathbf{x}}{dt}=\mathbf{x}'(t)=\lim_{\Delta t\rightarrow 0}\frac{\Delta\mathbf{x}}{\Delta t}
$$

it also follows that $\frac{d}{dt}(\mathbf{g}\mathbf{h})=\frac{d\mathbf{g}}{dt}\mathbf{h}+\frac{d\mathbf{h}}{dt}\mathbf{g}$. The derivative is also called *tangent vector* sometimes. Note that the tangent vector depends on the selection of parameter $t$, which prevents us to obtain an invariant measure of the curve. We could instead study the arc length:

$$
s=\int_{t_0}^t dt'|x'(t')|
$$

moreover, we have $\Delta s=|\Delta x|+\mathcal{O}(|\Delta x|^2)$ (as an aside, $|\cdot|$ here means the length or norm of a vector), we then have $\frac{ds}{dt}=sign(|x'|)$, where $sign(x)$ means that we need to set $x$ to be positive or negative according to whether the direction increase $t$ or not. With $s$ as our invariant parameterisation, we are able to define the induced tangent vector as $\frac{dx}{ds}$, easy to verify that the norm of this tangent vector is always 1. Similarly, the 'invariance' induced by the arc-length could be applied into the curvature of the curve (the second order derivative) as $\kappa(x)=\frac{d^2 x}{ds^2}$. Now curves map a scalar parameter to a vector value $x\in\mathbb{R}^n$, the scalar field does the contrary: $\phi:\mathbb{R}^n\rightarrow \mathbb{R}$. Generally we can define the vector field as $\phi:\mathbb{R}^n\rightarrow \mathbb{R}^n$. Consider the scalar field $\phi$, we could access the derivatives by its gradient: 
$$
\nabla \phi=\frac{\partial\phi}{\partial x^i}\mathbf{e}^i
$$
Basicly we just take derivative of $\phi$ w.r.t every coordinate, and constitute a new vector. 