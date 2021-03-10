---
layout: post
title:  Feasible link and path flow set
subtitle: Convexity and underdetermination 
tags: [research, linear algebra]
usemathjax: true
---



This blog reviews some basic concepts  in the traffic assignment. The focus will be on the network structure. 

A person wishing to travel must have a certain pair of origin and destination (OD). Conventionally, we will divide the entire city into different traffic zones, which can be regular or regular, dependent on the function of that zone. If we have $\vert Z\vert$ zones, then the total number of OD pairs can be as large as $\vert Z\vert \cdot \vert Z\vert$.

### Feasible path and link flow sets

Let's denote each OD pair as $w$ and the entire OD pair set as $W$. The demand of each OD pair is $d_w$.

For each OD pair $w$, there might be numerous paths to be selected to finish the trip. Let's say the path set of OD pair $w$ is $R_w$. We treat the number of travelers on the path as continuous variables and call it path flow. Let's use $x_{rw}$ to represent the $w$-th OD pair's $r$-th path, where $r\in R_w$. Then, we can define the feasible path flow set as:


$$
\Omega_p:= \{\mathbf{x}\mid \sum_{r\in R_w}x_{rw} = d_w, x_{rw}\geq 0, \forall w\in W \}
$$


The above constraint is very intuitive: all the travel demand is distributed into different paths of that OD pair and nobody disappears. It can be rewritten in vector form as:


$$
\Omega_p:= \{\mathbf{x}\mid \Lambda \mathbf{x}=\mathbf{d}, \mathbf{x}\geq 0 \}
$$

$$
\Lambda =

\begin{bmatrix}

\lambda_{r=1,w=1}, \ldots, \lambda_{r=\vert R_1\vert,w=1}, &0, &0\\

0, &\lambda_{r=1,w=2}\ldots \lambda_{r=\vert R_2\vert,w=2}, &0\\

\vdots & \vdots, & \vdots \\

0, &0, & \lambda_{r=1,w=\vert W \vert \ldots, \lambda_{r=R_{\vert W\vert}},w=\vert W\vert}

\end{bmatrix}

, \mathbf{x}=
\begin{bmatrix}
x_{r=1,w=1} \\
\vdots \\
x_{r=\vert R_1\vert,w=1} \\

\vdots \\

x_{r=1,w=\vert W \vert} \\
\vdots \\
x_{r=R_{\vert W\vert},w=\vert W\vert} \\
\end{bmatrix}
$$


The feasible link flow set is:


$$
\Omega_{l} := \{ 
\mathbf{v}\mid v_a=\sum_{w\in W}\sum_{r\in R_w}{\delta_{a}^{rw}x_{rw}, \mathbf{x}\in \Omega_{p}, \forall a\in A}
\}
$$


with its vector form being:
$$
\Omega_l := \{\mathbf{v}\mid \mathbf{v} = \Delta\mathbf{x}, \Lambda \mathbf{x}=\mathbf{d}, \mathbf{x}\geq 0 \}
$$

$$
\Delta = 
\begin{bmatrix}

\delta_{a=1}^{r=1,w=1}, \ldots, \delta_{a=1}^{r=\vert R_1\vert,w=1}, &0, &0\\

0, &\delta_{a=2}^{r=1,w=2}, \ldots, \delta_{a=2}^{r=\vert R_2\vert,w=2}, &0\\

\vdots & \vdots, & \vdots \\

0, &0, & \delta_{a=\vert A \vert}^{r=1,w=\vert W\vert }, \ldots, \delta_{a=\vert A \vert}^{r=\vert R_{\vert W\vert} \vert,w=\vert W \vert}

\end{bmatrix}
$$


$\Omega_l$ is like the "column space" of $\Delta$ spanned by the coefficients $\mathbf{x}$. 

#### Convexity

The path flow set is convex. The proof is very simple. For any two elements in $\Omega_p$, the convex combination of the two points is:


$$
\mathbf{x} = \beta\mathbf{x_1} + (1-\beta)\mathbf{x_2}, \forall \mathbf{x_1},\mathbf{x_2}\in \Omega_{p}\\
$$


Multiplying both sides by the OD-path incidence matrix:


