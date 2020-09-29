---
layout: page
title: Linear Systems and Signals Notes
keywords: sophocles, linear, systems, signals, convolution, integral, calculus, electrical, engineering, rutgers, 332, ECE
permalink: /education/linear-systems-signals/notes/
mathjax: true
---

Notes for Linear Systems and Signals Fall 2016

Professor: Sophocles Orfanidis

## Chapter 2 Time Domain Analysis of Continuous-Time Systems

A total system response is:

> **Total Response** = zero-input response + zero-state response

- **zero-input response**: $$ = x(t) = 0$$
- **zero-state response**: is the system's response to any external input, say, $$x(t)$$ when the system is in a zero-state (absence of any other energies)

Given an Nth order differential equation have the following

**Zero-State Response of Differential Equation**

> $$ Q(D)y(t) = P(D)x(t) $$

**Zero-Input Response of Differential Equation**

> $$ Q(D)y_0(t) = 0 $$

From these two equation we can obtain the general solution to a differential equation as $$y_0(t) + y(t)$$

#### Solving for the Zero-Input Response

For a quick review of our differential equations we should remember that all terms within the zero-input response should be of the form: $$ce^{\lambda t}$$

Recall that the **zero-input term** is equal to the following:

> $$ Q(D)y_0(t) = 0 $$

Note the term $$Q(D)$$. This is a polynomial which denotes the differentials in the equation and can help us evaluate which terms will be present in our zero-input response.

Given a polynomial $$Q(D)$$ we can solve for $$Q(D) = 0$$ to find the roots of the polynomial to help find the zero-input response.

We call $$Q(D)$$ the **characteristic polynomial**

The **characteristic equation** is $$Q(D) = 0$$

#### Non-Repeating Real Roots Polynomial

In the case that the roots of polynomial $$Q(D)$$ are **real roots** we can use the following process to solve for the zero-input response.

Assume the roots of polynomial $$Q(D)$$ with $$n$$ number of roots to have the roots $$ = \Lambda = \{\lambda_0, \lambda_1, \dots, \lambda_n \} $$

1. Once we've found our roots $$\Lambda$$, then we can find the **characteristic terms** to be equal to $$\{ e^{\lambda_0t}, e^{\lambda_1t}, \dots, e^{\lambda_nt} \} $$
2. This makes the **zero-input equation** $$y_0(t) = \sum\limits_{i=0}^n c_ie^{\lambda_i t}$$ 
3. From this we need for solve for our constants $$c_0 \dots c_n$$. To do this we need to:
  - Find the the 1 to nth derivative of the zero-input equation
  - Find/Get the the zero-input response at $$t = 0^-$$ for the 0 to nth derivative equations.
  - Solve the system of equations to get $$c_0 \dots c_n$$

#### Repeating Real Roots Polynomial

Given the steps found above for _Non-Repeating Real Roots Polynomial_ we can follow a similar procedure except that given we have, say, $$k$$ repeating roots of $$\lambda$$ in the polynomial, then the roots which correspond for $$[0, k]$$ are represented in the following way:

**Characteristic Terms**: $$\{t^0e^{\lambda t}, t^1e^{\lambda t}, \dots, t^ke^{\lambda t} \}$$

This makes the zero-input response equal to:

> $$ y_0(t) = \sum\limits_{i=0}^k c_it^ie^{\lambda t} $$

Using that information you can solve for the contants of the zero-input response by the same method as above for real-rooted polynomials.

#### Complex Roots Polynomial

Polynomials with complex ($$j$$) roots are slightly more complex.

It is imperative to remember **Euler's Formula**

> $$ e^{j\theta} = cos(\theta) + jsin(\theta) $$

and 

> $$ e^{-j\theta} = cos(\theta) - jsin(\theta) $$

Given a polynomial with complex roots, due to the nature of the quadratic equation, $$ r = \frac{-b \pm \sqrt{b^2 - 4ac} }{2a} $$, any complex roots must appear in pairs, such that they are **complex conjugates**, say, $$(\alpha + j\beta)$$ and $$ (\alpha - j\beta) $$.

If we treat these roots similar to the real-rooted polynomials then we would end up with an equation similar to the following:

> $$ y_0(t) = c_1e^{(\alpha + j\beta)t} + c_2e^{(\alpha - j\beta)t} $$

For a _real_ system, the response of $$y_0(t)$$ should be real as well. That is only the case such if $$c_1$$ and $$c_2$$ are conjugates.

