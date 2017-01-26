---
layout: page
title: Software Engineering
permalink: /education/software-engineering/
keywords: 14:332:452, rutgers, software, engineering, marsic, ivan, SE, git, version, control, team
description: A page full of notes and useful links for succeeding in Software Engineering at Rutgers University.
---

These notes are for a version of the class taught by professor Ivan Marsic in the spring of 2017.

[You can find the course materials here](http://ece.rutgers.edu/~marsic/Teaching/SE)

[Syllabus Here](http://eceweb1.rutgers.edu/~marsic/Teaching/SE/syllabus.html)

## Lecture 1 - 1/17/17 

Software engineering is the bridge from customer requirements to implementation.

**First law of software engineering**: Software engineers are willing to learn the problem domain. (or a problem cannot be solved without first gaining an understanding of it.)

Software engineers need to take customer problems or requirements and be able to analyze those requirements and turn that into a robust implementation. Good engineers will be able to combine their understanding of the requirements that a customer is asking for and use that and their background knowledge to come up with great implementations. They should be able to argue why their solution is the *best*.

There are three different types of people when creating software projects.

- **Customer**
  - Requires a computer system to achieve some business goals by a user interactions or interaction with the environment in a specified manner.
- **Software Engineer**
  - Required to understand how the system-to-be needs to interact with the user or the environment so that the customer's requimrent is met and *design* the software-to-be.
- **Programmer**
  - Goal is to implement the software-to-be which is designed by the sotware engineer.

Many times the software engineer and programmer's tasks will interweave with one another.

**Software Engineering Blueprints** - UML diagrams are like blueprints for a program. They allow us to visually represent our software

**Second Law of Software Engineering** - Software should be written for people first.

- Computers run software, but hardware quickly becomes outdated
- Useful & good software lives long.
- To nurture software people; must be able to understand it.


### Lecture 2 - 1/27/2017

[See my blog post on git](/blog/tips/intro-to-git/)

[Notes From Syllabus - Lecture 2](http://eceweb1.rutgers.edu/~marsic/Teaching/SE/syllabus.html)

### Lecture 3 - 1/24/2017

**Understanding the Problem and Dividing Work**

_Why we want to Subdivide Problems/Project_

- "...main value [of social skills] lies in the relationship between colleagues: people who can divide up task quickly nd effectively between them form more productive teams" - David Deming (Harvard U. & NBER)
- Common problem-solving strategy: divide and conquer
- Labor division within the developers' team
SUpport flexibility and future evolution by decoupling unrelated part, so each can evolve separately
  - Different team members can focus on different sub-problems.

**How Beginners Divide Work**

- List the components of the system-to-be
- Each subgroup of one or more team members is assigned different components to develop
  - One team member is assigned documentation
  - UI <--> Business Logic/Algo <--> Database/Network + documentation
- In this type of organization ever link is critical and if any fails the whole project fails
  - Separation of concerns is impossible.

**Decomposition vs Partitioning**

Projection-based decomposition helps us understand the components in the context of their use, relative to other parts of the system.

**Example: Restaurant Automation Decomposition by Projection**

- A better way to divide the project is by defining the subproblems of the project. Then assign each subgroup of the project team to each specific subproblem which may span the many areas of the project such database, networking, business logic, etc..

They are combined by the "shared infrastructure". After dividing sub-projects you can find the shared-infrastructure. You must be able to understand the whole project and the subproblem in order to identify the shared infrastructure.

Always ensure that your "mini-project" can be demonstrated standalone and achieves something interesting.

- Anticipate potential problems or integrations with other mini-projects.

Only after that can you ensure that other team members' contributions can be integrated and the whole project demonstrated for the greatest success.

We assume that software helps the user achieve a business goal in the problem domain.

- Problem domains can be virtual or physical
- Example problem types:
  - Transforming one virtual object to another (document format conversion)
  - Modifying virtual object (document editing)
  - Automatically controlling behavior of a physical object (ex. thermostat)
  - Manually controlling behavior of a physical object (drone flying)
  - Observing the behavior of a physical object (traffic monitoring)