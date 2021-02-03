---
layout: post
title:  A variational inequality framework for network games (Parise and Ozdaglar, 2019)
subtitle: Part 1
tags: [game theory, research]
usemathjax: true
---

这两天在Games and Economic Behavior上看到一篇个人感觉非常有趣的论文，名字是：A variational inequality framework for network games: Existence, uniqueness, convergence and sensitivity analysis。这篇文章试图用Finite Variational Inequality (VI)的框架去把之前文献中存在的一些关于network game的模型做一个统一。使用VI的主要作用是为了克服文献中使用potential game的框架所无法处理的asymmetric cost function。一些文献中已经研究的模型，可以看做是这篇文章在一定取值下的speical cases.

文章的在矩阵的表达上比较简洁，我会具体把矩阵的每一项来展开，方便更好理解，中间会穿插个人构造的小例子，来帮助理解模型的细节。大部分technical的证明部分，我还怎么看，就先略过了，之后看到精彩的证明部分，再补充。

## 模型

众所周知，在一个game中，玩家 i的收益是会由她本人及其他所有的玩家的策略所共同影响的。我们用 $x^i$ 来表示第 i 个玩家的策略向量，$x^i\in X^i\subseteq \mathbb{R}^n$[^1].  现在一共有$N$个玩家，我们用 $z^i(x)$ 来表示其他所有玩家策略的总和。这里$x$ 就是所有玩家的决策向量的”向量“，也就说是一个堆叠的$N\cdot n $维向量。 所有对于玩家 i 来说，cost (or payoff) function就长这样：

$$
J^i(x^i, z^i(x))
$$

可以用一个非负的adjacency matrix, $G$，来建模玩家之间的网络效应：

$$
z^i(x) = \sum_{k=1}^{N}G_{ik}x^k
$$
Here $G_{ij}$ is the effect of k on i; i.e., in the graph, the arrow direction is from k to i.

根据VI和纳什均衡的关系，我们可以写出：

$$
F(x^*)^T (x-x^*)\geq 0, \forall x \in X
$$
这里整个$X = X^1 \times X^2 \times \ldots \times X^N \subset R^{Nn}$. 而

$$
F(x) = 
\begin{bmatrix}F^1(x)\\...\\F^N(x) \end{bmatrix}
= \begin{bmatrix}\nabla_{x^1}J^1(x^1, z^1(x))\\...\\\nabla_{x^N}J^N(x^N, z^N(x)) \end{bmatrix}
$$

因为$J^i$是每个i的cost嘛，自然是一个scalar，而$x^i$是一个n-dim的向量，所以对于每一个i的策略的gradient整体是一个n-dim的向量。继续展开这个，可以写成：

$$
F(x) = 
\begin{bmatrix}
\frac{\partial J^1(x^1,z^1(x))}{x^1_1} \\
\ldots \\
\frac{\partial J^1(x^1,z^1(x))}{x^1_n} \\ 
\vdots \\
\frac{\partial J^N(x^N,z^N(x))}{x^N_1} \\
\ldots \\
\frac{\partial J^N(x^N,z^N(x))}{x^N_n}\\
\end{bmatrix} \in \mathbb{R}^{Nn}
$$

$\frac{\partial J^i(x^i,z^i(x))}{x^i_j}$ 表示的是第i个玩家，改变策略j，对于他自身cost function的影响。整个 $F(x)$ 又被叫作game jacobian，刻画了玩家的单方面策略改变，对于她自身cost的影响。

---

下面我们借助交通分配的例子来具象化一下

#### Traffic assignment example

Let's look at a simple traffic network with one OD pair. Suppose that there are two "UE players" controlling $d$ demands. One controls $\theta d$ while the other controls $(1-\theta)d$. There are five (n=5) links in the network. The link flow set for player i is denoted as $\Omega_i$.  

$x^i$就是UE玩家i 对于所有n=5个link上流量的分配向量。$G_{ij}=1, \forall j\neq i, G_{ii}=0, \forall i$。如果是对称的情况，那么经典的Beckmann函数就是potential function；对于UE玩家1来说：

$$
J^1(x^1, z^1(x)) = \sum_{j=1}^{n=5}\int_0^{x^1_j}{c_j(z^1(x)+w)dw} = \sum_{j=1}^{n=5}\int_0^{x^1_j}{c_j(x^2_j+w)dw}
$$

对link的梯度就是：

$$
F^1(x^1, z^1(x)) = \nabla_{x^1}J^1(x^1, z^1(x)) =
\begin{bmatrix}c_1(x_1^2+x_1^1)\\...\\c_5(x_5^2+x_5^1) \end{bmatrix}
$$

对于UE玩家2来说，是一样的。

---

## 性质

根据$F(x)$的不同性质，（比如strictly monotone, strongly monotone, 是否是uniform block P-function，是否是uniform P-function等）可以去确定VI解的存在性以及唯一性等问题 (参考VI最经典的那本书)，这也是这篇文章technical的部分。而要想证明$F(x)$的性质，可以进一步看一下$F(x)$的Jacobian, 也就是$J(x)$的Hessian矩阵：

$$
\nabla_x F(x) =
\begin{bmatrix}
\nabla^2_{x^1x^1}J^1(x^1, z^1(x)), \ldots, \nabla^2_{x^1x^k}J^1(x^1, z^1(x)), \ldots, \nabla^2_{x^1x^N}J^1(x^1, z^1(x)) \\

\vdots \\
\nabla^2_{x^ix^1}J^1(x^1, z^1(x)), \ldots, \nabla^2_{x^ix^k}J^1(x^i, z^i(x)), \ldots, \nabla^2_{x^ix^N}J^1(x^1, z^1(x)) \\

