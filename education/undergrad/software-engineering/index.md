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


### Lecture 4 - 1/27/2017

**Requirements Engineering**


The process of completing an engineering project should at a minimum always consists of:

- Requirements gathering
  - Helps the customer to define what is required, what is to be accomplished, how the system will fit into the needs of the business, and how the system will be used on a day-to-day basis
- Requirements analysis
  - Refining and modifying the gathered requirements.
- Requirements specification
  - Documenting the system requirements in a semi-formal or formal manner to ensure clarity, consistency, and completeness

  Typically the customer will describe the requirements of what needs to be done. The software engineer will also help specify the requirements. The software engineer however also will write the specifications which overlap with the programming. The specifications of a project are more formal and possibly more technical set of details that help the programmer identify the tasks which needs to be completed.

When writing requirements it is important to be specific and identify what exact requirements are going to be referenced throughout the document.

One way to do this is to label each requirement with an identifier, the priority of each requirement, and a short description of the requirements in as exact language as possible.

We should also think about _user stories_ when designing our requirements. This means focusing on the user of the system and how they will use the system.

I.e. "_As a tenant, I can unlock the doors to enter my apartment._"

This is similar to building system requirements, but focuses on the benefits that the user gains instead of on the features of the systems's solution to problems.

- This is the preferred tool in agile methods.

**Dynamic Prioritization of User Stories**

Define a number of user stories, i.e. ST-1, ST-2, ST-3, ST-4....

For each story assign an estimated number of days to complete and assign a number of points to each story to prioritize the feature development.

### Lecture 5 - 1/31/2017

**Hierarchical Organization of Software**

It is like a tree (List top of tree to bottom)

- Product Line
- System/Product
- Subsystem/Modules
- Packages
- Classes/Objects
- Methods

Software is not one long list of program statements but it has _structure_. The hierarchical organization above represents a taxonomy of structural parts to a software system.



**Why Decompose?**

- Tackle complexity by divide and conquer
- See if some parts already exist or could be reused
- Avoid reinventing the wheel
- Support for flexibility and future evolution by decoupling unrelated parts. So each can evolve separately.
- Create sustainable strategic advantage

**Software Architecture** - A set of high-level decisions that determine th structure of the solution

- Principal decisions made throughout the development and evolution of a software system.
- Made early and affect large parts of the system ("design philosophy")
  - Such decisions can be hard to modify later.
- Decisions to use well-known solutions that are proven to work for similar problems.
- Software architecture is not a _phase_ of software architecture.

Given the size of a system, a decision is architectural if it can be made only by considering the present scope of the system

- Could not be made from a narrowly-scoped, local perspective
- Architecture should focus on high impact and high priority areas.


**Concerns**

- How to break up the system
- Do we have all of the pieces
- Do the pieces fit together?
- Does the idea have conceptual integrity
- Software architecture produces a set of well-known solutions that are proven to work for similar problems.

**Architecture vs Design**

- Architecture focuses on non-functional requirements and decomposition of functional requirements
- Design focuses on implementing the functional requirements.

**Architectural Decisions Often Involve Compromise**

- The best design for a component considered in isolation may not be chosen when components are considered together or within a broader context.
- Additional considerings include business priorities, available resources, core competences, target customers, competitors moves, technology trends, existing investments, backwards compatibility, etc..

**Architectural Styles - Constituent Parts**

- Components - processing elements that "do the work"
- Connectors - enable communication among components
- Interfaces - connection points on components and connectors
- Configurations - Arrangements of components and connectors that form an architecture.


**MVC** - **M**odel **V**iew **C**ontroller

- Model: Holds all of the data, state, and application logic. it is oblivious to the view and controller. Provider API to retrieve state and send notifications of state changes to the observer
- View: The visualizer portion. The way to display the state of the system
- Controller - interpreter and processor of input redirecting to output.

**Fitting the Parts Together**

- Specifying semantics of components interfaces
  - Serves as a contract between the component providers and clients.
  - Interfaces must be fully documented, understandable, unambiguous, and precise.
- Adding Semantics
  - informal description
  - Design models
  - pre/post conditions

### Lecture 6 - 2/3/2017

**Use Cases**

Used for functional requirements analysis and specification

- A use case is a step-by-step description of how a user will use the system-to-be to accomplish the business goals
  - Detailed use cases are usually written as usage scenarios or scripts. These show an envisioned sequence of actions and interactions between the external actors and the system-to-be.

You can build use cases from the user stories that are written as a part of the system requirements.

**Types of Actors**

*Initiating Actor* - (Also called primary actor or simply "user"): Initiates the use case to achieve a goal.

*Participating Actor* (also called secondary actor): Participates in the use case but does not initiate it.  
- Subtypes of participating actors:
  - Supporting actor: Helps the system-to-be complete the usecase
  - Offstage actor: passively participates in the use case, i.e. neither initiates more helps complete the use case but may be notified for keeping records

