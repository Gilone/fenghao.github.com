---
title: "Detailed proof of the optimality property of Mean Absolute Error"
layout: post
date: 2021-10-10 20:59
image: https://upload.wikimedia.org/wikipedia/commons/thumb/a/ad/Gottfried-Wilhelm-Leibniz-Louis-Dutens-opera-omnia_MG_1180.tif/lossy-page1-220px-Gottfried-Wilhelm-Leibniz-Louis-Dutens-opera-omnia_MG_1180.tif.jpg
headerImage: True
tag:
- Tecch
category: blog
projects: false
author: Hao Feng
description: Detailed proof of the optimality property of Mean Absolute Error
---

In the homework of *CSE258 Data Mining and Recommendation System*, there is a very interesting question.

>Show that for a trivial predictor, i.e., y = θ0, the best possible value of θ0 in terms of the Mean Absolute Error is the median of the label y.

Where to start this proof? How to get its derivative?

The breakthrough of the problem is to treat MAE as a expectation of a random variable. Therefore, I have my idea.

For a trivial predictor,
$$
Mean\ Absolute\ Error=\frac{1}{N} \sum_{i=1}^{N}\left|y_{i}-\theta_{0}\right|
$$

To minimize the MAE, suppose we have a random variable $X$, which represents label $y$.  
The MAE of the $\theta_{0}$ with respect to the random variable $X$ is
$$
E(|X-\theta_{0}|)
$$
Thus we need to solve:
$$
\min _{\theta_{0} \in \mathbb{R}} E(|X-\theta_{0}|)
$$

Let $p$ be the probability density function of $X$ and let $J(\theta_{0})=E(|X-\theta_{0}|)$, the expectation could be represented as\\
$$
J(\theta_{0})=\int_{\mathbb{R}}|x-\theta_{0}|p(x)dx=\int_{-\infty}^{\theta_{0}}(\theta_{0}-x)p(x)dx+\int_{\theta_{0}}^{\infty}(x-\theta_{0})p(x)dx
$$

In order to find the minimum of this function with respect to $\theta_{0}$, we need to find its derivative and then set it to 0. Hence, we get that,
$$
\frac{dJ(\theta_{0})}{d\theta_{0}}=
\frac{d}{d\theta_{0}}\int_{-\infty}^{\theta_{0}}(\theta_{0}-x)p(x)dx +
\frac{d}{d\theta_{0}}\int_{\theta_{0}}^{\infty}(x-\theta_{0})p(x)dx=0
$$
Using \textit{Leibniz integral rule} $\frac{d}{dx}\left(\int_{a}^{x} f(x,t)dt\right)=f(x, x)+\int_{a}^{x} \frac{\partial}{\partial x} f(x,t)dt$ we get,
$$
\frac{d}{d\theta_{0}}\int_{-\infty}^{\theta_{0}}(\theta_{0}-x)p(x)dx=
(\theta_{0}-\theta_{0})p(x)+\int_{-\infty}^{\theta_{0}} \frac{\partial}{\partial \theta_{0}}[(\theta_{0}-x)p(x)] d x=\int_{-\infty}^{\theta_{0}}p(x)dx
$$
$$
\frac{d}{d\theta_{0}}\int_{\theta_{0}}^{\infty}(x-\theta_{0})p(x)dx=
-(\theta_{0}-\theta_{0})p(x)+\int_{\theta_{0}}^{\infty} \frac{\partial}{\partial \theta_{0}}[(x-\theta_{0})p(x)] d x=-\int_{\theta_{0}}^{\infty}p(x)dx \\
$$
Thus,
$$
\frac{dJ(\theta_{0})}{d\theta_{0}}=
\int_{-\infty}^{\theta_{0}}p(x)dx-\int_{\theta_{0}}^{\infty}p(x)dx=0
$$
Therefore, the solution of $\theta_{0}$ is
$$
\int_{-\infty}^{\theta_{0}}p(x)dx=\int_{\theta_{0}}^{\infty}p(x)dx
$$
Let the probability be $P(X)$, thus the solution is
$$
P(X\leq\theta_{0})=P(X\geq\theta_{0})
$$

Because $P(X)=P(X\leq\theta_{0})+P(X\geq\theta_{0})=1$,
$$
P(X\leq\theta_{0})=P(X\geq\theta_{0})=\frac{1}{2}
$$
The medians of $X$ are defined as any number $m \in \mathbb{R}$ such that
$$
P(X \leq m) \geq \frac{1}{2}\ and\ P(X \geq m) \geq \frac{1}{2}
$$
According to the definition, $\theta_{0}$ is the \textit{median} of random variable $X$, i.e. label $y$.\\
Therefore, the best possible value of $\theta_{0}$ in terms of the Mean Absolute Error is the median of the label $y$.
