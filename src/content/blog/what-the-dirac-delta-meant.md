---
title: "What the Dirac Delta meant"
description: "In previous posts, I shown how the importance of de Dirac Delta in computing the distribution density of a function of random variables, but I missed to show where it came from."
pubDate: "2023-07-18"
heroImage: "../../assets/blog-placeholder-3.jpg"
---

In [previous posts](/calculating-a-distribution-of-a-function-of-random-variables.html), I shown how the importance of the Dirac Delta in computing the distribution density of a function of random variables, but I missed to show where it came from.

From the dice idea, the probability of finding two dice which the sum is $s$ is to sum all the probabilities in which the sum is $s$. Sounds strange, isn't it? But it is that simple, I guess. If the set of all possible outcomes of two dice is $\Omega$ and the set of outcomes in which the sum is $s$ is $\Omega_s$, then the final probability is the sum over the intersection of these sets:

$$
P(sum=s) = \sum_{i,j \in \Omega \cap \Omega_s} P(face=i) P(face=j)
$$

There is another way to write the same equation, using the [Kroeneker delta](https://en.wikipedia.org/wiki/Kronecker_delta):

$$
P(sum=s) = \sum_{i,j\in\Omega}P(face=i)P(face-j)\delta_{i+j,s}
$$

The same idea applies to the continuous case:

$$
\begin{align*}
p(y) &= \int_{\Omega \cap \Omega_y} p(x_1)...p(x_i)...p(x_n) dx^n\\
    &= \int_\Omega p(x_1)...p(x_i)...p(x_n) \delta\left(y-f(x_1, ..., x_n)\right) dx^n
\end{align*}
$$

It is related to [measure theory](https://en.wikipedia.org/wiki/Measure_(mathematics)). I have to study measure theory ^^".