Some times it is useful to draw diagrams of use cases in order to visualize exactly how the system operates between a system of multiple users in order to visually represent and follow the set of logic and steps that are required of the system in a specific use case.

You should be able to discern the basic function of a system from a single diagram which displays the different actions, use cases, and actions of the software system.


### Lecture 7 - 2/7/2017

**Domain Modeling**

Topics:  
- Identifying concepts
- Concept Attributes
- Concept Associations
- Contract: Preconditions and postconditions


*Why Domain Modeling?*

We model domains in order to understand how the software system will work

- Requirements analysis determined how users will interact with the system
- Domain modeling determines how elements of the system interacts to produce external behaviors.

*How?* - we do domain modeling based on sources

- Knowledge of how the system is supposed to behave
- Studying the work domain (or problem domain)
- Knowledge base of software designs
- Developer's past experience with software design.

**Building Domain Model From Us Cases**

1. Identify the boundary concepts.
  - Software concepts that interact with our actors of the system
2. Identify the internal concepts.

### Lecture 8 - 2/10/2017

**Object-Oriented Design**

- Assigning Responsibilities to Objects
- Design Principles
- Business Policies
- Class Diagrams

We create system sequence diagrams to see how objects interact with one another within the software system.

**Types of Object Responsibilities**

- **Knowing Responsibilities**: Memorizing data or references such as data values, data collection, or references to other objects represented as a property
- **Doing Responsibility**: Performing computations, such as data processing, control of physical devices, etc..; Represented by a method
- **Communicating Responsibility**: Communicating with other objects, represented as message sending (method invocation)

Given a use case you need to "connect the dots" between the objects in order to figure out how the system-to-be will function. Ask the questions

- Who will handle this data?
- Who performs any verification?
- Who signals, when do they signal?

*Characteristics of Good Design*

- Short communication chains
- Balanced workload
- Low degree of connectivity or associations

*Design Principles*

- **Expert Doer Principle**: That who knows should do the task
- **High cohesion principle**: Do not take on too many computation responsibilities
- **Low Coupling Principle**: Do not take on too many communication responsibilities

**Responsibility-Driven Design**

1. Identify the responsibilities
  - Domain modeling provides a starting point
  - Some will be missed at first and identified in subsequent iterations
2. For each responsibility identify the alternative assignments.
  - If the choice appears to be unique then move to the next responsibility.
3. Consider the merits and tradeoffs of each alternative by applying design principles.
  - Select what you consider the "optimal" choice.
4. Document the process by which you arrived to each responsibility assignment.


### Lecture 9 - 2/17/17

*Note*: Last lecture was cancelled.

**Software Testing**

- **Test Driven Development** (TDD): Write tests to drive the development of software artifacts.
- A **test case** is a particular choice of input data to be used in a testing program.
- A test is a finite collection of test cases

Testing is Hard

- No hard barrier on economic tradeoff between time spent writing tests vs time spent actually writing code
- Find faults cheaply and quickly
- In practice we have to run many unsuccessful test cases that do not expose any faults

**Accepteance Tests**

Example:

- *Test Description (Expected Result)*
- Test with valid key of a current tenant on his/her apartment (pass)
- Test with the valid key of a current tenant on someone else's apartment (fail)
- Test with an invalid key on any apartment (fail)

**Test Coverage**:

- **Test coverage** measures the degree to which the specification or code of a software program has been exercised by tests.
- **Code Coverage**: Measures the degree to which the source code of a program has been tested
  - *Criteria*
  - Equivalence testing
    - A black-box testing method that divides the space of all possible inputs into equivalence groups such that the program behaves the same on each group.
  - Boundary testing
    - A special case of equivalence testing that focuses on the boundary values of the input parameters. Based on the assumption that developers often overlook special cases at the boundary of equivalence classes
  - Control-flow testing
    - Statement coverage
    - Edge coverage
    - Condition Coverage
    - Path coverage
  - State-based testing
    - Defines a set of abstract states that a software unit can take and tests the unit's behavior by comparing its actual states to the expected states. (Popular with object-oriented systems)
    - *State* of an object defined as a constraint of the values of an object's attributes

**Practical Aspects of Unit Testing**

- Mock objects
  - A test driver simulates parts of the system that invokes operations on the tested components.
  - A test stub simulates the components that are called by the tested components

*Cycle*: 

1. Create the thing to be tested, the driver, and the stubs
2. Have the driver invoke an operation on the fixture
3. Evaluate that the actual state equals the expected state.

**Junit and Other Frameworks**

- Popular for testing
- Use `assertX()` methods to verify conditions

**Integration Testing**

- Horizontal integration testing
- Vertical integration testing