$$
\begin{eqnarray}
 \Lambda\mathbf{x}  &=& \beta\Lambda\mathbf{x_1} + (1-\beta)\Lambda\mathbf{x_2} \nonumber \\
 &=& \beta \mathbf{d} + (1-\beta)\mathbf{d} \nonumber \\
 
 &=& \mathbf{d}, \forall \mathbf{x_1},\mathbf{x_2}\in \Omega_{p}
 
\end{eqnarray}
$$


For the link flow set, it is equivalent to applying the linear operator (i.e., $\Delta $) to the path flow set, which has been proved to be convex. So the link flow set is also convex. If we write the above reasoning into formulas, the convex combination of any two points in $\Omega_l$ is:


$$
\begin{eqnarray} 
\mathbf{v} &=& \beta \mathbf{v_1} + (1-\beta)\mathbf{v_2}, \forall \mathbf{v_1},\mathbf{v_2}\in \Omega_l \nonumber \\

&=& \Delta\beta\mathbf{x_1} + \Delta(1-\beta)\mathbf{x_2}, \forall \mathbf{x_1},\mathbf{x_2}\in \Omega_p \nonumber \\

&=& \Delta\cdot\mathbf{x}, \mathbf{x}\in \Omega_p

\end{eqnarray}
$$

### Underdetermination

For $\mathbf{v} = \Delta\mathbf{x}$ , given a $\mathbf{x}$, we can always get a unique $\mathbf{v}$. However, the reverse may not hold, because the  $\Delta^{-1}$ does not exist for non-square matrix. In reality, the column number (i.e., path No.) is usually much larger than the row number (i.e., link No.). 



### Traffic equilibrium

It is well-known that when link cost function is symmetric, finding the user-equilibrium flow pattern is equivalent to solving the following optimization problem:
$$
\min z(\mathbf{v}) = \sum_{a\in A} \int_{0}^{v_a}t_a(w)dw \\
\mathbf{v}\in\Omega_l \\
\Omega_{l} := \{ 
\mathbf{v}\mid v_a=\sum_{w\in W}\sum_{r\in R_w}{\delta_{a}^{rw}x_{rw}, \mathbf{x}\in \Omega_{p}, \forall a\in A}
\} \\
\Omega_p:= \{\mathbf{x}\mid \sum_{r\in R_w}x_{rw} = d_w, x_{rw}\geq 0, \forall w\in W \}
$$
This program is convex with respect to link flow variable: the constraint is convex set, as we have already shown; the objective function is the sum of strictly convex functions (an integral of this form is always strictly convex because the second-order derivative is always greater than 0).  

However, the objective function is not convex in path flow variable. To demonstrate, let's write its Hessian matrix. First, let's write the first-order derivative w.r.t. to one particular path $x_{rw}$:
$$
\frac{\partial z(\mathbf{v})}{\partial x_{rw}} = \frac{\partial z(\mathbf{v})}{\partial \mathbf{v}} \frac{\partial \mathbf{v}}{\partial x_{rw}} = \sum_a{t_a(v_a)\cdot \delta_a^{rw}} = c_{rw}
$$
In vector form, path cost, $c_{rw}$, can be arranged as:
$$
\mathbf{c} = \Delta^T \mathbf{t}(\Delta\mathbf{x})
$$
each element of which is:
$$
c_{rw}= \sum_{a\in A} \delta^{rw}_{a}\cdot t_a(\sum_{w\in W}\sum_{s\in R_w}{\delta^{sw}_{a}x_{sw}})
$$
Thus, the Hessian matrix of path flow of the first OD pair is:
$$
\begin{bmatrix}

\sum_a{\delta^{r=1,w=1}_a}{t_a^\prime(v_a)}\cdot \delta^{r=1,w=1}_{a} &\sum_a{\delta^{r=1,w=1}_a}{t_a^\prime(v_a)}\cdot \delta^{r=2,w=1}_{a} &\sum_a{\delta^{r=1,w=1}_a}{t_a^\prime(v_a)}\cdot \delta^{r=3,w=1}_{a} &\ldots  \nonumber \\

\sum_a{\delta^{r=2,w=1}_a t_a^\prime(v_a)}\delta_a^{r=1,w=1} &\sum_a{\delta^{r=2,w=1}_a t_a^\prime(v_a)}\cdot \delta^{r=2,w=1}_{a} &\sum_a{\delta^{r=2,w=1}_a t_a^\prime(v_a)}\cdot \delta^{r=3,w=1}_{a} &\ldots  \nonumber \\