\vdots \\
\nabla^2_{x^Nx^1}J^1(x^1, z^1(x)), \ldots, \nabla^2_{x^Nx^k}J^1(x^N, z^N(x)), \ldots, \nabla^2_{x^Nx^N}J^1(x^1, z^1(x)) \\

\end{bmatrix}
$$

上述矩阵的对角线元素是继续对自身策略求导。

$$
\nabla^2_{x^ix^i}J^i(x^i, z^i(x)) =
\begin{bmatrix}
\frac{\partial^2 J^i(x^i,z^i(x))}{x^i_1 x^i_1}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_1 x^i_j}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_1 x^i_n} \\

\vdots \\
\frac{\partial^2 J^i(x^i,z^i(x))}{x^i_j x^i_1}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_j x^i_j}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_j x^i_n} \\

\vdots \\
\frac{\partial^2 J^i(x^i,z^i(x))}{x^i_n x^i_1}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_n x^i_j}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_n x^i_n} \\

\end{bmatrix}
$$

而非对角线元素是对其他人的策略的求导。因为是network game，别人的策略影响，是通过公式(2)来影响的，

$$
\nabla^2_{x^ix^k}J^i(x^i, z^i(x)) =
\begin{bmatrix}
\frac{\partial^2 J^i(x^i,z^i(x))}{x^i_1 z^i_1} \frac{\partial^2 z_1^i}{x^k_1} , \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_1 z^i_j} \frac{\partial^2 z_j^i}{x^k_j}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_1 z^i_n} \frac{\partial^2 z_n^i}{x^k_n} \\

\vdots \\
\frac{\partial^2 J^i(x^i,z^i(x))}{x^i_j z^i_1} \frac{\partial^2 z_1^i}{x^k_1}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_j z^i_j} \frac{\partial^2 z_j^i}{x^k_j}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_j z^i_n} \frac{\partial^2 z_n^i}{x^k_n}\\

\vdots \\
\frac{\partial^2 J^i(x^i,z^i(x))}{x^i_n z^i_1} \frac{\partial^2 z_1^i}{x^k_1}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_n z^i_j} \frac{\partial^2 z_j^i}{x^k_j}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_n z^i_n} \frac{\partial^2 z_n^i}{x^k_n}\\

\end{bmatrix}
$$

而$\frac{\partial^2 z_j^i}{x^k_j} = G_{ik}, \forall j\in \mathbb{N}[1,n]$. 所以：

$$
\nabla^2_{x^ix^k}J^i(x^i, z^i(x)) =
\begin{bmatrix}
\frac{\partial^2 J^i(x^i,z^i(x))}{x^i_1 z^i_1}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_1 z^i_j}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_1 z^i_n} \\

\vdots \\
\frac{\partial^2 J^i(x^i,z^i(x))}{x^i_j z^i_1}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_j z^i_j}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_j z^i_n}\\

\vdots \\
\frac{\partial^2 J^i(x^i,z^i(x))}{x^i_n z^i_1}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_n z^i_j}, \ldots, \frac{\partial^2 J^i(x^i,z^i(x))}{x^i_n z^i_n}\\

\end{bmatrix} \cdot G_{ik}
$$

### $\nabla_x F(x)$分解

根据对对角线和非对角线元素的观察，我们可以把$\nabla_x F(x)$分解成，

$$
\nabla_x F(x) = D(x) + K(x)W
$$

$$
D(x) =
\begin{bmatrix}
\nabla^2_{x^1x^1}J^1(x^1, z^1(x)) & \ldots 0 \ldots & \ldots 0 \\

\vdots \\
0\ldots & \nabla^2_{x^ix^i}J^1(x^1, z^1(x)) &\ldots 0 \\

\vdots \\
0\ldots  & \ldots 0 \ldots &\nabla^2_{x^Nx^N}J^1(x^1, z^1(x)) \\

\end{bmatrix}
$$

$$
K(x) =
\begin{bmatrix}
\nabla^2_{x^1z^1}J^1(x^1, z^1(x)), \ldots, \nabla^2_{x^1z^k}J^1(x^1, z^1(x)), \ldots, \nabla^2_{x^1z^N}J^1(x^1, z^1(x)) \\

\vdots \\
\nabla^2_{x^iz^1}J^1(x^1, z^1(x)), \ldots, \nabla^2_{x^iz^k}J^1(x^i, z^i(x)), \ldots, \nabla^2_{x^iz^N}J^1(x^1, z^1(x)) \\

\vdots \\
\nabla^2_{x^Nz^1}J^1(x^1, z^1(x)), \ldots, \nabla^2_{x^Nz^k}J^1(x^N, z^N(x)), \ldots, \nabla^2_{x^Nz^N}J^1(x^1, z^1(x)) \\

\end{bmatrix}
$$

$$
W =
\begin{bmatrix}
G_{11}\odot I_n, \ldots, G_{1k}\odot I_n, \ldots, G_{1N}\odot I_n \\

\vdots \\
G_{i1}\odot I_n, \ldots, G_{ik}\odot I_n, \ldots, G_{iN}\odot I_n \\

\vdots \\
G_{N1}\odot I_n, \ldots, G_{Nk}\odot I_n, \ldots, G_{NN}\odot I_n \\

\end{bmatrix}
$$

到此为止，就成功把这个Hessian矩阵给分解成三个部分，$D(x)$描述了_own effect_, $K(x)$描述了_network effect_, $W$描述了_network topology_. 

在下一篇中，再继续分析这个分解之后的Hessian矩阵的性质，以及网络结构是如何影响解的存在和唯一性的。

#### 脚注

[^1]: 这里每个人的决策变量是多维的，这也是这篇文章比较general的地方，虽然因为这样，后面的矩阵分析，公式显得很臃肿。