---
layout: page
title: Probability & Random Processes - Exam 2 Review
keywords: rutgers, university, electrical, computer, engineering, ece, 14:332:226, ECE226
permalink: /education/probability-random-processes/exam-2/
mathjax: true
author: Zac Blanco
---

I'm going to use this page to help gather some of the most important information which will pertain to exam 2 and put it here!

### Families of Continuous Random Variables

| Name | Form | PDF | CDF | $$E[X]$$ | $$Var[X]$$ |
| Uniform| $$(a, b)$$| $$\begin{cases} \frac{1}{b-a} & a \leq x < b \\ 0 & \text{otherwise} \\ \end{cases}$$| $$\begin{cases} 0 & x \leq a \\ \frac{x-a}{b-a} & a < x \leq b \\ 1 & x > b \\ \end{cases} $$|  $$\frac{b+a}{2}$$| $$\frac{(b-a)^2}{12} $$ |
| Exponential | $$(\lambda)$$| $$\begin{cases} \lambda e^{-\lambda x} & x \geq 0 \\ 0 & \text{otherwise} \\ \end{cases}$$ | $$\begin{cases}  1- e^{-\lambda x} & x \geq 0 \\ 0 & \text{otherwise} \\ \end{cases}$$| $$\frac{1}{\lambda}$$ | $$\frac{1}{\lambda^2}$$|
|Erlang| $$(n, \lambda)$$|$$\begin{cases} \frac{\lambda x^{n-1}e^{-\lambda x}}{(n-1)!} & x > 0 \\ 0 & \text{otherwise} \\ \end{cases}$$| | $$\frac{n}{\lambda}$$ | $$\frac{n}{\lambda^2} $$|
| Gaussian | $$(\mu, \sigma)$$| $$\frac{1}{\sigma\sqrt{2\pi}}e^{-(x-\mu)^2/2\sigma^2}$$ | *$$\Phi(z) = \frac{1}{\sqrt{2\pi}}\int\limits_{-\infty}^z e^{-u^2/2} du$$ | $$\mu$$ | $$\sigma^2$$ | 

- **\*** Note that this is the _standard normal_ CDF of the Gaussian random variable. To adjust for $$\mu$$ and $$\sigma$$ use $$\Phi(\frac{x - \mu}{\sigma})$$

### List of Essential Equations

1. $$ P[x_1 < x < x_2] = F_X(x_2) - F_X(x_1) $$
2. $$\int\limits_{-\infty}^\infty f_X(x)dx = 1 $$
3. $$E[X] = \int\limits_{-\infty}^\infty xf_X(x) dx $$
4. $$E[X-\mu_x]= 0$$
5. $$ E[aX + b] = aE[X] + b = 0$$
6. $$ Var[X] = E[X^2] - \mu_X^2 $$
7. $$ Var[X] = E[(X-\mu_X)^2] $$
7. $$ Var[aX + b] = a^2Var[X] $$
8. $$E[X+Y] = E[X] + E[Y]$$
9. $$Var[X+Y] = Var[X] + Var[Y] + 2Cov[X, Y]$$
10. $$\rho_{X,Y} = \frac{Cov[X, Y]}{\sqrt{Var[X]Var[Y]}}$$
11. $$-1 \leq \rho_{X,Y} \leq 1$$
12. $$ r_{X,Y} = E[XY]$$
13. $$Cov[X,Y] = r_{X,Y} - \mu_X\mu_Y $$
14. $$Cov[X,X] = Var[X] = E[X^2] - (E[X])^2$$

Given two **_independent_** random variables:

1. $$f_{X,Y}(x, y) = f_X(x)f_Y(y)$$
2. $$r_{X,Y} = E[XY] = E[X]E[Y]$$
3. $$Cov[X,Y] = \rho_{X,Y} = 0$$
4. $$Var[X+Y] = Var[X] + Var[Y]$$
