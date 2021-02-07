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

### Some thoughts

For the link flow set $\Omega_l$, I wonder whether we can use the transpose of $\Delta$, just like this?


$$
\Omega_l := \{\mathbf{v}\mid \Delta^T\mathbf{v} = \mathbf{x}, \Lambda \mathbf{x}=\mathbf{d}, \mathbf{x}\geq 0 \}
$$


The answer is no, because $\mathbf{x} = \Delta^T \mathbf{v}$ is trying to represent the path flow as the combination of link flow, which is logically wrong! How can we add up the flows of all the links that a path contains?

In addition, given a fixed $\mathbf{x}$, solving the $\Delta^T \mathbf{v}=\mathbf{x}$  generally does not yield a unique solution. The below shows an example network in which the single OD pair has 4 paths and the network contains totally 10 links. Let's arbitrarily choose the path flow as $[1,2,3,4]^T$. By checking the rank of $\Delta^T$ and augmented $[\Delta^T;x]$, we can easily verify that the $\mathbf{v}$ has infinitely many solutions. 


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