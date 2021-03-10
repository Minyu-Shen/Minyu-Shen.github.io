---
layout: post
title: Day-to-day dynamics and FW algorithm
subtitle: Connections & differences
tags: [research, day-to-day, mathematical analysis]
usemathjax: true
---

### Traffic equilibrium

It is well-known that when link cost functions are symmetric, finding the user-equilibrium flow pattern is equivalent to solving the following optimization problem:


$$
\min z(\mathbf{v}) = \sum_{a\in A} \int_{0}^{v_a}t_a(w)dw \\
\mathbf{v}\in\Omega_l \\
\Omega_{l} := \{ 
\mathbf{v}\mid v_a=\sum_{w\in W}\sum_{r\in R_w}{\delta_{a}^{rw}x_{rw}, \mathbf{x}\in \Omega_{p}, \forall a\in A}
\} \\
\Omega_p:= \{\mathbf{x}\mid \sum_{r\in R_w}x_{rw} = d_w, x_{rw}\geq 0, \forall w\in W \}
$$


### Monotone decreasing of potential function in day-to-day models

Many day-to-day models have the following structure:


$$
x^{n+1}-x^{n} = \alpha (y^n-x^n) \implies x^{n+1} = \alpha y^n + (1-\alpha)x^n
$$


The difference between the potential function $F$ of $x^{n+1}$ and $x^n$ for UE:


$$
F(x^{n+1})-F(x^n) = F(\alpha y^n + (1-\alpha)x^n) - F(x^n) = \int_{x^n}^{\alpha y^n + (1-\alpha)x^n}F'(w)dw
$$


Applying the change of variable:

​	Let $w=g(s)=x^n+s(y^n-x^n)$, we know $dw=(y^n-x^n)ds$.

​	Since $s = \frac{w-x^n}{y^n-x^n}$, when $w=x^n$, $s=0$; when $w=\alpha y^n + (1-\alpha)x^n$, $s=\alpha$. 

So:


$$
\int_{x^n}^{\alpha y^n + (1-\alpha)x^n}F'(w)dw = \int_{0}^{\alpha}\frac{d F(x^n+s(y^n-x^n))}{ds}ds = \int_{0}^{\alpha}(y^n-x^n)^T \nabla F(x^n+s(y^n-x^n))ds
$$


Next let's use some properties and assumptions to prove that the above equation is less than some negative quantities. In other words, the direction of $y^n-x^n$ (with $\alpha$ being the step size) will actually decrease the potential function.


$$
\begin{eqnarray}
 && \int_{0}^{\alpha}(y^n-x^n)^T \nabla F(x^n+s(y^n-x^n))ds = \int_{0}^{\alpha}(y^n-x^n)^T c(x^n+s(y^n-x^n))ds = \int_{0}^{\alpha}(y^n-x^n)^T c(x^n)ds \nonumber \\
&& + \int_{0}^{\alpha}(y^n-x^n)^T \left( c(x^n+s(y^n-x^n)) -c(x^n) \right) ds
\end{eqnarray}
$$


If $y^n$ is determined by the projection operator, then $(y^n-x^n)^T c(x^n) \leq -\frac{1}{\gamma}\lVert y^n-x^n \rVert^2 \leq 0$. Therefore,


$$
\int_{0}^{\alpha}(y^n-x^n)^T c(x^n)ds \leq -\frac{\alpha}{\gamma}\lVert y^n-x^n \rVert^2
$$

$$
\begin{eqnarray}
 && \int_{0}^{\alpha}(y^n-x^n)^T \left( c(x^n+s(y^n-x^n)) -c(x^n) \right) ds \leq \int_{0}^{\alpha}\lVert y^n-x^n\rVert \lVert c(x^n+s(y^n-x^n)) -c(x^n) \rVert ds \nonumber \\
&& \leq \int_{0}^{\alpha}\lVert y^n-x^n\rVert Ls\lVert y^n-x^n \rVert ds = \frac{\alpha^2 L}{2}\lVert y^n-x^n \rVert^2
\end{eqnarray}
$$


Finally, 


$$
F(x^{n+1})-F(x^n) \leq \left( \frac{\alpha^2 L}{2}-\frac{\alpha}{\gamma}\right )\lVert y^n-x^n \rVert^2
$$


We need:


$$
\left( \frac{\alpha^2 L}{2}-\frac{\alpha}{\gamma}\right ) < 0, \alpha\in(0,1),\implies \alpha < \frac{2}{L\gamma}
$$


We know that a very large $\gamma$ represents very small control cost in the projection operator, which is close to the case of all-or-nothing assignment. In such case, a very small $\alpha$ is required to ensure that the potential is still decreasing. But note that here we can only present a sufficient condition for the decreasing. The chosen $\alpha$ ensuring decreasing can be larger.

On the other hand, for the FW algorithm, at each iteration, it will solve a shortest path problem, which is equivalent to conducting the projection operator with an infinitely large $\gamma$. After get the all-or-nothing pattern, $y$, this algorithm will solve a constrained ($0\leq \alpha\leq 1 $) subproblem to get the best $\alpha$; i.e., the $\alpha$ is dynamically changing and uses the information at each iteration. Thus, even $\gamma$ is large, it can still decrease the potential without violating the constraint. 



### Multi-class UE

To be continued...



## Appendix

### Mean value theorem

Let's review the mean value theorem.

#### 1-D version

 For the 1-D version, let $f:[a,b]\to R$ is a continuous function on the closed interval $[a,b]$ and differentiable on the open interval (a,b). Then there exists a $c$ such that:
$$
f(b) - f(a) = f'(c)(b-a)
$$
There is also an integral version. 
$$
f(c) = \frac{1}{b-a}\int_a^b{f(t)}dt
$$
​	To prove, first note that $f$ is continuous on $[a,b]$, so it is also integrable:
$$
F(x) = \int_{a}^{x}f(t)dt, \forall x\in[a,b]
$$
​	Fundamental theorem of calculus says that $F(x)$ should be continuous on $[a,b]$, differentiable on $(a,b)$, and
$$
F'(x) = f(x)
$$
​	That's just the condition for applying the MVT of Eq. (1), which results in:
$$
F(b)-F(a) = \int_{a}^{b}f(t)dt + \int_{a}^{a}f(t)dt = \int_{a}^{b}f(t)dt = f(c)(b-a)
$$

#### n-D version

Using the 1-D version of MVT, we can get the n-D version, the vector-valued functions.

Let $G$ be a open convex set in $R^n$. For any two points, $x$ and $y$ in $G$, the convex combination with $\alpha$ is still in $G$. Treating $f((1-\alpha)x + \alpha y)$ as a function with single variable of $\alpha$ and applying the 1-D mean value theorem, we can get:
$$
f'((1-\alpha)x + \alpha y) = \nabla f((1-\alpha)x + \alpha y)^T(y-x)
$$
