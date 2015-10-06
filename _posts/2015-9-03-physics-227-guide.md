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
 
 > $$ e = 1.6 \cdot 10^{-19} Coulombs $$
 
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

## September 10th 2015

### Lecture 3 Electric Field Flux and Gauss' Law

Quick review question:

> Imagine there is a negative charge, $$ -Q $$, in an electric field. It is released from rest. What direction will the particle's acceleration be?

- The particle will travel _opposite_ the direction of the electric field. Positive charges will flow with the field, negative flow in the opposite direction.

### Electric Flux

First, imagine water flowing through a pipe. How would you find the flow rate? Most people will probably want to use an equation similar to $$ \Delta{V} = A \cdot v \cdot 1s $$

Now what happens if we don't take a perfect cross section of the pipe though? How can we calculate the flow rate through this new cut? (Imagine the cut being at an angle to the edge of the pipe rather than perpendicular). Turns out we can still find the flow rate even though the cut is different.

All we have to do is simply take a dot product (or multiple through by $$ cos(\phi) $$).

- So the flow rate for a different (non-perpendicular cut) is $$ \Delta{V} =  \vec{A} \cdot \vec{v} \cdot 1s $$ or $$ \Delta{V} = Av cos(\phi) $$.

- Equation for flux

> $$ \Phi_a = \vec{a}(\vec{r}) \cdot d\vec{A} = a(r)A \cdot cos\phi $$

- Units of flux: $$ \frac{Nm^2}{C} $$
- Another Form: $$ \Phi_E = \vec{E} \cdot \vec{A} = \int\vec{E}(\vec{R}) \cdot d\vec{A} $$

### Point Charge at the Center of a Spherical Object

> At each point of a surface $$ \vec{E} * d\vec{A} $$ where $$( cos\phi=1 ) $$

Simplified, this means that the flux for a spehere is equal to $$ \frac{1}{4\pi\epsilon_0}\frac{q}{r^2}4\pi r^2 $$

This expression is equal to $$ \frac{q}{\epsilon_0} $$

- So what if we pose the question: What would be the flux of the gravitational field of Earth, through Earth's surface?

> Flux of Graviatational Field $$ = -4\pi G M $$

For fields that have $$ \frac{1}{r^2} $$ dependence, it is true that any field lines entering an object's surface must also exit. This means that the flix for any surface not contianing any charge, the flux is zero.

This also means _any surface deformations do not have an effect on the flux of the object_.

- Put simply this means that for any and all objects, the flux is dependent only upon the sum of the charge within the object,

From the lecture notes:

> For any closed surface, the flux of the electrostatic field would be proportional to the net charge inside the surface.

- Gauss' Law is also valid in electrodynamic, not just electrostatics because it is valid for **any** point inside of an object.
- Gauss' Law: First of Maxwell's Equations.

### Applications of Gauss' Law


- Gauss' Law in its integral form (one scalar equation) is not sufficient for finding three components of a vector field, **E**.
- However, Gauss' Law is very useful whenever it is possible to reduce a 3D (vector) problem to a 1D (scalar) problem.

Useful symmetries:
- Sphereical
Cylindrical + translational
- Plane

an example of _insufficient_ symmetry could be an electric dipole.

### Spherical Symmetry

Ex: The electric field of a uniformly charged ball. Radius $$ R $$, Total charge $$ Q $$.

- Gaussian surface: a sphere centered at the origin O. It's radius $$ r $$ can be smaller or larger than $$ R $$. 

![Slide with Spherical Symmetry](../../../assets/images/physics-227/spherical-symmetry-1.png)


## September 14th 2015

### Lecture 4 - Conductors in Electrostatics

The general problem of electrostatics is that given object carry many different charges - and we want to find out the electric field

Reminder: Gauss' Law

> $$ \int \vec{E}(\vec{r}) \cdot d\vec{A} = \frac{Q_{net}}{\epsilon_0} $$

Basically the law tells us that the integral over a sphere of an electric field (a vector) dot-multiplied by the differential area will give us the flux - which for any sphere can be simplified to $$ \Phi = \frac{Q_{net}}{\epsilon_0} $$


##### Matter in Electrosatic Fields

##### Conductors

- Electrons flow freely inside the metals - they will arrange themselves based on the field applied to the metal

- An electric field applied to a metal will cause the electric field inside the confuctor to become zero, because the electrons will rearrange themselves to form an arrangement where the total field ends up being zero.

