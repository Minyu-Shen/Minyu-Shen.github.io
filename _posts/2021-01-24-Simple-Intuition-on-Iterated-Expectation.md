---
layout: post
title:  Simple intuition on law of iterated expectation
subtitle: Nested form
tags: [probability]
usemathjax: true
---

最近一个月都在搞论文的时候，没有更新。这几天隔离期空闲了，打算重新回顾下强化学习的一些基础知识；而在MDP里一些基础公式的推导，有时候有law of iterated expectation的影子，所以先稍微扯一下（其实就是偷个懒<（￣︶￣）>），后续更新RL的内容。

先从基本形式开始，再介绍一个嵌套形式的。

### 1. $E[E[X|Y]] = E[X]$

有一个非常简单的例子，假如现在我们想去统计整个人群中的平均身高$E[X]$，那么我们可以把采集到的数据分成男和女，我们可以先统计男生的平均身高，然后再统计女生的平均身高，分别是


$$
E[X|Y=``male" ],\, E[X|Y=``female" ]
$$


然后把这个两个期望数，用男女各自的比例加权平均一下就行了。


$$
E[X] = P(Y=``male")\cdot E[X|Y=``male" ] + P(Y=``male")\cdot E[X|Y=``female" ]
$$


为什么这样是对的呢？下面我们把期望展开一下，为了更general一点，考虑$X,Y$都是连续的情况。


$$
E[E[X|Y]] = E\left[ \int{x\cdot p(x|Y)dx} \right] = \int{p(Y=y)\left[\int{x\cdot p(x|Y=y)dx}\right]dy}
$$


第二个等式是因为积掉$x$之后，就只剩随机变量$Y$了，可以看成一个关于随机变量的函数$f(Y)$。

然后根据 **富比尼定理** ，只要整个积分bounded，我们就能交换顺序：


$$
\int{x} \int{p(Y=y)\cdot p(x|Y=y)dy}dx = \int{x} \int{p(X=x,Y=y)dy}dx = \int{x } p(X=x)dx = E[X]
$$


第一个等式是条件概率，第二个等式是全概率公式。



### 2. $E[E[X|Z,Y]|Y] = E[X|Y]$

这第二个看上去有点蛋疼，但还用的蛮多的。从1.里面的例子延展一下，假设我们现在并不想知道整个人群的身高，而是就想知道女生的平均身高，那咱可以把女生再细分一下，比如说分成戴眼镜和不戴眼镜的（随便想的◑ˍ◐），用$Z$随机变量来表示。

统计女生并且戴眼镜的平均身高，以及女生不戴眼镜的平均身高，分别是


$$
E[X|Y=``female",Z=``no\,glasses""], E[X|Y=``female",Z=``glasses"]
$$


然后把这两个期望数，用女生中戴眼镜和不戴眼镜的比例加权平均一下就行了，现在我们关注在女生上，女生中戴眼镜的概率，是一个条件概率，加权平均就得到了。


$$
E[X|Y] = P(Z=no|Y=F)\cdot E[X|Y=F,Z=no] + P(Z=yes|Y=F)\cdot E[X|Y=F,Z=yes]
$$


（我把引号去掉，用F代表femal，no代表不戴眼镜，要不然看着有点难受，😋）

同样的，我们稍微严格证明下为什么这是对的；其实$Y$未必需要一定是 $F$，可以更加general一点，就看成是个随机变量$Y$，于是上面这个式子的右边，用连续变量表示的话，就是


$$
\int{p(Z=z|Y) E[X|Z=z,Y]dz} = \int{p(Z=z|Y) \int{x\cdot p(X=x|Z=z,Y)dx}dz}
$$


然后做一些跟1.里面类似的变换，


$$
\int{x\int{p(Z=z|Y)\cdot p(X=x|Z=z,Y)dz}dx}
$$


根据概率的链式法则，


$$
\int{x\int{ p(X=x,Z=z|Y) dz}dx} = \int{x\cdot p(X=x|Y)dx} = E[X|Y]
$$


搞定~ 睡觉。