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

## September 3rd 2015

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


## September 6th 2015

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

- If we see the current directed into the positive terminal and out of the negative terminal, then we see a **_voltage drop_** occur.
- If we see current directed into the negative terminal and out of the positive terminal, then we see a **_voltage gain_**.

#### Power

> Power, $$ p $$ is defined as $$ \frac{dw}{dt} = iv $$

- Think about multiplying the units of Voltage and Current
- $$ \frac{Coulomb}{second} * \frac{Joules}{Coulomb} = \frac{Joules}{second} $$ which is the unit for Power.
- In an electric circuit, the total power generated or consumed by elements within the circuit must be equal to 0.
- This means that $$ p_{generated} = p_{consumed} $$
- If power is supplied or generated by an element, another element will supplant/cancel out that element's power out

> Whenever the current flows in a direction from high potential to low potential (i.e. there is a voltage drop across the element), then the element is **consuming** the power or energy being delivered to it. (Positive Value

> Whenever the current flows in a direction from low potential to high potential (i.e. there is a voltage rise across the element), then the element is **generating** the power or energy (Negative Value)

- Low potential to high potential: $$ p = -iv $$
- High potential to low potential $$ p = iv $$

One way to think of it - is that whichever terminal (positive or negative) the current is flowing into - that is the going to be the sign of the power equation.

Example:

- Current flows into the positive terminal of an element in the circuit --> $$ p = iv $$
- Current flows into the negative terminal of an element in the circuit --> $$ p = -iv $$.

See below for more explanation

#### Passive Sign Convention

![Passive Sign Convention Example](../../../assets/images/ee1-guide/passive-sign-convention.png)


#### Finding the power consumed or delivered by an Element

If we have multiple elements in a circuit, we know the voltage drop/rise across each, and the current for all but one element, then we can find the power that is being delivered across the final element. We follow just a few simple steps

1. For each element, find if the power is consumed or delivered.
2. For each element calculate the power the is being consumed or delivered.
3. Add all of the power across each element up. Whatever the remaining difference is, the opposite sign (positive/negative) is the amount of power delivered across the last element.

## September 10th 2015

### Unit 2 Circuit Components

Topics covered in this Unit:

- Inpedendent Source
- Depedent Source
- Passive Circuit Elements
- Resistance and Ohm's Law
- Kirchoff's Law
- Sources Interconnection
- Components Interconnection
- Circuit Modeling and Circuit Analysis

### Circuit Components Symbols: Ideal, Independent, Voltage Source

**Independent Voltage Source**: Generates a voltage drop across its terminals without relying on other voltages in the circuit.

![Voltage Source Component](../../../assets/images/ee1-guide/voltage-source-1.png)

- Independent sources are always marked with a circle

> An _ideal voltage source_ supplies the same voltage regardless of current.

**Independent Current Source**: Generates a current through a branch in a circuit without relying on other currents in the circuit.

![Current Source Component](../../../assets/images/ee1-guide/current-source-1.png)

> An _ideal current source_ supplies the same current regardless of voltage.

- It is very important to note here that the two sources we just introduced do _not_ effect the other properties of the circuit. i.e. the independent voltage source will not change anythign about the current, and the independent current source will not change anything about the voltage.

### DC (Direct Current) & AC (Alternating Current) Sources

There are two types current sources that are present in electric current. AC (Alternating current), and DC (direct current). In the images below you can see the difference in these two types of currents.

> **Direct Current**

![DC Current Image](../../../assets/images/ee1-guide/dc-current-1.png)

> **Alternating Current**

![AC Current Image](../../../assets/images/ee1-guide/ac-current-1.png)

- AC in the United States changes at a frequency at 60Hz. Many other countries in the world use 50Hz.
- AC is usually used to transmit power to power plants and so on.
- See chapter 9 for advantages and disadvantages of AC and DC current. 


### Additional Circuit Sources

**Dependent Sources** Generate a voltage whose value depends on the value of another voltage or current in the circuit. They are usually represented by a diamond.

> Voltage Controlled Voltage Source (VCVS): $$ v_s = \alpha v_x $$
> Current Controlled Voltage Source (CCVS): $$ v_s = ri_x $$

- Note: $$ \alpha $$ and $$ r $$ are constants
- $$ v_x $$ and $$ i_x $$ are a specific voltage and a specific current elsewhere in the circuit.


> Voltage Controlled Current Source (VCCS): $$ i_s = gv_x $$
> Current Controlled Current Source (CCCS): $$ v_s = \beta i_x $$

- Note: $$ g $$ and $$ \beta $$ are constants
- $$ v_x $$ and $$ i_x $$ are a specific voltage and a specific current elsewhere in the circuit.


### Interconnection of Sources 

#### Voltage Sources

> Voltage sources in parallel must be of the same value.

- A good analogy for voltages changes is that it is a change in elevation. Think of walking up a mountain. If you walk up a mountain 10 meters. Then you walk down 10 meters. You end up at the same elevation.
- Now think of walking up and down the mountain as the change in voltage level instead. Where elevation is the same as voltage change.

- Voltage sources of any type can be connected in series. It will step up (or down) the voltage whatever the direction.
- **Voltage sources in parallel must be of the same value**.

#### Current Sources

> Current sources connected in series must be of the same value.

- Current sources connected in parallel are okay. Current flow on the sources essential branch can be maintained without interference.


### Passive Circuit Components Symbols

> **Passive Circuit Component**: A device that cannot generate electric energy and does not require external power sources to operate.


There are three types of passive circuit components:

- Resistors

> Equation: $$ v_R = Ri_R $$

- Capacitor:

> Equation: $$ i_C = C\frac{dv_C}{dt} $$

- Inductor:

> Equation: $$ v_L = L\frac{di_L}{dt} $$

![Passive Circuit Component](../../../assets/images/ee1-guide/passive-circuit-comp-1.png)

### Resistance

> **Resistance**: The physical property of an element that impedes the flow of current (electron drift)

Represented by $$ R $$ and is defined by:

- $$ \rho = $$ resistivity in $$ ohm\cdot cm $$
- $$ l = $$ length of material in $$ cm $$
- $$ A = $$ cross section of the material in $$cm^2$$

> $$ R = \frac{\rho \cdot l}{A} $$

The unit of resistance is the _Ohm_ --> $$ \Omega $$


### Ohm's Law: Linear Model

> $$ v= i \cdot R $$ or $$ i = \frac{v}{R} $$

When the voltage is positive and current flows into the positive terminal, then the current **_drops_**.


The voltage, $$v$$ across and the current, $$i$$ through a resistance $$R$$ that flows down the potential hill 

> Power delivered to a resistor: $$ p = i^2R=\frac{v^2}{R} $$

Note! Power is always **_consumed_** by a resistor (_positive_).

Hold up! hold up! I just said that power consumed by a resistor is always positive. How is that possible? What if we switch the positive and negative terminals?

> Resistors don't actually have positive and negative terminals. You can just assume - no matter the orientation or current flow - that they will be consuming power.


## September 14 2015

### Unit 2, Lecture 2

#### Calculating Power From Ohm's Law

We know now that $$ p = iv $$, right? We also know that from Ohm's law, $$ v = iR $$. This can then give us a new equation to calculate power based on resistance.

- $$ p = iv = i \cdot (iR) = i^2R $$

Another form is:

- $$ p = iv = (\frac{v}{R}) \cdot v = \frac{v^2}{R} $$


#### Open and Short Circuit Symbols

- In a short circuit the Resistance through between two nodes is 0.
- In a closed circuit the resistance between two nodes is $$ \infty $$.

![Open and Short Ciruit Symbols](/assets/images/ee1-guide/open-short-circuit-symbols.png)


#### Kirchoff's Laws

Arguably some of the most important equations in solving circuits come from Kirchoff's laws. They help us gain information about a circuit to be able to obtain and solve for different component's quantities.

Let's start off with current, $$ i $$.

##### Kirchoff's Current Law

> The algebraic sum of all the currents at any node in a circuit equals zero.

What does this mean?

- We can pick any nodes in our circuit. A node should always have _at least_ two currents flowing into it. This means that those two (or more) currents flowing into that node **must** sum to equal 0.

- We **ADD** currents entering a node. (_Postive_)
- We **SUBTRACT** currents leaving a node. (_Negative_)

![KCL Example 1](/assets/images/ee1-guide/kcl-ex-1.png)