Electric Field inside a metal

> $$ E = 0 $$

##### Dialectrics

- Electrons don't flow freely. But they will align or arrange themselves in an orientation based on the electric field applied to the dialectric.

Note:

#### iClicker Question Concept

- We cannot measure the electric field inside of a cynlinder using Gauss' Law due to the ends of the sphere requiring different integrals than the sides of the cylinder.



#### Comparison of Charged Metallic and Dialectric Spheres

In a uniformly charged dialectric spehere, the electric field inside (and only inside!) the sphere is equal to:

> $$ E = \frac{kQr}{R^3} $$ where $$r$$ is the radius of the sphere inside the dialectric and $$R$$ is the radius of the dialectric.

When $$ R = r $$:

> $$ E = k\frac{Q}{R^2} $$

And then when the gaussian surface has a radius _outside_ of the sphere, then the electric field is equal to: 

> $$ E = \frac{kQ}{r^2} $$

For a conducting spehere where the gaussian surface sphere is inside the conducting sphere:

> $$ E = 0 $$

For when the radius of the gaussian sphere is greater than the radius of the conducting sphere, the electric field is:

> $$ E = \frac{kQ}{r^2} $$

#### Faraday Cage

- A piece of metal with a hole in the middle. If the cage is conducting, then the electric field inside the cage is 0.

#### Cavities (No Charge within the cavity)

$$ Q_{net} = +Q = Q_{in} + Q_{out} $$

We can find the net surface charge is we place a charge inside a metallic shell.

Normally $$ E = 0 $$

then $$\Phi = 0 $$

So then if we put Q in a cavity inside the aphere then we know that the enclosed charge in the cavity will then attract charge to the inside surface of the cavity (within the conductor). This means the leftover charge will be equal to the charge felt on the outside of the sphere.

Thus, the charge enclosed in the cavity is equal to the charge on the outsie of the sphere.

## September 17th 2015

### Lecture 4 - Electrostatic Potential Energy

Note:

- Potentiential Energy $$ = U(r) $$ - measured in Joules
- Potential $$ V(r) $$ measured in volts
- $$ V(r) = \frac{U(r)}{q_0} $$


Coulomb's Law and the Superposition principle are usually sufficient to solve any electrostatic problem. Gauss' Law simplifies the alculation of $$ E $$ fields for symmetric charge distributions.

Say we want to calculate the velocity of two particles. Given the equation we have it would be very difficult because if the electrostatic forces push the particles away from each other, the acceleration changes. Because acceleration changes it would involve solving an integral to obtain the speed. This can be difficult.

So instead we can use potential energy to find the kinetic energy, and thus, speed.

- Remember that the force to move an object from point a to point b is going to be equal to:

$$ W_{a-b} = F_{a-b} \cdot (b - a) $$

_Integral Form_

$$ \Delta U(r) = W_{a-b} = \int_{a}^{b} \vec{F} \cdot d\vec{l} $$

**Reference Points** : A matter of convencience, only $$ \Delta U $$ matters (because only forces can be measured and he forces depend on $$ \Delta U $$ not $$ U $$)

$$ \vec{F}(f) = \frac{\partial U(r)}{\partial r} \hat{r} $$

##### Gravitational and *Electrostatic* fields are conservative

The reason? Both fields are **central** (a central force depends only on the distance between interacting objects and is directed along the line joining them.)

$$ \Delta U $$ depends only on the _initial_ and _final_ points of the trajectory.

- For any loop $$ \int \vec{F_e} \cdot d\vec{l} = 0 $$ .

Thus for our potential energy for any two electrostatic charges

#### Electrostatic Potential Energy

- $$ W_{\infty - r} = \frac{q_1q_2}{4\pi\epsilon_0 \cdot r} $$

This energy is conservative. Meaning that no matter the path a particle takes from point 1 to point 2 - The change in energy will still be the same!

##### From $$ U(r) $$ to $$ F(r) $$

> $$ \vec{F}(x) = -\frac{dU(x)}{dx}\hat{x} $$

### September 21st 2015

#### Electrostatic Potential

Volts! The electrostatic potential.

> $$ V(r) = \frac{U(r)}{q} $$

The electrostatic potential is related to the potential energy of charges in the electric field. Numberically, this is the work done by external forces to bring a positite unit charge from some reference point to the point in question.

- Units: $$ \frac{J}{C} = $$ Volt

#### Electric Potential to Electric Field

