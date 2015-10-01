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
![Circuit and definitions slide](/assets/images/ee1-guide/definition-slide-1.png)


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

![Image of basic circuit element with flow](/assets/images/ee1-guide/basic-element-flow.png)

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

![Passive Sign Convention Example](/assets/images/ee1-guide/passive-sign-convention.png)


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

![Voltage Source Component](/assets/images/ee1-guide/voltage-source-1.png)

- Independent sources are always marked with a circle

> An _ideal voltage source_ supplies the same voltage regardless of current.

**Independent Current Source**: Generates a current through a branch in a circuit without relying on other currents in the circuit.

![Current Source Component](/assets/images/ee1-guide/current-source-1.png)

> An _ideal current source_ supplies the same current regardless of voltage.

- It is very important to note here that the two sources we just introduced do _not_ effect the other properties of the circuit. i.e. the independent voltage source will not change anythign about the current, and the independent current source will not change anything about the voltage.

### DC (Direct Current) & AC (Alternating Current) Sources

There are two types current sources that are present in electric current. AC (Alternating current), and DC (direct current). In the images below you can see the difference in these two types of currents.

> **Direct Current**

![DC Current Image](/assets/images/ee1-guide/dc-current-1.png)

> **Alternating Current**

![AC Current Image](/assets/images/ee1-guide/ac-current-1.png)

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

![Passive Circuit Component](/assets/images/ee1-guide/passive-circuit-comp-1.png)

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

#### The Switch Symbol

![Switch Symbol](/assets/images/ee1-guide/switch-symbol-1.png)

- Switches can affect the direction the current will flow.
- The current will try to take the path of least resistace.

If a current can take a path through a circuit that eliminates flow through a resistor, then the current will not flow through to that part of the circuit (or resistor) at all.


#### Kirchoff's Laws

Arguably some of the most important equations in solving circuits come from Kirchoff's laws. They help us gain information about a circuit to be able to obtain and solve for different component's quantities.

Let's start off with current, $$ i $$.

##### Kirchoff's Current Law

> The algebraic sum of all the currents at any node in a circuit equals zero.

What does this mean?

- We can pick any nodes in our circuit. A node should always have _at least_ two currents flowing into it. This means that those two (or more) currents flowing into that node **must** sum to equal 0.

> $$ \Sigma_j i_j = 0 $$

- We **ADD** currents entering a node. (_Postive_)
- We **SUBTRACT** currents leaving a node. (_Negative_)

![KCL Example 1](/assets/images/ee1-guide/kcl-ex-1.png)


Now! We are going to see a similar law, but this time it will apply to voltage.

##### Kirchoff's Voltage Law

> The algebraic sum of all the voltages around any closed path in a circuit equals zero.


So what exactly does this mean? It means that we can pick _any_ closed loop in the circuit. When we follow that loop we must add (or subtract) the voltages over different elements together to give us a final sum of zero.

- If the element causes a _voltage drop_ (positive -> negative), then we **subtract**.
- If the element causes a _voltage rise_ (negative -> postitive), then we **add**.

> $$ \Sigma_j v_j = 0 $$

![KVL Example 1](/assets/images/ee1-guide/kvl-ex-1.png)


- Here's one more example problem you can use to test your calculation skills.

![Example Problem 1](/assets/images/ee1-guide/kirchoff-ex-prob-1.png)


Mastering these problems takes lots of practice - so it could be beneficial to find more problems online or in a textbook to practice calculating and solving for circuits.

#### September 21st 2015

How many KCL and KVL Independent Equations?

- KVL

> $$ b-n+1 $$ independent loop equations OR $$ b_e - n_e + 1 $$ Is the number of essential branches and $$n_e$$ is the number of essential nodes

- KCL

> $$ n - 1 $$ independent node equations OR $$ n_e - 1 $$ if every essential branch has a single current associated with it.

##### Steps to Solve for a Circuit

1. Mark all nodes, resistors, currents, and voltages including direction and polarity.

2. Choose Nodes for KCL

3. Choose Loops for KVL

4. Write KCL equations --> $$ n_e - 1 = 3 $$ 


### Unit 3 - Simple Resistive Circuits

In this unit we'll address creating circuits that contain resistors in series and parallel - and then finding methods for learning about how to simplify said circuits and then solve for them.

#### Resistors In Series

Resistors are an element within a circuit - and sometimes these elements are difficult to solve for indicidually. But what we can do is simplify these circuits with multiple of similar elements to make solving these circuits easier.


Resistors in series mean that the resistors are in a "line" with each other. There are no essential nodes between the two resistors. In this case (where there are no essential nodes) we can simply add the resistances of each resistor together

> $$ R_{eq} = R_1 + R_2 $$

From here on our circuit diagrams we can simply just combine the two resistors into one, where the single resistor now has the resistance of $$ R_{eq} $$.


## September 24th 2015

### Simplifying Circuits with Resistors in Series and Parallel

#### Resistors in Series 

