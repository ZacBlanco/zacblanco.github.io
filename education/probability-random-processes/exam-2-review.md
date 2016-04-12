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