We know that $$ \vec{F}(x, y) = -\frac{\partial{U}(x, y)}{\partial x}\hat{x} -\frac{\partial{U}(x, y)}{\partial y}\hat{y} $$

Similarly the Electric field is equal to the derivative of the electrostatic potential.

That is 

- $$ \vec{F}(x, y) = q\vec{E}(x, y) $$
- $$ U(x, y) = qV(x, y) $$


#### Calculations of Electrostatic Potential

- $$ V(r) = k\Sigma_i\frac{q_i}{|\vec{r} - \vec{r_i}|} $$

##### Electric Field and Potential of a Charged Ring

$$ V(x) = \frac{1}{4\pi\epsilon_0}\frac{Q}{\sqrt{x^2 + a^2}} $$

#### Electric Potential Inside and Outside a Conductor

- Inside of a (spherical) conductor, the electric potential is equal to $$ k\frac{q}{R} $$ where $$R$$ is equal to the radius of the "sphere" conductor.
- Outside of a conductor, the electric potential decreases with distance and is equal to $$ k\frac{q}{r} $$. Where $$r$$ is the total distance from the center of the spherical conductor.

## September 24th 2015

###  Lecture 7 - Capacitors and Electric Field Energy

With a metal we know that the electric field must be perpendicular to the surface of the metal (while the inside of the metal has an electric field of zero).

But what about the electric potential distribution over the surface?

> The conducting surface is an equipotential surface

Because the E field is 0 everywhere inside a conductor, the electrostatic potential on the surface of a conductor must be equal to the potential everywhere on the surface.

> The E field in a conductor is always zero!

> The Electrostatic Potential at any point in the spehere is the same as the potential on the surface!


### Capacitors

- A cpacitor is a system of two conducting surfaces (electrodes), the net charge of the two are zero.

That is, one plate will be positively charged and the other will be negatively charged.

- _Important!_ Because the surfaces are conducting, they are equipotential and 

$$ \Delta V = \int^{-electrode}_{+electrode}\vec{E}\cdot d\vec{l} $$

Capacitance - Is the amount of charge per equipotential. $$ \frac{Coulombs}{Volts} $$

> Capacitance: $$ C = \frac{Q}{\Delta V} = \frac{Q}{\int^{-electrode}_{+electrode}\vec{E}\cdot d\vec{l}} $$

##### Capacitance Analogy

Bucket Of Water

- $$Q$$ = Amount of Water
- $$C$$ = Amount of water divided by volume of bucket
- $$\Delta V $$ = Height of water in bucket

Capacitance Does not depend on the "amount of charge" really. If we increase the charge on the capacitor, the voltage will also change, meaning the capcitance is constant.

### Computing Capacitance

Computing capacitance for a given $$Q$$: Calculate $$ \Delta V $$ and use the definition of capacitance.

- $$ E = \frac{\sigma}{\epsilon_0} = \frac{Q}{\epsilon_0A} $$
- $$ \Delta V = Ed = \frac{Qd}{\epsilon_0A} $$
- $$ C = \frac{\epsilon_0A}{d} $$ (For a parallel Plate capacitor).


### Energy Stored in a Capacitor

Final state for a charged capacitor --> E field - $$ Q = \frac{Q}{\epsilon_0A} $$


Work for a capacitor:

- $$ W = \frac{Q^2}{2C} $$
- $$ U = \frac{Q^2}{2C} = \frac{CV^2}{2} = \frac{QV}{2} $$

### Energy Density

Valid for almost any point! Very Useful 

- $$ u_E = \frac{1}{2}\epsilon_0E^2 $$ 

Total Energy of the electric field

$$ U_E = \int_{all space} \frac{1}{2}\epsilon_0E^2(r)d\tau $$


## September 28th 2015

### Two types of Capacitor Problems

There are two types of capcitor problems

One where a capacitor is connected to a voltage source
Here, the voltage is constant

- $$ V = constant $$

One where a voltage is not connected and the capacitor is charged.
In this case, the charge $$ Q $$ is constant.

- $$ Q = constant $$


A few extra equations:

- $$ C = \frac{\epsilon_0A}{d} $$
- $$ V = \frac{Q}{C} $$

### Parallel Connections of Capacitors

If capacitors are connected in parallel with one another, then we can replace them with an equivalent capacitor $$ C_{eq} $$ that is equal to

> $$ C_{eq} = C_1 + C_2 + ... + C_n $$

