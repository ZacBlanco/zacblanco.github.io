---
layout: post
title: A Guide for Principles of EE I at Rutgers University
tags: [ engineering, rutgers, fall 2015, principles of electrical engineering, guide, class, study, circuits, computer engineering, electrical engineering, engineers  ]
permalink: blog/electrical-engineering/rutgers-principles-electrical-engineering-1
date: 3/9/2015
Author: Zac Blanco
---

## Introduction

Here we are - a new study guide - for Principles of Electrical Engineering 1 at Rutgers University in the Fall of 2015.

I will try to keep this as up to date as possible as the class goes on throughout the semester. Hopefully addressing all topics in the syllabus. The aim is for this to hopefully be able to serve as a study-guide by the time the end of the semester comes around. I hope that this guide will be able to cover all of the topics that are in this course, as well as provide examples of circuits and step-by-step solutions to different problems.

- Course Instructor: Prof. Hana Godrich

##### Topics:

- Voltage, Current, Resistance
- Ohm's Law, Kirkhoff's Law
- Tools for analyizing circuits
- Node-Voltage Method, Mesh-Current Method
-Thevenin and Norton Equivalents, maximum Power Transfer
- Operational amplifiers, inverting, not-inverting and difference amplifier circuits
- Properties of Inductances, etc.
- Review of Complex variables - Intro to sinusoidal steady state analysis, sinusoidal sources, Phasors
- Impedance, Admittance, reactance, susceptance, delta-wye simplifications, Phasor Diagrams
- Sinusoidal steady sate power calculates, RMS values, frequency selective circuits

### September 3rd 2015

##### Definition: Circuit

> An interconnection of components or elements that form a _closed path_ or many closed paths.

- For any element or component in a circuit, there are at least two terminals (i.e a connection) on each side of the element
- A node is an intersection of _two or more_ branches or connections off of an element. It can also be thought of as anything that is not part of a component or element.
- Circuits can take many different forms - but they can still perform the same function so long as all of the interconnections are the exact same.
- A _Path_ is a single "way" that you can walk through a circuit. Most circuits will have many, many, many paths. They must be continuous - no breaks in the 
- A _Branch_ is a path that moves form one node to another to another. It encompasses only a single element

#### Basic Circuit Components

- _Element or Component_: a two - or more - terminal device in a circuit with behavior described by _charactieristics_ which vary with time
- $$ V $$ or $$ v $$ - Voltage. Usually talked about voltage drop or gain over an element or component represented with  $$ + $$ or $$ - $$.
- $$ I $$ or  $$ i $$ - Current. Current has a direction chosen _almost_ arbitrarily by a person analyzing a circuit.
- Elements and components don't have to actually be a _part_ of the circuit. For example, we can think of a lightbulb as behaving similarly to a resistor. This means in a circuit diagram we can represent the lightbulb simply as a resistor.

#### More Definitions

- _Essential Node_ - A node where three or more circuit elements join
- _Essential Branch_ - A path that cnnects two essential nodes without passing through an essential node. Must include elements in series. (d-g)
- _Loop_ - a closed path that starts and ends at the same point.
- A set of independent loops - Each loop in the set contains at least one branch that is not part of any other loop in the set(a-b-c, b-e-g-d, b-c-f-h-g-d)
- _Mesh_ - a loop that does not enclose any other loop (d-b-e-g)
- _Path_ - A trace of connecting basic components or sources with no component or source included (a-b-e-h)
- _Node_ A point (junction) within a circuit where _two or more_ circuit elements join. (D)
- _A set of Indenpendent Loops_ - Each loop in the set contains at least one branch that is not part of any other loop in the set 

_!Important!_ A network with $$ b $$ _branches_, $$ n $$ _nodes_ and a total of $$ l $$ _indenedent loops_ will satisfy the following:

> $$ b = l + n - 1 $$

- Note! The above equation also works for the number of essential branches and essential nodes.


See the image below for an explanation
![Circuit and definitions slide](../../../assets/images/ee1-guide/definition-slide-1.png)


### September 6th 2015

#### Circuit Variables

- Charge: $$ q $$ or $$ Q $$
- Current: $$ i $$ or $$ I $$
- Voltage: $$ v $$ or $$ V $$
- Power: $$ p $$
- Energy (or work done): $$ w $$ 

#### Charge

- Charge is a property that almost all particles posess.
- _Like charges repel, and similar charges attract_.
- There is a minimum unit of charge is denoted by $$ q_e = 1.6 * 10^{-19} C $$
- Measured in Coulombs.
- Coloumb's law: $$ F = k\frac{q_1q_2}{r^2} $$

How does this help us???

Electrical current is actually defined as _the flow of positive charge_.

**It is very important to note that current flow is usually defined as the flow with POSITIVE charge.**

#### Electron Drift

- A conducting wire consists of atoms with loosely attached electrons. The electric force from Coulomb's law causes the loose electrons to jump from atom to atom inside the metal.

#### Electrical Current

>  $$ i(t) = \frac{dq(t)}{dt} $$

- The unit _Ampere_ is what we use to measure current - Also defined as _Coulomb's per second_.

#### Voltage or Electric Potential Difference

> $$ v = \frac{dw}{dq} $$

- The units for **voltage** is Joules per Coulomb, or $$ \frac{J}{C} $$
- Voltage is the energy per unit of charge created by the separation of positive and negative charges
- So we might also say that voltage is the amount of work required to get a certain amount of charge across two terminals.

#### Direction for Current and Voltage

![Image of basic circuit element with flow](../../../assets/images/ee1-guide/basic-element-flow.png)

- When creating circuits, it's very important to note the direction of current and where voltage drops occur.

** SECTION NOT FINISHED **


#### Power

> Power, $$ p $$ is defined as $$ \frac{dw}{dt} = iv $$

- Think about multiplying the units of Voltage and Current
- $$ \frac{Coulomb}{second} * \frac{Joules}{Coulomb} = \frac{Joules}{second} $$ which is the unit for Power.
- In an electric circuit, the total power generated or consumed by elements within the circuit must be equal to 0.
- This means that $$ p_{generated} = p_{consumed} $$.yse
- If power is supplied or generated by an element, another element will supplant/cancel out that element's power out

> Whenever the current flows in a direction from high potential to low potential (i.e. there is a voltage drop across the element), then the element is **consuming** the power or energy being delivered to it. (Positive Value

> Whenever the current flows in a direction from low potential to high potential (i.e. there is a voltage rise across the element), then the element is **generating** the power or energy (Negative Value)

- Low potential to high potential: $$ p = -iv $$
- High potential to low potential $$ p = iv $$

See below for more explanation

#### Passive Sign Convention

![Passive Sign Convention Example](../../../assets/images/ee1-guide/passive-sign-convention.png)


#### Finding the power consumed or delivered by an Element

If we have multiple elements in a circuit, we know the voltage drop/rise across each, and the current for all but one element, then we can find the power that is being delivered across the final element. We follow just a few simple steps

1. For each element, find if the power is consumed or delivered.
2. For each element calculate the power the is being consumed or delivered.
3. Add all of the power across each element up. Whatever the remaining difference is, the opposite sign (positive/negative) is the amount of power delivered across the last element.





