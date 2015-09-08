---
layout: post
title: A (Somewhat) Comprehensive Guide to Physics 227 at Rutgers University
tags: [physics, rutgers, fall 2015, electromagnetism, magnet, 227, undergraduate, physics.rutgers.edu, electrostatics, dipoles ]
permalink: blog/physics/rutgers-physics-227-guide
date: 3/9/2015
Author: Zac Blanco
---

So here we are again - another semester of introductory physics at Rutgers University, New Brunswick. This is the Fall 2015 semester. This will be a kind-of continuation from the Physics IB guide from the previous spring 2015 semester.

The aim of this is post/page to provide a (somewhat) comprehensive guide to this class by periodically updating it throughout the semester. My plan is to make notes during lecture as well as provide practice problems and explanations.

Without furthur ado - Let's dive in!

## September 3rd, 2015

Almost everything in the universe can be described by just 4 (!) equations.

### Electric Charge

- _Electric Charge_ is a physical quantity that characterizes how charged object participate in electrostatic interactions

#### Electric Charge Properties
 
- It is a scalar value
- It is quantizes - meaning that a charge must be in integer multiples of the fundamental value of charge
 - This fundamental amount is the charge of an electron.
 - $$ e = 1.6 * 10 ^ -19 C $$
 - The net charge of any isolated system is _conserved_. i.e the net charge of the universe is zero
 
 
### Coulomb's Law

Coulomb's law describes the _forces_ that charged particles exert on each other due to...well...simply their charge.

The equation for Coulomb's law is the following:

> $$ F_{12} = k\frac{\left|q_1q_2\right|}{r^2} $$

Now let's quickly describe each quantity

 - $$ F_{12} $$ is the force that each particle exerts on one another. This means that _both particles experience the same force_!
 - $$ k $$ is a constant value expressed in this function. It's value is equal to $$ \frac{1}{4\pi\epsilon_0} $$
 - $$ q_1 $$ and $$ q_2 $$ are the charges of each particle measured in  _Coulomb's_ or the symbol $$ C $$
 - $$ r $$ is the distance between the two charges.
 
 
> $$ k = \frac{1}{4\pi\epsilon_0} = 9*10^9 \frac{Nm^2}{C^2} $$

> $$ \epsilon_0 = 9 * 10^{-12} \frac{C^2}{Nm^2} $$
 
### The Superposition Principle

 - This principle states that the _interaction between any two charges is unaffected by any other charges present_
 - This principle also tells us that the equations are _linear_. The solutions of multiple equations can be added together to form other solutions (as in the presence of multiple charges). 
 
 
### Electric Dipole

An _electric dipole_ is formed by two charges $$ +q $$ and  $$ -q $$ at the distance $$ d $$ apart.
 

And that wraps up section one! I hope to keep this as up to date as possible.
If you would like to contribute to this guide please submit a pull request on GitHub.


## September 6th 2015

### Lecture 2 - Electric Field and Electric Field Lines.

One great analogy for electrostatic fields is gravity. Think about it

Gravity is a force we feel due to mass. All around a massive object, the force due to gravity than another object would feel is always directed at the center of mass of the larger object no matter the position around the massive object.

Now think about a negatively charged object with no mass (and every other object is a positively charged object). Every positive object would undergo a force directed at the very center of the negatively charged object (so long as it is a point charge). It wouldn't matter the location of the positive test charges. The force is always pointing towards the same point on the negatively charged object.

#### Electric Field

- Units are in $$ \frac{N}{C} $$
- A force due to an electric field can be defined as $$ \vec{F} = q\vec{E} $$
- Another form: $$ \vec{E} = k\frac{q}{r^2} $$ 


It's very useful to know that due to the superposition principle (mentioned above) We can add vectors of electric fields to find the _net electric field_ due to multiple charges, simply by adding the fields due do different charges at a single point. To add fields from different charges together, you must be using the _same test-point for each field_.

#### Electric Field Lines

- The direction of the electric field vector is tangential to the field line (curve)
- The intensity of the electric field at a given point is proportional to the local density of the field lines.

Another way of stating it, is that at every point on a field line, the force experienced by a test charge is in a direction that is tangential to the field line.

More things to note:

> When drawing electric field lines, the lines should always point away from a positive charge, and either terminate at a negative charge, or at infinity.

- A weaker field will have less lines (less dense)
- A stronger field will have more lines (more dense)


- For a point charge: $$ E(r) \propto \frac{1}{r^2} $$
- The density of the lines is $$ \propto \frac{1}{r^2} $$
- The area of a sphere centered at the charge is $$ \propto r^2 $$


#### Electric Polarization

> Redistribution of charges within a neutral object by an external electric field. As a result, a neutral object acquires a dipole moment.

We call materials that have induced or built-in dipoles, **dialectrics**. For example **water**. Dialectrics are usually insulators.

If we apply an electric field to any of these dialectrics, they will polarize themselves. As is one side of the object will become more positive, and one more negative.

On the other hand, we have conductors that can polarize themselves because their electrons act as a "sea". The electrons flow freely inside the metal. So if you apply and electric field, the electrons will move in the directon of the electric field, leaving some parts more positively/negatively charged than others.







 
 