### Series Connections of Capacitors

If capacitors are connected in series (nodes that are directly connected to only one other capacitor), then we can combine them into a new capacitor with a capacitance of $$ C_{eq} $$ that is equal to:

> $$ C_{eq} = \frac{1}{\frac{1}{C_1} + \frac{1}{C_2} + ... + \frac{1}{C_n}} $$

### Linear Dialectrucs in an External Electric Field

The electric field inside of a capacitor is actually in the opposite direction. This means the electric around the capacitor will change slightly.

This means that we can place a different material inside of a capacitor, and increase the capacitance

The dialectric constant for a meterial can be measured by taking the E field inside and outside the capacitor.

> $$ K = \frac{E_0}{E_{in}} > 1 $$

> $$ \frac{\sigma_i}{\epsilon_0} = E_0 - E_{in} = E_0(1 - \frac{1}{K}) $$

### Energy in a Capacitor with a Dialectric

> $$ \frac{U_{filled}{U_{empty}} = \frac{1}{K} $$

> $$ U_{empty} = $$

> $$ U_{filled} = \frac{Q^2}{}$$

## October 1st 2015

### Exam 1 Review

In a dialectric, the particles inside will align themselves according to the electric field applied to it. 

The dialectric constant for a material is equal to the ratio of the E field inside the material divided by the E field on the outside. The ratio should always be equal to one another.

### Energy stored in a Dialectric Capacitor

$$ \frac{Q^2}{2C} $$
$$ \frac{1}{2}CV^2 $$

### Half Filled Dialelctric Capacitor

If the dialectric is in between the positive and negative plates (fully extending from one to the other), we can find the capacitance by adding the similar capacitances as if it was in parallel because the plates receive the same voltage.

If it is halfway in the opposite way, (only touching one plate), then we can assume they are in series and find the equivalent capacitance by adding the two capacitors as if they were in series.

Electric Field and Electric Potential

Remember $$ E(x) = -\frac{dV(x)}{dx} $$


## October 5th 2015

### Charging a capacitor

1. Electrons are driven by the electric field in the wires (potential difference)
2. How much time does it take ( $$t_{equal} $$) -- Will learn in 2 lectures
3. To keep current running, we need to maintain the potential difference along a wire.

#### Electrons in Metals: "Classical" Microscopic Picture" 

Electrons are in random motion, colliding with static and dynamic imperfections of the crystal lattice

The average distance between scattering events = **the mean free path**, _mfp_, $$l$$ depends on temperature, density of defects, ...). Typically  $$l$$ ~ 10 nm @ 300k  (still about 100 lattice periods).

Time intervals between _elastic_ (energy conserving) collisions is $$\tau_{el} = 10^{-14} $$

### E /= 0 : Electron Drift

Under a gentle "breeze" of the electric field, the electron cloud drifts slowly inside the wire.

but why is there a drift velocity? Why don't they have constant acceleration?

- Because tere are processes of energy dissipation (compared to air friction). The terminal velocity is reached when the rate of energy gained from the electric field becomes equal to the rate of energy loss.
- Drift velocity = terminal velocity

### Current

> $$ I = \frac{dQ}{dt} $$

Current is the charge carried by the current carriers through a wire cross section in a unit of time

- Units: Amperes $$ = \frac{1C}{1s} = 1A $$.


> $$ I = \frac{ne(v_d \cdot 1s \cdot A)}{1s}  = nev_dA$$

A is cross sectional area.

- Current Density : $$ j = \frac{I}{A} $$
Regardless of the nature of charge carriers, (both positive and negative carriers exist in different types of conductors), the current is defined as a __directional motion of positive charges__, it always flows from higher potential to lower potential.

Electrons flow from low potential to high potential flow in the opposite direction of current (towards the positive region of potential)

We usually assume that we have positive current carriers in a circuit.

### Current through two different wires

If we have two wires conntexted together with different cross-sectional areas, the only quantity that stays the same across the wires is the current. The E field, Drift velocty, and current density all change across the two different wires.

Another quantity that changes across the two different wires is the drift velocty. It will increase in the smaller wire.

### Ohm's Law and Resistivity

> $$ V = IR $$

> $$ I = \frac{E}{\rho}A $$

> $$ j = \frac{E}{\rho} $$

The brightness of a lightbulb will increase if you cool the temperature of the resistor of the bulb (using something such as liquid nitrogen).

Pickles can glow if you connect them to a ciruit.
























































