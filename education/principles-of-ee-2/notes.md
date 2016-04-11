---
layout: page
title: A Guide for Principles of EE II at Rutgers University
keywords: engineering, rutgers, spring 2016, principles of electrical engineering 2, principles of electrical engineering II, guide, class, study, circuits, computer engineering, electrical engineering, engineers, peddapulla sannuti, sannuti
permalink: /education/principles-of-ee-2/notes/
mathjax: true
author: Zac Blanco
---

These notes are for the version of the course taught in the spring of 2016 by professor Pedapulla Sannuti

- [Homework Instructions](../HW-instructions.pdf)
- [Syllabus Spring 2016](../Syllabus-222-S2016.pdf)


## Lecture 1 - January 19th 2016

- See the [notes for lecture 1 here](../Review-of-basics-of-EEI.pdf)
- [Homework 1 (Due Jan 26th)](../HW-due-Jan-26.pdf)


We introduce a new notation we'll call the "Laplace Domain". It is similar to the phasor domain from PEE 1. It's very simple.

We introduce the variable $$s$$.

> $$ s = j\omega $$

We solve everything the same way as we previously did. But now we just replace every $$j\omega$$ with $$s$$.

It's that simple!

## Bode Plots and Filters

-------


**[How to Bode Plot - A Beginner's Guide](../how-to-bode-plot/)**


See below for professor's PDF notes on Bode plots and filters.

- [Notes on Bode Plots](../Bode-plots.pdf)
- [Notes on Filters](../Filters-Notes-S2016.pdf)

**Basic Introduction to Fourier Transforms of a Time Domain Signal**

- Fourier and Laplace revolutionized many fields by introducing the concept that **any signal can be thought of as being composed by a number (finite or infinite) of sinusoidal signal of different frequencies, amplitudes, and phase angles**.

That is the sinusoidal signals that compose a signal $$x(t)$$ are called the **frequency components** of $$x(t)$$. If a sinal has a countably finite or infinite number of frequency components, then

> $$ x(t) = \sum\limits_{k=1}^N X_kcos(\omega_kt + \theta_k) $$

But then what happens when we say that our signal $$x(t)$$ has an infinite number of components? We can turn it into an integral:

> $$ x(t) = \int\limits^{\omega_2}_{\omega_1} X(\omega)\cdot cos(\omega t + \theta(\omega))d\omega $$

We are limited here to say that the frequencies lie within the range $$\omega_1 \text{ to } \omega_2$$.

Let's not worry too much about the math right now.

In the purpose of transmitting signals over the air (think TV, radio, etc..) channels will typically occupy a certain **"band"** of signals.

For our purposes we'll just define a **band** as a **range of frequencies**.

Now these TV and radio **channels will typically be made of many different signals with frequencies that lie within a band** assigned to that channel.

For example the TV channel WABC occupies the band of 765 KHz to 775 KHz. This means that the channels transmissions will be made up of a signal of the frequencies in that range.

A filter itself does almost exactly what you think it would it would do with a signal. It will allow (or disallow) a certain range of frequencies from passing through it.

A somewhat more formal definition:

> A **filter** is basically a circuit that has a **transfer function** between its input and output. The transfer function lets certain frequency components of the input pass through to the output,  while it blocks all other frequency components.

Depending on what a filter allows and disallows through it can be classified with different names.


Right now we will focus on these 4 types of **ideal filters**

1. Low-Pass Filter
  - A filter which will only allow signals with frequencies that are **lower** than a specified $$\omega_0$$
2. High-Pass Filter
  - A filter which will only allow signals with frequencies that are **higher** than a specified $$\omega_0$$ through the filter
3. Band-Pass Filter
  - A filter which will only **allow a certain range (band)** of frequencies between a specified $$\omega_0$$ and $$\omega$$
4. Band-Rejection Filter
  - A filter which will **disallow a certain range (band)** of frequencies between a specified $$\omega_0$$ and $$\omega$$


Please make note of the word **ideal**. Because this means that the filter will definitely pick up (or block) the intended signal.

A non-ideal filter will pick up the intended signal, but possibly some non-intended signals as well. This results in **channel interference**.

Now imagine a normal RC-series circuit where we have a voltage source $$V_{in}$$ in series with a Resistor, R, and Capcitor of capacitance C

![img of RC circuit](../../../assets/images/pee-2/rc-circuit-1.png)

Now from this vircuit we can deduce that our transfer function is:

> $$ H(s) = \frac{V_0}{V_{in}} = \frac{\frac{1}{sC}}{R + \frac{1}{sC}} $$

Then we can convert this to the frequency domain

> $$ H(j\omega) = \frac{ 1 }{1 + j\omega RC} $$

This gives us our transfer function based on frequency. This is a ratio between the input and output signals. 

We find that based on different types of filters, these transfer functions will differ.

Now let's take a look at a plot of one of these transfer functions

![img of RC circuit](../../../assets/images/pee-2/low-pass-transfer-1.png)

Here we see 3 different bands of frequencies:

- Pass-band
- Transfer-band
- Rejection-band

When we have higher order (3rd, 4th, 5th) RC circuits, we'll see that the range of frequencies  in the transfer-band decreases.

The smaller the transfer band the **less cross-talk interference**.

## Filters - Terminology

Typically of a transfer function we can represent our transfer function in the phasor domain as

> $$ H(j\omega) = M(\omega) \angle \theta(\omega)$$

We should also note that:

- Power $$\propto \text{voltage}^2$$
- Power transmitted by input to output $$\propto M^2(\omega)$$

At a certain frequency, say $$\omega_m$$ has a maximum possible value of $$M(\omega_m)$$

Then from this we can also say that the **maximum power transferred is $$M^2(\omega_m)$$**

The **half power frequency** is the frequency at which the power transmitted by the input $$\implies M^2(\omega) = \frac{1}{2}M^2(\omega_m)$$

Or, equivalently using a decibel scale, the **half power frequency** $$= 20log_{10}(M(\omega_m)) - 3dB$$

- **Practical Low Pass Filters** if they are properly designed, will have only one _half-power frequency_ or cut-off frequency (denoted by $$\omega_0$$). Then, the **pass-band** of this filter ranges from 0 to $$\omega_c$$
- **Practical High-Pass Filter** For properly designed, ideal high pass filters there is only one cutoff frequency which is then starts at the cutoff frequency and continues to $$\infty$$. However, due to signal degredation and other factors, we can assume high-pass filters are just very large **band-pass filters**
- **Practical Band-Pass Filters** In there there are simply just two cutoff frequencies with a "center" frequency (not necessarily exactly between the two), called the **resonant** frequency.
- **Band-Reject Filters**: For the reject filters we find there are also two half-power frequencies which reject the frequencies between them.

In filters we also have a **time delay** which occurs due to the phase change of the filter (transfer function)

If our input is $$x(t)$$, then our output has a function of $$x(t - t_d)$$, where $$t_d$$ is the time delay.

We can see the exact relationship with phase angle $$\theta$$ by the following:

> $$ x(t-t_d) = Acos(\omega(t-t_d)) = Acos(\omega t - \omega t_d) = Acos(\omega t + \theta) $$

From this we can see that $$\theta = -\omega t_d$$

Or that $$t_d = -\frac{\theta}{\omega}$$


## Transfer Function of a First-Order LPF Op-Amp Circuit

![](/assets/images/pee-2/transfer-of-lpf-op-amp-1.png)


## Designing Filters

**Roll-Off Rate** At the edges of a passband filter, the rectangular shape is an idealized filter response. The slope is infinite. However, in actual circuits we find our transfer functions do not actually provide us with this ideal slope. 

The steeper we can make these slopes the better our filter will tend to be. The slope of the frequency magnitude esponse outside the passband it often called the **gain roll-ff rate**. The **higher the roll-off rate the better**

**Methodology for Filter Design**

First and second-order filters are often cascaded (connected end-to-end) to form what we call a **composite filter**. This means we need to analyze the common first and second-ordr filters.

**Passive filter** circuit (if properly designed) have the advantage of not requiring maintenance and are relatively cheap. However, when passive filters are cascaded, one filter circuit acts as a load to the previous filter. If the loading effect is dominant, the performance of the composite circuit will degenerate.

On the other hand **active filter circuits** do not present any loading problems since Op-amps have very low output impedance and high input impedance. However, the downside here is that active filters require maintenance and are prone to parasitic effects.

**Prototyping and Filter Design**

Typically filters will be designed with units in mind that make them easy to work with. The problem is that not all applications will use these numbers. They need to be **scaled** to their applications.

------------

**See below for a more detailed explanation on cascading**

![cascading filters](/assets/images/pee-2/cascading-filters.png)

------------

When we scale a single **active filter circuit** from one function to another. Usually we will want to scale the magitude and frequency up to certain values.

The new scaled frequency and phase will be equal to the original frequency and magnitude multiplied by a constant. We will call

- $$K_m$$ scaled magnitude constant
- $$K_f$$ scaled frequency constant

From this we can then determine how we should modify the filters components (capacitors, resistors, inductors) depending on the scaling we want

To derive these new component values for the magnitude scaling we will use the following equations

- $$R' = K_m\cdot R$$
- $$j\omega L' = j\omega L \cdot K_m $$
- $$\frac{1}{j\omega C'} = \frac{1}{j\omega \frac{C}{K_m}}$$

Then for frequency scaling we get

- $$R' = R$$
- $$j\omega K_f\cdot L' = j\omega L $$
- $$\frac{1}{j\omega K_f\cdot C'} = \frac{1}{j\omega C}$$

From these we get our scaling formulae

- $$R' = K_mR$$
- $$L' = \frac{K_mL}{K_f}$$
- $$C' = \frac{C}{K_mK_f}$$

### Effects of Scaling on a Transfer Function

> $$H(j\omega) = H'(jK_f\omega)$$

> $$H'(j\omega) = H(\frac{j\omega}{K_f})$$

> $$H'(s) = H(\frac{s}{k_f})$$


### Steady State Response of a Circuit

**Voltage over an Inductor**

> $$ V = L\frac{di}{dt} $$

**Current Through an Inductor**

If we use some differential equations to solve for the current through an inductor we get


> $$ i(t) = I_0e^{-(R/L)t}$$ where $$t \geq 0$$

**Current over a Capacitor**

> $$ I = C\frac{dv}{dt} $$

**Voltage over a Capacitor**

If we use some differential equations to solve for the voltage over a capacitor we get

> $$ v(t) = V_0e^{-(t/\tau)} $$ where $$\tau = RC$$ and $$t \geq 0$$

Now we need to use the above information to study the **Step Response** of a circuit

Step Response:

> The response of a circuit to the sudden application of a constant voltage or current source is referred to as the step response of the circuit.

**Step Response of an RL Circuit**

We find that in a basic series RL circuit the current through the circuit is:

> $$ i(t) = \frac{V_s}{R} + (I_0 - \frac{V_s}{R})e^{-(R/L)t} $$

**Step Response of an RC Circuit**

For a basic series RC circuit, the step response is equal to

> $$ V_c = I_s(R) + (V_0 - I_sR)e^{-t/RC} $$


## Intro to Laplace Transforms

A Laplace transform for a function $$f(t)$$ is denoted as:

> $$F(s) = \mathscr{L}\{f(t)\}$$

Where the laplace transform $$L\{f(t)\}$$is defined as:

> $$F(s) = \mathscr{L}\{f(t)\} = \int\limits_0^{\infty} f(t) e^{-st} dt$$

This is called a **unilateral** or **one-sided** laplace transform which extends from 0 to infinity. We will only be dealing with this type of transform.


### The Delta (Impulse) Function

A delta function is characterized as a function which at any point in time, $$t$$, that the amplitude is $$\infty$$, but the duration of the pulse at that point is equal to 0.

Mathematically the impulse function is defined as:

> $$ \int\limits_{-\infty}^{\infty} K\delta (t) dt = K $$

where $$\delta(t) = 0 \text{ for all } t \neq 0$$

In the real world, these electric circuits these types of electric signals don't actually exist, but they come very close, and this mathematical model is useful.

We should note that

> $$ \mathscr{L}\{\delta (t)\} = \int\limits_0^\infty \delta (t) e^{-st} dt = 1 $$


### Table of Useful Laplace Transforms


|Name| $$f(t)$$ | $$\mathscr{L}\{f(t)\} = F(s) $$ |
|Impulse|$$\delta(t)$$ | $$ 1 $$ | 
|Straight Line| $$1$$ | $$\frac{1}{s} $$ |
|Constant| $$k$$ | $$\frac{k}{s} $$ |
|Ramp| $$t$$ | $$\frac{1}{s^2}$$ |
|Exponential| $$e^{-at}$$| $$\frac{1}{s + a} $$|
|Sine| $$sin(\omega t)$$ | $$\frac{\omega}{s^2 + \omega^2} $$ |
|Cosine| $$cos(\omega t)$$ | $$\frac{s}{s^2 + \omega^2}$$|
|Damped Ramp|$$te^{-at}$$|$$\frac{1}{(s+a)^2}$$|
|Damped Sine|$$e^{-at}sin(\omega t)$$|$$\frac{\omega}{(s+a)^2 + \omega^2}$$|
|Damped Cosine|$$e^{-at}cos(\omega t)$$|$$\frac{s + a}{(s+a)^2 + \omega^2}$$|

### Operations on the Laplace Function


**Addition**

Given

- $$\mathscr{L}\{f_1(t)\} = F_1(s)$$
- $$\mathscr{L}\{f_2(t)\} = F_2(s)$$
- $$\mathscr{L}\{f_3(t)\} = F_3(s)$$

Then 

> $$\mathscr{L}\{f_1(t) + f_2(t) - f_3(t)\} = F_1(s) + F_2(s) - F_3(s)$$

**Differentiation**

Given a function $$f(t)$$ where $$\mathscr{L}\{f(t)\} = F(s)$$, then:

> $$ \mathscr{L}\{\frac{df(t)}{dt}\} = sF(s) - F(0) $$

Certain operations in the time domain correspond to multiplications, additions, and divisions in the Laplace domain

Given $$\mathscr{L}\{f(t)\} = F(s) $$

Then 

> $$\mathscr{L}\{\int\limits_0^t f(t)\} = \frac{F(s)}{s} $$

In other words, integration in the time domain of a function corresponds by simply dividing by $$s$$ in the Laplace transform of the function


Given $$\mathscr{L}\{f(t)\} = F(s) $$

Then 

> $$\mathscr{L}\{ e^{-at}f(t)\} = F(s + a) $$

### Inverse Transforms in the Laplace Domain

It is common to evaluate the functions for voltage or current of a into a form like:

> $$ F(s) = \frac{N(s)}{D(s)} =\frac{a_ns^n + a_{n-1}s^{n-1} + \dots + a_1s + a_0}{b_ms^m + b_{m-1}s^{m-1} + \dots + b_1s + b_0}$$

In this case, if the **degree of the polynomial of the denominator is greater**, than the numerator we consider it to be called a **proper rational function** if $$ m > n$$. Otherwise if $$ m \leq n $$ then it is an **improper rational function**

You need to know how to solve partial fractions. Use the links below in case you need a refresher

- [Click Here for a Google Search on Solving Partial Fractions](https://www.google.com/search?q=partial+fractions)
- [Partial Fractions on Khan Academy](https://www.khanacademy.org/math/precalculus/partial-fraction-expans/partial-fraction/v/partial-fraction-expansion-1)
- [Partial Fractions on Paul's Notes](http://tutorial.math.lamar.edu/Classes/CalcII/PartialFractions.aspx)

Now for an example on how an inverse transform from the Laplace domain. Imagine we're given the function:

> $$ F(s) = \frac{96(s+5)(s+12)}{s(s+8)(s+6)} $$

Because the denominator is in the form $$s(s+8)(s+6)$$ we can split this us using partial fractions to be

> $$ F(s) = \frac{K_1}{s} + \frac{K_2}{s+8} + \frac{K_3}{s+6} $$

Now to solve using partial fractions we need to find $$K_1, K_2, K_3$$. First we'll find $$K_1$$. To solve for $$K_1$$ we will multiply everything by $$s$$ and then solve for $$s=0$$

$$s = 0$$

- $$ \frac{96(s+5)(s+12)}{(s+8)(s+6)} = K_1 + \frac{sK_2}{s+8} + \frac{sK_3}{s+6} $$
- $$ \frac{96(0+5)(0+12)}{(0+8)(0+6)} = K_1 + \frac{(0)K_2}{s+8} + \frac{(0)K_3}{s+6} $$

This gives $$K_1 = 120$$

We follow the same procedure for $$K_2$$ and $$K_3$$ except that we will set $$s$$ equal to -8 and -6 respectively. This helps to cancel out the values from the other $$K$$ values.

We get from that that $$K_2 = 48$$ and $$K_3 = -72$$

So finally (after a bit of algebra) we can say

> $$ F(s) = \frac{120}{s} + \frac{48}{s+6} - \frac{72}{s+8} $$

Then we need to perform the inverse Laplace on this to find the original function

> $$ \mathscr{L}^{-1}\bigg\{ \frac{96(s+5)(s+12)}{(s+8)(s+6)} \bigg\} = \mathscr{L}^{-1}\bigg\{ \frac{120}{s} + \frac{48}{s+6} - \frac{72}{s+8} \bigg\} $$

> $$ \mathscr{L}^{-1}\bigg\{ \frac{120}{s} + \frac{48}{s+6} - \frac{72}{s+8} \bigg\} = u(t)(120 + 48e^{-6t} - 72e^{-8t}) $$


















































