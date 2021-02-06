---
layout: post
title:  Improvimg greedily leads to optimality in policy iteration
subtitle: Why?
tags: [reinforcement learning]
usemathjax: true
---

在Dynamic programming里，policy iteration是一个经典的框架，通过反复迭代evaluation和improvement 来得到一个最优的策略。Evaluation得到的值函数，传给improvement过程，而improvement过程根据这个值函数做一个贪婪的policy，返回给evaluation过程。下面看一下为什么贪婪的反馈能够得到一个optimal policy。

在Silver的课上，或者在Sutton的书上，推导的过程里面那个$E_{\pi^{\prime}}$ 让有点让我犯晕…… 我就把它展开方便理解一点。

## Evaluation

这个过程就是通过bellman expectation equation来做的，先设一个价值函数$v_{\pi}(s), \forall s$，它cache了，在状态$s$，之后一直执行$\pi$ ，所能得到的expected total rewards。从随机初始化的一个值函数开始迭代，根据第$k$轮的值函数，通过one-step-look-ahead，算一个更新的$k+1$轮的值函数，以此往复：


$$
v^{k+1}_{\pi}(s) = \sum_{a\in A}\pi(a|s)\left( \sum_{r\in \R^1}r \sum_{s'\in S}P(s',r|s,a) + \gamma \sum_{s'\in S}{\sum_{r\in \R^1}P(s',r|s,a) v^k_{\pi}(s')} \right)
$$


上面这个式子里，我完整的展开了one-step-look-ahead的所有概率公式。这里transition operator, $ P(s',r\mid s,a)$, 完整地描述了随机切换到下一个state，同时给一个随机reward。我们认为离开当前state $s$的时候才得到reward，所以reward取决于$s,a$和环境，把marginalization去掉：


$$
v^{k+1}_{\pi}(s) = \sum_{a\in A}\pi(a|s)\left( \sum_{r\in \R^1}r P(r|s,a) + \gamma \sum_{s'\in S}{P(s'|s,a) v^k_{\pi}(s')} \right)
$$


上述的两个展开确实太臃肿了，于是一般大家都会简写成:


$$
v^{k+1}_{\pi}(s) = E_{\pi}[R_{t+1} + \gamma v_{\pi}^{k}(S_{t+1})|S_t=s]
$$


这里用了大写的$R$和$S$表示reward和下一个state的随机变量。这个公式可以一直不停的迭代，最后当$k$和$k+1$的值函数相等的时候，就得到了一个“真实的”关于$\pi$的价值函数。(也可以stop early直接做下面的improvement。)

## Improvement

当得到对于$\pi$的evaluation之后，我们就可以做improvement了。贪婪的做法就是:


$$
\pi^{\prime}(s) = {\arg\max}_{a\in A} = q_{\pi}(s,a)
$$


( 你可能会问，在Evaluation里面，我们做的是对值函数$v$的估计，但这里做决策是对动作值函数$q$ ？这个很好解决，因为DP假设的是知道整个model的，用一下Bellman expectation equation就行了：


$$
q_{\pi}(s,a) = \sum_{r\in \R^1}r P(r|s,a) + \gamma \sum_{s'\in S}{P(s'|s,a) v^k_{\pi}(s')} = R_s^a + \gamma \sum_{s'\in S}{P_{ss'}^{a} v^k_{\pi}(s')}
$$


这里用了$R_s^a$表示期望，是一个scalar，而$P_{ss'}^{a}$表示状态转移概率。)

接下来我们就来看一下为什么这样贪婪的做法能提高值函数：

贪婪的做法是在$s$，选最大化$q$的那个，所以执行那个action之后的动作函数是大于执行任一由policy指定的action，


$$
q(s,\pi^\prime(s)) = \max_{a\in A}q(s,a) \geq q(s,\pi(s)) = v_{\pi}(s)
$$

$$
\begin{eqnarray} 
v_\pi(s) &\leq& q(s,\pi^\prime(s))= R_s^{\pi^\prime(s)} + \gamma \sum_{s'\in S}{P_{ss'}^{\pi^\prime}}v_{\pi}(s') = E[R_{t+1}+\gamma v_{\pi}(S_{t+1})| S_t=s, A_t=\pi^\prime(s)] \nonumber \\

&\leq& E[R_{t+1} + \gamma q(S_{t+1}, \pi^\prime(S_{t+1})) | S_t=s, A_t=\pi^\prime(s)] \nonumber \\

&=& E\left[ R_{t+1} + \gamma E[R_{t+2} + \gamma v_{\pi}(S_{t+2}) | S_{t+1}, \pi^\prime(S_{t+1})] | S_t=s, A_t=\pi^\prime(s) \right] \nonumber \\

&=& E\left[ R_{t+1} + \gamma E[R_{t+2} + \gamma v_{\pi}(S_{t+2}) | S_{t+1}, \pi^\prime(S_{t+1}), S_t=s, A_t=\pi^\prime(s)] | S_t=s, A_t=\pi^\prime(s) \right] \nonumber \\ 

&=& E\left[ R_{t+1} + \gamma R_{t+2} + \gamma^2v_{\pi}(S_{t+2}) | S_t=s, A_t=\pi^\prime(s) \right] \nonumber \\

&\ldots& \nonumber \\

&\leq& E\left[ R_{t+1} + \gamma R_{t+2} + \gamma^2R_{t+3}, \gamma^3 v_{\pi}(S_{t+4}) + \ldots | S_t=s, A_t=\pi^\prime(s) \right] \nonumber \\

&=& v_{\pi^\prime}(s) \nonumber \\ 

\end{eqnarray}
$$


第三行到第四行的等号，是因为马尔科夫性质，这样可以构造一个[iterated expectation](2021-01-24-Simple-Intuition-on-Iterated-Expectation.md)的形式。QED

那这个improvement过程什么时候终止呢，就是


$$
\max_{a\in A}q(s,a) = q(s,\pi(s))
$$


这个时候，


$$
v_{\pi}(s) = \max_{a\in A}q(s,a) 
$$


就是Bellan optimality equation.