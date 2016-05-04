---
layout: page
title: Probability & Random Processes - Final Exam Review
keywords: rutgers, university, electrical, computer, engineering, ece, 14:332:226, ECE226, final, exam, review, equation, sheet, probability, PMF, stochastic, processes, PDF, continuous, random, variables
permalink: /education/probability-random-processes/final-review/
mathjax: true
author: Zac Blanco
---

This page will just contain the most essential equations that pertain to topics on the final exam

## Set Theory

1. $$ P[A\cap B] = P[AB]$$
2. $$ P[A] \geq 0 $$
3. $$ P[\varnothing ] = 0 $$
4. $$ P[A \cup B] = P[A] + P[B] - P[A \cap B] $$
5. $$ P[A^c] = 1 - P[A] $$
6. $$ P[A \vert B ] = \frac{P[AB]}{P[B]} $$
7. $$ P[B\vert B] = 1 $$
8. $$ \text{Bayes Theorem: } P[B\vert A] = \frac{P[A\vert B]P[B]}{P[A]} $$
9. $$ \text{n choose k} = \begin{pmatrix} n\\k \end{pmatrix} = \frac{n!}{k!(n-k)!} $$

## Table of Discrete Random Variables

|Name of Random Variable| Description of variable | PMF Function | Expected Value- $$E[X]$$ | Variance - $$Var[X]$$| 
|-----------------------|-------------------------|--------------|
|Bernoulli|number successes in 1 trial		|$$ P_X(x) = \begin{cases} 1-p , & x = 0 \\ p , & x = 1 \\ 0, & \text{otherwise} \end{cases} $$| $$p$$ | $$p(1-p)$$ |
|Geometric|number of trials until 1st success|$$ P_X(x) = \begin{cases} p(1-p)^{x-1}, & x = 0 \\0, & \text{otherwise} \end{cases} $$ |  $$\frac{1}{p}$$ |  $$\frac{1-p}{p^2}$$ |
|Binomial |number of successes in n trials	|$$ P_X(x) = \begin{pmatrix} n \\ x \end{pmatrix} p^x(1-p)^{n-x} $$ |  $$np$$ |  $$np(1-p)$$ |
|Pascal   |number of trials until k successes|$$ P_X(x) = \begin{pmatrix} x-1 \\ k-1 \end{pmatrix} p^k(1-p)^{x-k} $$ | $$\frac{k}{p}$$ |  $$\frac{k(1-p)}{p^2}$$ |
| Poisson | Probability of x arrivals in T seconds | $$ P_X(x) = \begin{cases} \frac{\alpha^xe^{-\alpha}}{x!}, x = 0, 1, 2\dots \\ 0,\text{otherwise} \\ \end{cases} $$ | $$\alpha$$ | $$\alpha$$ |
|Discrete Uniform | Probability of any value between k and l | $$ P_X(x) = \begin{cases} \frac{1}{l - k + 1},\  x=k, k+1, k+2\dots \\ 0, \text{otherwise} \end{cases} $$ | $$\frac{k + l}{2}$$ | $$\frac{(l-k)(l-k+2)}{12}$$ |


## Table of Continuous Random Variables

| Name | Form | PDF | CDF | $$E[X]$$ | $$Var[X]$$ |
| Uniform| $$(a, b)$$| $$\begin{cases} \frac{1}{b-a} & a \leq x < b \\ 0 & \text{otherwise} \\ \end{cases}$$| $$\begin{cases} 0 & x \leq a \\ \frac{x-a}{b-a} & a < x \leq b \\ 1 & x > b \\ \end{cases} $$|  $$\frac{b+a}{2}$$| $$\frac{(b-a)^2}{12} $$ |
| Exponential | $$(\lambda)$$| $$\begin{cases} \lambda e^{-\lambda x} & x \geq 0 \\ 0 & \text{otherwise} \\ \end{cases}$$ | $$\begin{cases}  1- e^{-\lambda x} & x \geq 0 \\ 0 & \text{otherwise} \\ \end{cases}$$| $$\frac{1}{\lambda}$$ | $$\frac{1}{\lambda^2}$$|
|Erlang| $$(n, \lambda)$$|$$\begin{cases} \frac{\lambda x^{n-1}e^{-\lambda x}}{(n-1)!} & x > 0 \\ 0 & \text{otherwise} \\ \end{cases}$$| | $$\frac{n}{\lambda}$$ | $$\frac{n}{\lambda^2} $$|
| Gaussian | $$(\mu, \sigma)$$| $$\frac{1}{\sigma\sqrt{2\pi}}e^{-(x-\mu)^2/2\sigma^2}$$ | *$$\Phi(z) = \frac{1}{\sqrt{2\pi}}\int\limits_{-\infty}^z e^{-u^2/2} du$$ | $$\mu$$ | $$\sigma^2$$ | 

- **\*** Note that this is the _standard normal_ CDF of the Gaussian random variable. To adjust for $$\mu$$ and $$\sigma$$ use $$\Phi(\frac{x - \mu}{\sigma})$$


## List of Essential Equations

1. $$ P[x_1 < x < x_2] = F_X(x_2) - F_X(x_1) $$
2. $$\int\limits_{-\infty}^\infty f_X(x)dx = 1 $$

#### Expected Value

1. $$E[X] = \int\limits_{-\infty}^\infty xf_X(x) dx $$
2. $$E[X-\mu_x]= 0$$
3. $$ E[aX + b] = aE[X] + b = 0$$
4. $$E[X+Y] = E[X] + E[Y]$$

#### Variance

1. $$ Var[X] = E[X^2] - \mu_X^2 $$
2. $$ Var[X] = E[(X-\mu_X)^2] $$
3. $$ Var[aX + b] = a^2Var[X] $$
4. $$Var[X+Y] = Var[X] + Var[Y] + 2Cov[X, Y]$$

#### Covariance & Correleational Coefficients

1. $$\rho_{X,Y} = \frac{Cov[X, Y]}{\sqrt{Var[X]Var[Y]}}$$
2. $$-1 \leq \rho_{X,Y} \leq 1$$
3. $$ r_{X,Y} = E[XY]$$
4. $$Cov[X,Y] = r_{X,Y} - \mu_X\mu_Y $$
5. $$Cov[X,X] = Var[X] = E[X^2] - (E[X])^2$$

#### Independent Variables

Given two **_independent_** random variables:

1. $$f_{X,Y}(x, y) = f_X(x)f_Y(y)$$
2. $$r_{X,Y} = E[XY] = E[X]E[Y]$$
3. $$Cov[X,Y] = \rho_{X,Y} = 0$$
4. $$Var[X+Y] = Var[X] + Var[Y]$$


















