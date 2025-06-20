---
title: "Calculating the Distribution of a Function of Random Variables"
description: "Although I am a physicist, I have never taken an advanced course in statistics. However, I have faced many situations in my work where I needed to deal with random variables. This has led me to become interested in the use of the Dirac delta function to model the distribution of a function of a set of random variables."
pubDate: "2023-07-17"
heroImage: "../../assets/blog-placeholder-2.jpg"
---

Although I am a physicist, I have never taken an advanced course in statistics. However, I have faced many situations in my work where I needed to deal with random variables. This has led me to become interested in the use of the Dirac delta function to model the distribution of a function of a set of random variables.

The Dirac delta, represented as $\delta(x)$, is a generalized function that has the nice property:

$$
\int_\Omega f(x) \delta(x-a) dx = f(a), ~\forall a \in \Omega
$$

This makes it well-suited for modeling the distribution of a function of a set of random variables, because it allows us to calculate the contribution of a single point.

For instance, the distribution of the random variable $\hat{Z}$, $p(z)$, given by the equation $\hat{Z} = f\left(\hat{X}, \hat{Y}\right)$, in which the random variables are distributed according to the density function $p(x, y)$ with domain $\Omega$, is given by

$$
p(z) = \int_\Omega p(x, y) \delta\left(z - f(x, y)\right)dxdy
$$

In the rest of this blog post, I will give the intuition behind how the distribution of a function of random variables works, using the sum of rolling independent dice as an example. I will then present the general formula for discrete random variables, and then for continuous random variables. Finally, I will discuss the situation of conditional probabilities, sampling, and some interesting observations.

## Craps