We can simplify resistors that share a single essential node. We know that they are in series if they share an essential node and there are no other elements in between them. We can simply add their resistances.

So for resistors in series

> $$ R_{eq} = R_1 + R_2 $$

![Resistors in Series](/assets/images/ee1-guide/res-series-1.png)

Some notes about simplifying resistors in series

- The current through both resistors is the same. You can prove it using KCL.
- The voltage drop is related to the addition of the voltage drop across each individual resistor.


#### Resistors In Parallel

Resistors in parallel is where two resistors share a node with another branch from an element. To find the equivalent resistance we must use the reciprocal of the resistances. 

So for resistors in parallel:

> $$ \frac{1}{R_{eq}} = \frac{1}{R_1} + \frac{1}{R_2} $$

Therefore

> $$ R_{eq} = \frac{R_1R_2}{R_1 + R_2} $$

![Resistors in Parallel](/assets/images/ee1-guide/res-parallel-1.png)

The equivalent resitance of resistors in parallel will ALWAYS be smaller than the minimum resistance in the set of parallel resistors.

### The Delta-Wye Connection

In the triangular (Delta) configuration - We name the resistors in a very systematic way.

- For the branch between node a and b, the resistor must be $$ R_c $$.
- For the branch connecting nodes b and c, the resistor must be $$ R_a $$.
- For the branch ocnnection nodes a and c, the resistfor must be $$ R_b $$.

In the Wye (Y) configuration

- The (a) node should be connected to $$ R_1 $$.
- The (b) node should be connected to $$ R_2 $$.
- The (c) node should be connected to $$ R_3 $$.

![Resistors in Parallel](/assets/images/ee1-guide/delta-wye-equivalents-1.png)

This transformation to and from the Delta and Wye (Y) configurations can be useful in analyzing more complex circuits.


## September 28th 2015

#### Voltage Divider Circuit

So the first thing we need to know is that if we want to measure the voltage over a resistor, then we need to put the voltmeter in parallel with the resistor.

Now in a voltage divider circuit, we take that principle and expand on it to divide the voltage by a certain amount by putting multiple resistors in series with one another.

The voltage divider contains a voltage source of value $$ V_x $$ and resistors $$ R_1 $$ and $$ R_2 $$ in series with one another and the voltage source.

We know that from KVL the equation for the voltage must be that 

> $$ V_x = V_1 + V_2 $$

From KCL we can find that

> $$ i_x = i_1 = i_2 $$

and now from KCL and Ohm's we have

> $$ i_x = \frac{V_1}{R_1} = \frac{V_2}{R_2} $$
 
we want to now find $$V_1$$ in terms of $$V_x$$.

- $$ V_1 = f(V_x) $$
- $$ V_2 = f(V_x) $$

We can then find that $$ V_x = V_1 + \frac{R_2}{R_1}V_1 = (1 + \frac{R_2}{R_1})V_1 $$

So then solving for $$ V_1 $$ we find:

> $$ V_1 = \frac{R_1}{R_1 +  R_2}V_x $$

Similarly, if we solve for $$ V_2 $$, then 

> $$ V_2 = \frac{R_2}{R_1 + R_2}V_x $$

#### Current Divider Circuits

In a current divider circuit we can divide a voltage source's current by an amount by placing resistors in parallel and putting an ammeter in series with one of the resistors.

So if we have two resistors in series, then through KVL we have:

> $$ V_x = V_1 = V_2 $$

and through KCL we have that

> $$ i_x = i_1 + i_2 $$

And with Ohm's law included, we can get the equation

> $$ i_1 = \frac{R_2}{R_1 + R_2} i_x $$

So then if we solve for $$ i_2 $$ we find that

> $$ i_2 = \frac{}{}i_x $$


## October 1st 2015

### The Wheatstone Bridge

In a Wheatstone bridge, the $$R_x$$, (resistor with unkown resistance) is equal to:

- $$ R_x = \frac{R_2R_3}{R_1} $$. Where $$R_2$$ and $$R_3$$ are the resistors that shore a branch with $$R_x$$.

## Unit 4a Node-Voltage, Mesh-Currnet, & Source Transformation

### The Node-Voltage Method

In the node voltage method, you must pick a node as a reference node. At that node we say that it is $$ V_g  = 0 V$$. From here we can create equations that represent the voltage drops over multiple elements.

You can add the voltage drops over multiple elements to remove the amount of variables involved in the cicuit. From here we can represent each element with a difference between two different node voltages.

We can now convert some of these node voltages into current equations. The equation for current between two nodes with a resistance R is equal to:

> $$ i = \frac{V_b - V_a}{R} $$

Note that the value $$V_a$$ is the value of the node connected to the resistor on one end, while $$V_b$$ is equal to the voltage of the node at the other. $$V_b - V_a$$ represents the voltage drop across the resistor.


The steps for Vode-Voltage Analysis:

1. Mark all nodes, node voltages, and choose a reference (ground) node.
2. Write the node voltage equations
3. Calculate node voltages, elements, and current voltages.




















