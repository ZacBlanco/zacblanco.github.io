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


See below for PDF notes on Bode plots and filters.

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







































