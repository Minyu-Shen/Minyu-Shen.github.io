---
layout: post
title:  A simple paradox of Bayesian
subtitle: Medical test paradox
tags: [probability]
usemathjax: true
---

这两天在3Blue1Brown上看到一个关于贝叶斯悖论的视频， [Medical test paradox](https://www.youtube.com/watch?v=lG4VkPoG3ko&ab_channel=3Blue1Brown)。大概的意思是说，对于一个比较少见的病，即使检测出来呈阳性，那么真正得这个病的概率也很低。把自己看完的理解写一下，这个问题相对简单，但是对应到实际生活中，含义还是很有趣的。先理解一些概念。

### 概念

- True positive rate (TPR):
有疾病并且检测阳性 $P(E=+\mid H=+)$ ，这个概率长被称为sensitivity, recall，顾名思义，对于有疾病的人，检测的`灵敏度`。

- True negative rate (TNR):
没病并且检测阴性 $P(E=-\mid H=-)$， 这个概率长被称为specificity或者selectivity，感觉可以翻译为特异性，表达一个`specifically` 排除其他导致positive的可能性。

- False positive rate (FPR):
没病但被误诊为阳性  $P(E=+\mid H=-)$, 这个是特异性的取1-x。

- False negative rate (FNR):
有病但被误诊为阴性 $P(E=-\mid H=+)$ , 这个是灵敏度的取1-x。

### 贝叶斯公式

大家都懂，贝叶斯公式，H是假设 (hypothesis), E是证据(evidence).

$$
P(H\mid E) = P(H)\frac{P(E\mid H)}{P(E)}
$$

这里$P(E\mid H)$是似然函数，也就是在给定模型$H$的情况下，咱能观察到我们的证据$E$的概率。再配合这个证据$E$自身Marginal distribution的大小来一起更新先验。这两个的如果它大于1，那么会`放大`先验$P(H)$，如果小于1，那么就`缩小`先验概率。 这里我们就简单用$H$表示$H=+$, $\neg H$表示$H=-$。

同样的公式，对于$\neg H$ ，

$$
P(\neg H\mid E) = P(\neg H)\frac{P(E\mid \neg H)}{P(E)}
$$

两个式子比一下，

$$
\frac{P(H\mid E)}{P(\neg H\mid E)} = \frac{P(H)}{P(\neg H)}\cdot \frac{P(E\mid H)}{P(E\mid \neg H)} = O\cdot B
$$

这里B就是`贝叶斯因子`，或者也叫做`似然比`。这里很有趣的一点是，如果把$\frac{P(H\mid E)}{P(\neg H\mid E)}$看做是有了信息之后更新的先验比，那么就等于说是用likelihood ratio的比例去更新之前的先验$\frac{P(H)}{P(\neg H)}$。


最后，因为$P(H\mid E)=1-P(\neg H\mid E)$，所以我们可以写出准确的计算公式：

$$
P(H\mid E) = \frac{OB}{1+OB}
$$

### 快速估算

我们想要的是后验概率，但是从上面的贝叶斯公式可以看到，其实快速计算这个后验概率还是挺繁琐的，主要是这个万恶的$P(E)$。那有没有一种方法能够让我们大致快速地估计这个后验呢？如果有一个很好的近似，那么我们就可以直接心算了，答案是有的：

$$
P(H\mid E) \approx P(H)\frac{P(E\mid H)}{P(E\mid \neg H)} = P(H)\cdot B
$$

这个近似啥时候准呢？根据公式(4)，如果$P(H) << 1$的话（比如说一些罕见病），那么$O\approx P(H)$。此外，如果$B$也不是很大的话，那么$OB << 1$，这个时候估计就很准了。 当OB在[0,0.1]之间的话，还是挺准的。

![comparison](/figs/curve_comp.png "two curves comparison")

在阴阳性的例子里，我们只需要知道$P(E\mid H)$，$P(E\mid \neg H)$通过1-x就能得到。如果$H$是有疾病，$E$是检测阳性，那么前者就是灵敏度；如果$H$是没有疾病，$E$是检测阴性，那么前者就是specificity。

这个时候假如一个人检测出是阳性，也就是$E$是阳性，那么我们通过公式(5)，我们只需要知道这个病在人群中的先验患病率，再乘以灵敏度，除以1-灵敏度即可。



### 悖论

悖论就是说：即使检测的成功率（比如TPR 和 TNR都达到了95%以上）很高，并且测试呈阳性，也可能因为先验非常小，导致后验的概率还是远小于TPR和TNR。

但在现实生活中，我们碰到这个的情况并没有那么多，原因就是就算某个疾病在人群中的流行度非常低，也并不能就完全代表我们（去做检测前）的先验了，因为往往是有了一些症状之后，才会选择去做测试的，也就是说，人群中的流行率这个先验已经被更新了，$P(H=+\mid Symptoms) > P(H=+)$.