We should then set $$ c_1 = \frac{c}{2}e^{j\theta} $$ and $$c_2 = \frac{c}{2}e^{-j\theta} $$ where we introduce a new parameter, $$\theta$$

This will then result in our zero-input response being

$$ y_0(t) = \frac{c}{2}e^{j\theta}e^{(\alpha + j\beta)t} + \frac{c}{2}e^{-j\theta}e^{(\alpha - j\beta)t} $$

This allows us to separate the roots into real and imaginary components.

$$ = \frac{c}{2}e^{\alpha t}[e^{j\beta t + \theta} + e^{-j(\beta t + \theta)}] $$

Then doing some algebra..:

$$  = \frac{c}{2}e^{\alpha t} [cos(\beta t + \theta) + jsin(\beta t + \theta) + cos(\beta t + \theta) - jsin(\beta t + \theta)] $$

$$  = \frac{c}{2}e^{\alpha t} [2cos(\beta t + \theta)] $$

Which finally gives us a nice simplified zero-input equation where we can solve for the constants $$c$$ and $$theta$$:

$$ y_0(t) = ce^{\alpha t}cos(\beta t + \theta) $$


### The Unit Impulse Response

Given our differential equation before

> $$ Q(D)y(t) = P(D)x(t) $$

We want to define a new quantity which we call the **unit impulse response**, $$h(t)$$.

This quantity allows us to impart initial conditions on a system given the correct system parameters.

Let's redefine $$Q(D)$$ and $$P(D)$$ by

> $$ (D^N + a_1D^{N-1} + \dots + a_n)y(t) = (b_0D^N + b_1D^{N-1} + \dots + b_n)x(t) $$

Where $$x(t) = \delta(t)$$, the **unit-impulse** function. We then define the **unit-impulse** to be

> $$ h(t) = b_0\delta(t) + \text{characteristic modes} = b_0\delta(t) + (P(D) y_n(t))u(t)$$

We should also note that **if the order of $$P(D)$$ is less than that of $$Q(D)$$ then $$b_0 = 0$$**

### The Zero-State Response of a Systems

To determine the zero state response of a system, excluding the systems initial conditions we can calculate such a response by the following, given:

- $$x(t)$$, the input function
- $$y(t)$$, the output response
- $$h(t)$$, the impulse response

Then we can say that 

> $$ y(t) = x(t) * h(t) = \int\limits_{-\infty}^\infty x(\tau)h(t - \tau)d\tau $$


This is called the **convolution** of two functions. For more information, refer to the convolution section below.

### Classical Solutions to Differential Equations - Coefficient Matching

Recall that the total response of a system can be characterized by either (We just reviewed the zero state+input method above)

- Zero-State + Zero-Input
- Homogeneous(Natural) Response + Forced Response

In this section we'll review how to solve a system of equations based on the Homogeneous and forced response solutions using the method of coefficient matching. This method is typically the same method taught in introductory differential equations courses.


In this method we're given a differential equation of the form $$ Q(D)y(t) = P(D)x(t) $$.

The basic methodology for solving via the coefficient matching is as follows:

1. Solve for the characteristic polynomial and use that to find the characteristic modes. From construct the natural response equation
  - $$ y_n(t) = \sum\limits_{i=0}^N K_ie^{\lambda_i t} $$
2. After obtaining the form of the natural response with arbitrary coefficients $$K_0, \dots, K_N$$ use the following table to determine the the _forced response_ equation

| Input $$x(t)$$ | Forced Response |
| $$k$$ | $$\beta_0$$ |
| $$cos(\omega t + \theta)$$ | $$\beta cos(\omega t + \theta) $$|
| $$(t^n + a_{n-1}t^{n-1} + \dots + a_0)e^{\zeta t} $$ | $$(\beta_nt^n + \beta_{n-1}t^{n-1} + \dots + \beta_0)e^{\zeta t} $$

3\. Use the differentials from the characteristic polynomial  and differentiate the forced solution the required number of times to obtain a new set of equations with unkown coefficients which are added together on the left side of the equation.
4\. Use the differentials for the input $$x(t)$$ to create a new equation. This equation should have terms which match up in degree with or function with terms from the forced response. This goes on the right side of the original equation.
5\. Using all of the different degree terms from the polynomials, create a system of equations to solve for the unkown coefficients.
6\. Finally after solving for the coefficients, we can add the new forced response to our natural response with our unkown coefficients $$K_0, \dots, K_N$$. Use the given intial conditions to find the unknown coefficients with the natural+forced response system.

