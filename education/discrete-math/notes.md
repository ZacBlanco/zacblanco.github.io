---
layout: page
title: Discrete Mathematics (CS 205)
keywords: engineering, rutgers, spring 2016, computer, undergraduate, logic, math, discrete, science, cs205, 01:198:205, rutgers university
permalink: /education/discrete-math/notes/
mathjax: true
author: Zac Blanco
---

These are notes for the version of the class which is being taught by professor Minsky in the Spring of 2016.

**Class Information**

In this class we use the book _"Discrete Math"_ by Kenneth Rosen, 7th Edition. We cover chapters, 1, 2, 5, 9, and 13.

## Propositional Logic

At first we're going to study **propositional logic**. This is a type of logic which uses statements called **propositions** to convey truth (or falsity).

Examples of propositions:

> The sky is blue

> The internet connection is poor.

In these two statements we can clearly determine whether they are true or false. This is the key to propositions. They are **only** _true_ or _false_.

Typically we map True/False values to 1/0 respectively.

We can also create compound propositions! These are propositions which are made of two (or more) propositions by utilizing **binary logical operators**. There is also a **unary operator** which we will cover as well.

Below is a table of the basic binary operators which shows their basic meanings. We will go into more deatil shortly.

| And | Or | Implication | Exclusive Or |
| $$\land$$ | $$\lor$$ | $$\implies$$ | $$\oplus$$ |

**AND**: $$\land$$

This operator tells us that the entire should only be true **if and only if** the statements which are being operated on are both true. That is it can only be true if both propositions are true as well.

**OR**: $$\lor$$

This operator will convey truth if either one or both propositions are true. That is it can only be false if both of is propositions which it is comprised of are false.

**Implication**

Implication is a bit trickier, but what it tells us is that If one statement is true, then the next statement should be true as well. The order in implication matters, whereas in the **AND** and **OR** operators it does not.

So then for an implication statement to be false, the statements which comes first i.e. $$ p \implies q$$, $$p$$ would have to be true, and $$q$$ would have to be false for the entire statement $$p \implies q$$ to be false.

**Exclusive Or**: $$\oplus$$

Exclusive Or is similar to the regular Or, but in this case, we exclude the case which both statements are true. This means that in this statement, if both statements are true, then the result is false.

**Unary Operator**: NOT, $$\neg$$

This operator only requires a single proposition and will return the opposite value of a proposition. That is if we have a true statements, negated would be false. And then a false statemented _negated_ would be true.

### Sample Truth Table

|p|q| $$\neg p$$ | $$p\land q$$|$$p \lor q$$| $$p \implies q$$| $$p \oplus q$$| 
|-|-|------------|-------------|------------|-----------------|---------------|
|1|1|0|1|1|1|0|
|1|0|0|0|1|0|1|
|0|1|1|0|1|1|1|
|0|0|1|0|0|1|0|

The above truth table showcases the possible combinations of truth values for two different propositions whether they are true are false.


Now applying the same logic (ha, get it??) we can compound even more propositions together using these operators

One thing that might be interesting to note, is that if we want to create truth tables for multiple propositions (2, 3, 4, etc... different propositions) That the number of possible rows in our table increases exponentially as the number of propositions increases. 

This occurs because for every new proposition we add another possible combination of 0 or 1 to every already present combination

Simply put we'll call this the **counting principle**. In terms of truth tables:

> The total number of rows in a truth table is equal to $$2^n$$ where $$n$$ is the number of propositions we have.


Definition: **Tautology**

A **Tautology** is a propositional statement (compound or not) which will _always_ evaluate to true.

### Propositional Equivalencies

In propositional logic we find that it is actually possible to change the form of some propositions to equivalents which might make it easier to evaluate statements. 

Below is a table which highlights these equivalencies. Note that _T_ and _F_ represent **True** and **False**. They are not propositions

| Propositional Statement | Equivalent |
|:-----------------------:|:----------:|
| $$ p \lor T $$ | T |
| $$ p \land T $$ | $$p$$ |
| $$ p \land F $$ | F |
| $$ p \lor F $$ | $$p$$ |
| $$ p \lor p $$ | $$p$$ |
| $$ p \land p $$ | $$p$$ |
| $$ \neg(\neg p) $$ | $$p$$ |
| $$ p \lor (q \lor r) $$ | $$ (p \lor q) \lor r $$ |
| $$ p \implies q $$ | $$ \neg q \lor p $$ |
| $$ \neg q \implies \neg q $$ | $$ q \lor \neg p $$ |
| $$ q \lor p $$ | $$ q \lor p $$ |

























