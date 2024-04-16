---
layout: post
usehighlight: true
tags: [LLM, introduction, NLP]
title: Large Language Models - Aspects and Methods
---


This post is the note for reviewing the fundamental mathematics in machine learning, especially, deep learning area. We will briefly revisit the basic concepts and useful tools in the analysis of ML algorithms.

Here are some excellent materials you may find helpful:

1. [Algebra, Topology, Differential Calculus, and Optimization Theory For Computer Science and Machine Learning](https://www.cis.upenn.edu/~jean/math-deep.pdf)
2. [The matrix cookbook](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf)
  
In fact, most of the contents in this post derive from [1].

# Table of Contents
- [Table of Contents](#table-of-contents)
  - [Preliminaries](#preliminaries)
  - [Calculus](#calculus)
    - [Curves, Scalar Fields, and Gradients](#curves-scalar-fields-and-gradients)

## Preliminaries

**Note**: This post assumes you have the preliminary knowledge of simple calculus in univariate case and linear algebra. Here provides some concepts which help us see the things in an abstract way (meaning that we then know where can or cannot we generalize our analysis to).

(Derivative of the univariate scalar-valued function) A function is called *differentiable* at a point $a$ of its domain, if its domain contains an open interval containing $a$, and the limit:
$$
\lim_{h\rightarrow 0}\frac{f(a+h)-f(a)}{h}
$$
exists. The $\epsilon-\delta$ definition of the derivative of $f(x)$ w.r.t its input $x$ states, $\forall \epsilon\in\mathbb{R}^+$,  $\exist\delta\in\mathbb{R}^+$, such that, $\forall |h|<\delta$ and $f(a+h)$ is defined, and
$$
|L-\frac{f(a+h)-f(a)}{h}|<\epsilon
$$

## Calculus

This post assumes you have the preliminary knowledge of simple calculus in univariate case. The overall goal is to generalize the analysis on the function $f:\mathbb{R}\rightarrow\mathbb{R}$ to $f:\mathbb{R}^m\rightarrow\mathbb{R}^n$. Some useful concepts are:

### Curves, Scalar Fields, and Gradients

The curves take the form $f:\mathbb{R}\rightarrow\mathbb{R}^n$. We could use $\mathbf{x}(t)$ to denote the curve with $\mathbf{x}\in \mathbf{R}^n$ and $t\in\mathbb{R}$. The curve is said to be *differentiable* if as $\Delta t\rightarrow 0$, we have $\mathbf{x}(t+\Delta t)-\mathbf{x}(t)=\mathbf{x}'(t)\Delta t+\mathcal{O}(\Delta t^2)$. This serves as the definition of the derivative $\mathbf{x}'(t)$:

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