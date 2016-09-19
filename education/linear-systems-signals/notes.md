Notes for Linear Systems and Signals Fall 2016

Professor: Sophocles Orfanidis


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

$$ (y_n - B_0x_n) - y_{n-1} - B_0x_{n-1} = p(-ay_n + B_1x_n) = q(-aY_{n-1} + B_1x_{n-1})

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

> The same linear combination of inputs should result in the same output.


