The resulting coefficient values will give the final solution which includes the natual + forced responses of the system which should be the same as the zero-state + zero-input response. 


### Chapter 2: Discretization Schemes and Convolution

Given the simple differential equation: $$ y'(t) = f(t) $$

The solution involves:

$$ y(t) = y(0) + \int_0^t f(t')dt' $$

So how can we solve this for any $$ f(t) $$?

One possible way to to refer to the definition of an integral - the area under a curve.

With this in mind we can apply some approximation techniques that we may have learned back in our calculus courses to estimate the value of $$ \int_0^t f(t')dt' $$


There are three methods which involve approximations:

- Forward Euler
  - $$ T \cdot f(t_{n-1}) $$
- Backward Euler
  - $$ T \cdot f(t_n) $$
- Trapezoidal (Binilear Approximation)
  - $$ T \cdot \frac{1}{2} (f_(t_n) + f(t_{n-1})) $$

In order to shorten our notation we'll use the following symbols:

- $$ y_n = y[n] \approx y(t_n) = y(nT) $$
- $$ f_n = f(t_n) = f(nT) $$

Working with linear systems we're able to add parts of systems together to find the solution to a whole system

Given:

> $$ y_n - y_{n-1} = pf_n + qf_{n-1} $$

If we set

$$ p = T, q = 0 \rightarrow $$ Backwards Euler

$$ p = 0, q = T \rightarrow $$ Forwards Euler

$$ p = q = \frac{1}{2}T \rightarrow $$ Trapezoid

Given the system

$$ y'(t) + ay(t) = B_0x'(t) + B_1x(t) $$


This can modified to

$$ y' - B_0x' = -aY + B_1x $$

This can then be expanded via our previous definitions

$$ (y_n - B_0x_n) - y_{n-1} - B_0x_{n-1} = p(-ay_n + B_1x_n) = q(-aY_{n-1} + B_1x_{n-1}) $$

If we then play around some more we an obtain:

$$ y_n = -ay_{n-1} + b_0x_n + b_1x_{n-1} $$

Using this it's now possible to solve this system because it's defined recursively (see how we define $$y_n$$ as being equal to an expression with $$y_{n-1}$$)

Sample:

$$ y_0 = -a_1y_{-1} + b_0x_0 + b_1x_{-1} $$
$$ y_1 = -a_1y_0 + b_0x_1 + b_1x_0 $$
$$ y_2 = -a_1y_1 + b_0x_2 + b_1x_1 $$


### Another Discretization Scheme

Matlab uses this method as the default. It's called **Zero-Order Hold**

It can be invoked with `c2d` or `c2dm`

Given an exponential differential equation:

$$ y(t) = y(0^-)e^{-at} + B_0x(t) + (B_1 - aB_0)a^{-at}\int_0^t e^{at'}x(t')dt' $$

If we want to write all of our $$t$$'s in terms of $$t_n$$ we can simple multiply through by $$e^{-aT}$$. This gives:

> $$ y(t_n) = y(0_-)e^{-at_n} + B_0x(t_n) + (B_1 - aB_0)e^{-at_n}\int_0^{t_n} e^{at'} x(t_n ')dt' $$


## Convolution

A linear time-invariant systems (LTI) satisfies the property that order in which an operation is performed (e.g time delay or transfer function) commute (the final value from the system is the same no matter the order).

_The same linear combination of inputs should result in the same output_.

A convolution is defined as:

> $$ (f * g)(t) \equiv \int_{-\infty}^\infty f(\tau)g(t-\tau)d\tau $$

> $$ y(t) = \int h(t')x(t-t')dt' $$

For

> $$ (a + c) \leq t \leq (b + d) $$

Convolutions are special in that they allow us to calculate the zero-state responses of a system. It is not always the simplest way, but it does provide one method of calculation.

Convolution has a few key properties to keep in mind:

- Commutative
- Distributive
- Associative
- Shift
  - Defined such that if $$y(t) = x(t) * h(t) $$ then $$ \rightarrow x(t-T) * h(t) = y(t-T) $$


### Chapter 4: Solving Differential Equations - Laplace method

There is yet another way to solve differential equations by using something called the **Lapalce Transform**

The laplace is defined as 

> $$ {\scr L}\{f(t)\} = \int\limits_{-\infty}^\infty f(t)e^{-st}dt $$


























