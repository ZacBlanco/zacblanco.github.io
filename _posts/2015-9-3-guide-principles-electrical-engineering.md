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

Topics:
s
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

### September 3rd, 2015

##### Definition: Circuit

> An interconnection of components or elements that form a _closed path_ or many closed paths.

- Of any element or component in a circuit, there are at least two terminals (i.e a connection) on each side of the element
- A node is an intersection of _two or more_ branches or connections off of an element. It can also be thought of as anything that is not part of a component or element.
- Circuits can take many different forms - but they can still perform the same function so long as all of the interconnections are the exact same.
- A _Path_ is a single "way" that you can walk through a circuit. Most circuits will have many, many, many paths. They must be continuous - no breaks in the 
- A _Branch_ is a path that moves form one node to another to another. It encompasses only a single element

#### Basic Circuit Components

- _Element or Component_: a two - or more - terminal device in a circuit with behavior described by _charactieristics_ which vary with time
- $$ V $$ or $$ v $$ - Voltage. Usually talked about voltage drop or gain over an element or component represented with  $$ + $$ or $$ - $$.
- $$ I $$ or  $$ i $$ - Current. Current has a direction chosen _almost_ arbitrarily by a person analyzing a circuit.
- Elements and components don't have to actually be a _part_ of the circuit. For example, we can think of a lightbulb as behaving similarly to a resistor. This means in a circuit diagram we can represent the lightbulb as simply a resistor.

#### More Definitions

- _Essential Node_ - A node where three or more circuit elements join
- _Essential Branch_ - A path that cnnects two essential nodes withoutpassing through an essential node. Must include elements in series.
- _Loop_ - a closed path that starts and ends at the same point.
- A set of independent loops - Each loop in the set contains at least one branch that is not part of any other loop in the set(a-b-c, b-e-g-d, b-c-f-h-g-d)
- _Mesh_ - a loop that does not enclose any other loop (d-b-e-g)

_!Important!_ A network with $$ b $$ _branches_, $$ n $$ _nodes_ and a total of $$ l $$ _indenedent loops_ will satisfy the following:

> $$ b = l + n - 1 $$


See the image below for an explanation
![Circuit and definitions slide](../../../assets/images/ee1-guide/definition-slide-1.png)