In many movies featuring casinos, the characters play a game called [craps](https://en.wikipedia.org/wiki/Craps), in which the points of each player come from the result of the sum of the values of two dice. We know that the distribution of a fair die is $1/6$ for each face, but what about two dice?

First, let's check an empirical distribution of the sum of two fair dice. By fair, I mean the chance of getting any of its faces is equal to $1/6$, i.e., it is a [uniform distribution](https://en.wikipedia.org/wiki/Continuous_uniform_distribution). Its distribution is shown below:

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.ticker import PercentFormatter

g = np.random.default_rng(0x533D)
N = 1_000_000
sample = g.integers(1, 7, size=N)
plt.hist(sample, density=True, bins=range(1, 8), align="left", edgecolor='black')
ax = plt.gca()
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
plt.title("Fair die distribution")
plt.ylabel("Percentage")
plt.xlabel("Die face")
plt.xticks(range(1, 7))
ax.yaxis.set_major_formatter(PercentFormatter(1))
plt.show()
```

![Fair die distribution](/images/delta-random-variable_2_0.png)

As expected, each face was drawn approximately $1/6\sim 16.\bar{6}$%. It is a uniform distribution. If we sum two of these uniform distributions, we get the two-dice sum distribution, which is not a uniform distribution, as shown below:

```python
g = np.random.default_rng(0x533D)
N = 100_000
sample = g.integers(1, 7, size=(N, 2))
sample_sum = sample.sum(axis=1)
plt.hist(sample_sum, density=True, bins=range(2, 14), align="left", edgecolor='black')
ax = plt.gca()
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
plt.title("Two fair dice sum distribution")
plt.ylabel("Percentage")
plt.xlabel("Dice sum")
plt.xticks(range(2, 13))
ax.yaxis.set_major_formatter(PercentFormatter(1))
plt.show()
```

![Two dice sum distribution](/images/delta-random-variable_4_0.png)

Even though the distribution of a die is uniform, the sum of two dice has a pyramidal shape. We can reason about this shape by calculating how many ways the dice can land to generate each result. For example, to get the value $3$, the dice could be (1, 2) or (2, 1), but to get the value 2, both dice must land on 1: (1, 1). All the possibilities are mapped in the table below:

| Dice sum | Possible outcomes                | # outcomes | percentage         |
|:--------:|:------------------------------- |:----------:|:------------------:|
| 1        | -                               | 0          | impossible         |
| 2        | (1, 1)                          | 1          | $2.\bar7$%         |
| 3        | (1, 2), (2, 1)                  | 2          | $5.\bar5$%         |
| 4        | (1, 3), (2, 2), (3, 1)          | 3          | $8.\bar3$%         |
| 5        | (1, 4), (2, 3), (3, 2), (4, 1)  | 4          | $11.\bar1$%        |
| 6        | (1, 5), (2, 4), (3, 3), (4, 2), (5, 1) | 5   | $13.\bar8$%        |
| 7        | (1, 6), (2, 5), (3, 4), (4, 3), (5, 2), (6, 1) | 6 | $16.\bar6$% |
| 8        | (2, 6), (3, 5), (4, 4), (5, 3), (6, 2) | 5   | $13.\bar8$%        |
| 9        | (3, 6), (4, 5), (5, 4), (6, 3)  | 4          | $11.\bar1$%        |
| 10       | (4, 6), (5, 5), (6, 4)          | 3          | $8.\bar3$%         |
| 11       | (5, 6), (6, 5)                  | 2          | $5.\bar5$%         |
| 12       | (6, 6)                          | 1          | $2.\bar7$%         |
| >12      | -                               | 0          | impossible         |
| **Total**|                                  | **36**     | **100%**           |

The pyramidal shape appears again! It happens because different outcomes can generate the same dice sum. Let's redo the graph above with the numbers we have found:

```python
g = np.random.default_rng(0x533D)
N = 100_000
sample = g.integers(1, 7, size=(N, 2))
sample_sum = sample.sum(axis=1)
dice_sum_proba = np.array([1, 2, 3, 4, 5, 6, 5, 4, 3, 2, 1]) / 36
# Central Limit Theorem in 95% confidence
err = [3.2 * np.sqrt(p*(1-p)/N) for p in dice_sum_proba]
plt.hist(sample_sum, density=True, bins=range(2, 14), align="left", edgecolor='black')
plt.errorbar(list(range(2, 13)), dice_sum_proba, yerr=err, fmt='o', capsize=4)
ax = plt.gca()
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
plt.title("Two fair dice sum distribution")
plt.ylabel("Percentage")
plt.xlabel("Dice sum")
plt.xticks(range(2, 13))
ax.yaxis.set_major_formatter(PercentFormatter(1))
plt.show()
```

![Two dice sum distribution with error bars](/images/delta-random-variable_6_0.png)

The values from the simulation are within the error bars (the error bars are not exact, but an approximation). So our calculation makes sense! We can reason about what we have done using the [Kronecker delta](https://en.wikipedia.org/wiki/Kronecker_delta):

$$
p(s) = \sum_{i=1}^6\sum_{j=1}^6 p(i) p(j) \delta_{i+j,s},~~~ \delta_{a, b} =
\begin{cases}
    1 & \text{if } a=b \\\\
    0 & \text{otherwise }
\end{cases}
$$

For instance, for $p(s=3)$:

$$
\begin{align*}
p(s=3) &= \sum_{i=1}^6\sum_{j=1}^6 p(i) p(j) \delta_{i+j,3}\\
       &= p(1) p(2) + p(2) p(1) = \frac{1}{6} \frac{1}{6} + \frac{1}{6} \frac{1}{6}\\
       &= \frac{2}{36} = 5.\bar5\%\\
\end{align*}
$$

## The triangle

The same can be applied to continuous functions with:

$$
p(z) = \int_\Omega p(x, y) \delta\left(z - f(x, y)\right)dxdy
$$

It has nice properties, since if we integrate it over the $z$ domain $\Omega_z$ we have 1, as expected:

$$
\int_{\Omega_z}p(z)dz = \int_\Omega p(x, y) \int_{\Omega_z}\delta\left(z - f(x, y)\right)dzdxdy = \int_\Omega p(x, y) dxdy = 1
$$

Let's make an example for the sum of two continuous independent uniform distributions $\hat{X}\sim U[0,1]$ and $\hat{Y}\sim U[0,1]$:

$$
\hat{Z} = \hat{X} + \hat{Y}
$$

So the final distribution is given by the integral below:

$$
\begin{align*}
p(z) &= \int_\Omega p(x, y) \delta\left(z - x - y\right)dxdy\\
     &= \int_0^1\int_0^1 \delta\left(x - z +y\right)dxdy\\
     &= \begin{cases}
    \int_{\max(0, z - 1)}^{\min(1, z)} dx & \text{if } 0<z<2 \\
    0 & \text{otherwise }
\end{cases} \\
     &= \begin{cases}
    \min(1, z) -  \max(0, z - 1)& \text{if } 0<z<2 \\
    0 & \text{otherwise }
\end{cases}
\end{align*}
$$

```python
@np.vectorize(otypes=[np.float64])
def p(z):
    if 0 < z < 2:
        return min(1, z) - max(0, z-1)
    else:
        return 0

z = np.linspace(0, 2.5, num=1_000)
prob = p(z)
plt.plot(z, prob)
ax = plt.gca()
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
plt.title("Two Uniform distribution sum (z = x + y)")
plt.ylabel("Percentage")
plt.xlabel("z")
ax.yaxis.set_major_formatter(PercentFormatter(1))
plt.show()
```

![Two Uniform distribution sum (z = x + y)](/images/delta-random-variable_9_0.png)

Now, let's test if we get the same result from a simulation:

```python
g = np.random.default_rng(0x533D)
N = 1_000_000
sample = g.uniform(size=N) + g.uniform(size=N)
plt.hist(sample, density=True, bins=100)
plt.plot(z, prob)
ax = plt.gca()
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
plt.title("Two Uniform distribution sum (z = x + y)")
plt.ylabel("Percentage")
plt.xlabel("z")
ax.yaxis.set_major_formatter(PercentFormatter(1))
plt.show()
```

![Two Uniform distribution sum (z = x + y) - simulation](/images/delta-random-variable_11_0.png)

Great! I know I promised myself to write more topics here. But I deviated from the task and realized that I might have to study measure theory ^^".