\ldots &\ldots &\ddots \nonumber \\

\sum_a{\delta^{r=\lvert R_w\rvert,w=1}_a t_a^\prime(v_a)}\delta_a^{r=1,w=1} & & &\sum_a{\delta^{r=\lvert R_w\rvert ,w=1}_a t_a^\prime(v_a)\delta_a^{r=\lvert R_w \rvert, w=1}}  \nonumber \\


\end{bmatrix}
$$

The above matrix is symmetric. 

In vector form, 
$$
\nabla_{\mathbf{x}}{\mathbf{c}} = \Delta^T \begin{bmatrix}
\frac{\partial t_1}{v_1} & \ldots & 0\\
\vdots & \ddots & 0 \\
0 &0 &\frac{\partial t_m}{v_m}
\end{bmatrix} \Delta
$$
where $m$ is the link No.

### Some thoughts

For the link flow set $\Omega_l$, you may wonder whether we can apply the linear operator $\Delta^T$ on all the "coefficients" $\mathbf{v}$ to induce the path set $\Omega_p$, like this?



$$
\Omega_l := \{\mathbf{v}\mid \Delta^T\mathbf{v} = \mathbf{x}, \mathbf{x}\in \Omega_p \}
$$



The answer is no. The reasoning is as follows. In this case, $\Omega_l$ can be viewed as the solution set of linear equations $\Delta^T\mathbf{v} = \mathbf{x}$. Note that $\Delta^T$ is in most case not full-rank, which means even if the set of $\mathbf{v}$ is the entire $R^{\vert A \vert}$, the column space of $\Delta^{T}$ might not contain the whole $\Omega_p$. That is, for a $\mathbf{x}\in \Omega_p$, there might be no corresponding $\mathbf{v}$; i.e., no solution. Formally, we can multiply $\Delta$ at both sides and rewrite the linear equations:


$$
\Delta \Delta^T \mathbf{v} = \Delta \mathbf{x}
$$


Only when $\Delta$ is full-row-rank does  $\Delta \Delta^T$ become invertible and solution exists. Generally it is not the case. Logically, it is also meaningful! For a particular $\mathbf{v}$,  $\mathbf{x} = \Delta^T \mathbf{v}$ is trying to represent the path flow as the combination of link flow, which is logically wrong!  How can we add up the flows of all the links that a path contains?

Moreover, given a particular $\mathbf{v}\in \Omega_{l}$, there might be numerous $\mathbf{x}$ associated with the given $\mathbf{v}$, because $\Omega_l$ is the total collection that generates $\Omega_p$.

### Solution number check

The below shows an example network in which the single OD pair has 4 paths and the network contains totally 10 links. Let's arbitrarily choose the path flow as $[1,2,3,4]^T$. By checking the rank of $\Delta^T$ and augmented $[\Delta^T;x]$, we can easily verify that the $\mathbf{v}$ has infinitely many solutions. 

$$
\Delta^T = 
\begin{bmatrix}

1, &0, &0 &1 &0, &0, &0 &0 &0 &0  \nonumber \\

0, &0, &0 &0 &0, &1, &1 &0 &0 &0  \nonumber \\

0, &0, &1 &1 &0, &1, &0 &0 &0 &0  \nonumber \\

0, &0, &0 &0 &0, &0, &0 &0 &1 &0  \nonumber \\

\end{bmatrix}
$$


- $\Delta^T \mathbf{v}=\mathbf{x}$ has infinitely many solutions iff $r(\Delta^T) = rank(\Delta^T\mid \mathbf{x}) < n$, where $n$ is the number of columns (i.e., unknown's).
- $\Delta^T \mathbf{v}=\mathbf{x}$ has a unique solutions iff $r(\Delta^T) = rank(\Delta^T\mid \mathbf{x}) = n$, where $n$ is the number of columns (i.e., unknown's).
- $\Delta^T \mathbf{v}=\mathbf{x}$ has no solution iff $r(\Delta^T) < rank(\Delta^T\mid \mathbf{x})$, where $n$ is the number of columns (i.e., unknown's).