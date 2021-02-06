---
layout: post
title:  Value iteration process converges to an optimal policy
subtitle: Why?
tags: [reinforcement learning]
usemathjax: true
---

In the last policy iteration blog, we prove that starting from an initial pocliy, the iteration process of "evaluation -> greedy improvement -> evaluation -> greedy improvement ..." can guarantee an optimal policy. We note that at each step of evaluation, we have to iterate it many times to get a trustable state value estimate. A natural question is then: can we just do the greedy improvement before we get the true state value estimate of the policy? The answer is yes and it is in fact the value iteration algorithm. 

Recall that given a policy, the evaluation process is:


$$
v^{k+1}_{\pi}(s) = \sum_{a\in A}\pi(a|s)\left( R^a_s + \gamma \sum_{s'\in S}{P_{ss'}^a v^k_{\pi}(s')} \right)
$$


The value iteration process is that we only take the above backup once and get the q value function. Then we directly do the greedy improvement without averaging all the q values by $\pi(a\mid s)$. This turns out to be the bellman optimality update:


$$
v^{k+1}(s) = \max_{a}\left\{ R^a_s + \gamma \sum_{s'\in S}{P_{ss'}^a v^k_{\pi}(s')} \right\}
$$


We can iteratively apply the above formula and finally we can get the optimal value function and the corresponding optimal policy. (But before the algorithm converges, there is no explicit policy). 

> **Theorem**: For any finite MDP, there exists an optimal policy $\pi^\star$ such that it is better than or equal to every other possible policy $\pi$.

But wait... You may ask that what do you mean by "better than"? Formally, we define it ($\geq$) as:


$$
\pi^1 \geq \pi^2 \;\text{, if}\; V^{\pi^1}(s)\geq V^{\pi^2}(s), \forall s\in S
$$


That is, for every state in the state space, the state value should be greater than the other one. 

### Banach fixed-point theorem

The proof is based on the **Banach fixed-point theorem** (a.k.a contraction mapping theorem), which works in the complete metric space. (The name "Banach" appears in this theorem, because the complete metric space is also called Banach space.) The theorem basically says that for a complete metric space, applying a contraction operator on the elements of the set again and again would eventually leads us to a fixed point. In the value iteration, it echoes the optimal policy. 

> **Banach fixed-point theorem**: Let $(X,d)$ be a non-empty complete metric space, with a contraction mapping $T:X\to X$. Then $T$ admits a unique fixed point $x^{\star}$ in $X$ (i.e. $T(x^{\star}) = x^{\star}$). The fixed point is the limiting point of a sequence of $f(f(f(\ldots f(x))))$ until convergence.

There are a lot of terminologies to be digested, which are introduced in the appendix. All we need to prove is just that the bellman optimality operator is a contraction. 

#### Contraction

A operator $ f(\cdot)$defined on the elements of the metric space $(X,d)$ is a contraction if:


$$
\exists \gamma\in [0,1), d(f(x_1), f(x_2)) \leq \gamma d(x_1,x_2), \forall x_1,x_2 \in X
$$


It means that after applying the operator $f(\cdot)$, the two points (elements) of $X$ get closer to each other. The "closer" is measured by metric $d$. Note that $\gamma$ should not be 1, otherwise it is only a **nonexpansive** operator; e.g., the projection operator onto a closed convex set. 

In the following, let's choose $\infty$-norm (or max-norm) as our distance metric, i.e., 


$$
d(x_1,x_2) = \lVert x_1-x_2 \rVert_{\infty} = \max_{i\in [1,n]} \lvert x_1^i-x_2^i \rvert
$$


$X$ can also consist of functions instead of vectors, 


$$
d(u,v) = \lVert u-v \rVert_{\infty} = \max_{s\in S} \lvert u(s) - v(s) \rvert
$$


where $u$ and $v$ are two functions. And this is exactly we want to use because our aim is to measure how close any two state value functions are (i.e., $v$ or $q$ value functions). 

###  Bellman optimality operator

Denoting the bellman optimality operator (Eq. 2) as $O$, 


$$
\begin{eqnarray} 
\lVert Ov_1(s) - Ov_2(s) \rVert_{\infty} &=&  \max_{s} \left\vert Ov_1(s) - Ov_2(s) \right\vert \nonumber \\

&=& \max_{s}\left\vert \max_a{\{ R^a_s+\gamma\sum_{s'\in S}\mathcal{P}^a_{ss'}v_1(s') \}} - \max_a{\{ R^a_s+\gamma\sum_{s'\in S}\mathcal{P}^a_{ss'}v_2(s') \}}\right\vert   \nonumber \\

&\leq& \max_{s}\left\vert \max_a{\{ R^a_s+\gamma\mathcal{P}^a_{ss'}v_1(s') - R^a_s-\gamma\mathcal{P}^a_{ss'}v_2(s') \}}\right\vert   \nonumber \\

&=& \gamma\max_{s}\left\vert \max_a{\{ \sum_{s'\in S}\mathcal{P}^a_{ss'}(v_1(s')-v_2(s')) \}}\right\vert   \nonumber \\

&\leq& \gamma\max_{s}\left\vert \max_a{\{ \max_{s'}\lvert v_1(s')-v_2(s') \rvert\}}\right\vert   \nonumber \\

&=& \gamma \max_{s'}\lvert v_1(s')-v_2(s') \rvert  \nonumber \\

&=& \gamma\lVert (v_1(s')-v_2(s')) \rVert_\infty   \nonumber \\

\end{eqnarray}
$$


The first and second steps are applying the definition of max-norm and Bellman optimality operator, respectively.

In the third step, the two max operations over $a$ will obtain different $a$, but we want to combine them. To this end, we assume that the first maximization achieves its maximum at $a$. Then by replacing the second maximization's best action, we must have reduced its value (or kept the same if the two maximizations coincidently induce the same action).

In the fifth step, since the transition dynamic operation $\mathcal{P}_{ss'}^a$ is a convex combination of its corresponding values, it mush be less than the maximum over all the summands' absolute values. This maximization is exactly the definition of max-norm over all the states. 

In the sixth step, since the summands no longer contain $a$ and $s$, so the two maximization can be removed.

### Bellman expectation operator

In addition ,we can also prove that the bellman expectation backup, denoted $B$, is a contraction operator. (This also explains why the policy evaluation process in the policy iteration algorithm does converge to a true state value function.)


$$
\begin{eqnarray} 
\lVert Bv_1(s) - Bv_2(s) \rVert_{\infty} &=&  \max_{s} \lvert Bv_1(s) - Bv_2(s) \rvert \nonumber \\

&=& \max_{s}\left\vert \sum_a{\pi(a\mid s)}{\left(R^a_s+\gamma\sum_{s'\in S}\mathcal{P}^a_{ss'}v_1(s')\right)} - \sum_a{\pi(a\mid s)}{\left(R^a_s+\gamma\sum_{s'\in S}\mathcal{P}^a_{ss'}v_2(s')\right)} \right\vert  \nonumber \\

&=& \gamma\max_{s}\left\vert \sum_a{\pi(a\mid s)}\sum_{s'\in S}\mathcal{P}^a_{ss'}{\left(v_1(s') - v_2(s') \right)} \right\vert \nonumber \\

&\leq& \gamma\max_{s}\left\vert \max_{s'}{\lvert v_1(s') - v_2(s')\rvert} \right\vert \nonumber \\

&=&  \gamma\max_{s'}\left\vert {v_1(s') - v_2(s')} \right\vert \nonumber \\

&=& \gamma\lVert (v_1(s')-v_2(s')) \rVert_\infty   \nonumber \\

\end{eqnarray}
$$


This proof is similar to the $O$ operator and in fact simpler. 

In the fourth step, given any $s$, instead of averaging the environment dynamic $\mathcal{P}_{ss'}^a$ and policy $\pi$, we can  just simply pick the $s'$ that maximize the $\lvert v_1(s')-v_2(s') \rvert$. (This inequality scaling seems to be very loose.)

In the fifth step, again, the summands are not related to $s$. 

#### Q function

The proof with respect to $q$ function is similar to the case of $v$. 

---



### Appendix

#### Cauchy sequence

For a metric space $(X,d)$, suppose there is a sequency of elements in $X$, $x_1,x_2,\ldots,x_n$. The sequence is called Cauchy sequence if:
$$
\forall \epsilon, \exists i,j> N, \ni d(x_i,x_j)\leq \epsilon
$$
The intuition is that no matter how small the distance $\epsilon$ is, the sequence can always find indexes $i,j$ past which the two terms are within the given distance. It formally defines what is "two elements become close". 

#### Complete metric space

A metric space is said to be complete if every possible cauchy sequence of the elements in the set $X$ converges to an element within $X$. Complete basically means that when we take the limit, the obtained element will not go out of the set. 

#### Proof of contraction mapping theorem

- Existence:

  All we need to show is that a sequence induced by the contraction operator is a Cauchy sequence, since the latter will converge to a single element in $X$. Let's denote the contraction operator as $f(\cdot)$, and the corresponding sequence is:

  
  $$
  x^k = f^k(x), k=1,2,\ldots
  $$
  

  Taking two elements $x^m,x^n, m>>n$ , we iteratively apply the triangular inequality property of the metric space:

  
  $$
  \begin{eqnarray}
  d(x^m,x^n) &\leq& d(x^{m},x^{m-1}) + d(x^{m-1}, x^n) \nonumber \\
  
  &\leq& d(x^m,x^{m-1}) + d(x^{m-1}, x^{m-2}) + d(x^{m-2}, x^{n}) \nonumber \\
  
  &\leq& d(x^m,x^{m-1}) + d(x^{m-1}, x^{m-2}) + \ldots d(x^{n+1}, x^{n}) \nonumber \\
  
  &=& d(f^m(x), f^{m-1}(x)) + d(f^{m-1}(x), f^{m-2}(x)) +\ldots d(f^{n+1}(x), f^{n}(x)) \nonumber \\
  
  &\leq& \gamma^{m-1}d(f(x), x) + \gamma^{m-2}d(f(x), x) + \ldots \gamma^n d(f(x), x) \nonumber \\
  
  &\leq& \frac{\gamma^n (1-\gamma^{m-n})}{1-\gamma}d(f(x), x) = \frac{\gamma^n-\gamma^{m}}{1-\gamma}d(f(x), x)\nonumber \\
  
  \end{eqnarray}
  $$
  

  As long as we set $m$ sufficiently larger than $n$ , $\gamma^n$ dominates; and when $n\to\infty$, the RHS approaches 0. 

  
  $$
  \lim_{n\to\infty} \frac{\gamma^n}{1-\gamma}d(f(x), x) = 0
  $$
  

  So for any given $\epsilon$, we can always find a $n$ such that $d(x^m,x^n)\leq \epsilon$.

- Uniqueness:

  Proof by contradiction: Suppose now there are two fixed points $x_1^\star, x_2^\star$. Then we know the distance between them before and after applying the operator should be the same, i.e.,

  
  $$
  d(f(x_1^*), f(x_2^*)) = d(x_1^*,x_2^*)
  $$
  

  However, the definition of contraction operator tells us that:

  
  $$
  d(f(x_1^*), f(x_2^*)) \leq \gamma d(x_1^*,x_2^*), \gamma\in[0,1)
  $$
  

  , which results in the contradiction. 