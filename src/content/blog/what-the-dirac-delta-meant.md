---
title: "What the Dirac Delta Meant"
description: "In previous posts, I showed the importance of the Dirac Delta in computing the distribution density of a function of random variables, but I missed showing where it came from."
pubDate: "2023-07-18"
heroImage: "../../assets/blog-placeholder-3.jpg"
---

In [previous posts](/blog/calculating-a-distribution-of-a-function-of-random-variables), I showed the importance of the Dirac Delta in computing the distribution density of a function of random variables, but I didn't show where it comes from.

Starting from the dice example, the probability of finding two dice whose sum is $s$ is simply the sum of all probabilities where the sum is $s$. Sounds strange, doesn't it? But it's really that simple. If the set of all possible outcomes of two dice is $\Omega$ and the set of outcomes where the sum is $s$ is $\Omega_s$, then the final probability is the sum over the intersection of these sets:

$$
P(sum=s) = \sum_{i,j \in \Omega \cap \Omega_s} P(face=i) P(face=j)
$$

There's another way to write the same equation, using the [Kronecker delta](https://en.wikipedia.org/wiki/Kronecker_delta):

$$
P(sum=s) = \sum_{i,j\in\Omega}P(face=i)P(face=j)\delta_{i+j,s}
$$

The same idea applies to the continuous case:

$$
\begin{align*}
p(y) &= \int_{\Omega \cap \Omega_y} p(x_1)...p(x_i)...p(x_n) dx^n\\
    &= \int_\Omega p(x_1)...p(x_i)...p(x_n) \delta\left(y-f(x_1, ..., x_n)\right) dx^n
\end{align*}
$$

This is related to [measure theory](https://en.wikipedia.org/wiki/Measure_(mathematics)). I still have to study measure theory ^^".

![Fair die distribution](/images/delta-random-variable_2_0.png)

![Two dice sum distribution](/images/delta-random-variable_4_0.png)

![Two Uniform distribution sum (z = x + y)](/images/delta-random-variable_9_0.png)
