---
layout: page
title: CSE 291 - Program Synthesis
keywords: CSE, 232, UCSD, UC, San Diego, software, PL, programming, language, sketch, synthesis, POPL
description: Graduate Course on Program Synthesis by Nadia Polikarpova at UC San Diego
mathjax: true
notes:
  augmented-examples: Augmented Examples
  blinkfill: BlinkFill
  brahma: Brahma
  euphony: Euphony
  eusolver: EUSolver
  nope: Nope
  sketch: Sketch
  suslik: SusLik
  synquid: Synquid
---


## Paper Notes

{%for pg in page.notes -%}
- [{{ pg[1] }}](./readings/{{pg[0]}}.html)
{% endfor %}

### Lecture 1 - Intro

- Understand what program synthesis can do and how
- Use existing synthesis tools
- contribute to synthesis techniques and tools towards a publication in an academic conference

### Intro to Program Synthesis

- goal: automate programming

- examples:
    - MS Excel FlashFill
    - simpler problem: Isolate the least significant zero bit in a word
- other ideas:
    - define program synthesis goal as a type:
        - the type defines constraints on the output result.

Overall program synthesis comes from a high-level specification, and turns it into a program
    - synthesis is different from compilation because there is a search space to find the right program.


- Dimensions of program synthesis
    - search strategy
        - how does the system find the right strategy?
    - behavioral constraints (specification)
        - how to tell the system what the program should do
    - structural constraints
        - what is the space of programs to explore?

- Behavioral constraints
    - how to tell the system what the program should do?
        - what is the input language/format?
        - what is the interaction model?
        - what happens when intent is ambiguous?
    - examples of behavioral constraints
        - input/output examples
        - equivalent program
        - format specifications
        - natural languages
- Structural constraints
    - what is the space of programs to explore?
        - large enough to contain interesting programs, yet small enough to exclude garbage/search efficiently
        - built-in or user defined?
        - can domain knowledge be extracted from existing code?
    - structure constraint examples:
        - built-in DSL
        - user-defined DSL
        - user-provided components
        - languages with synthesis constructs (e.g. generators in sketch)
- Search strategies
    - synthesis in search:
        - find a program in space defined by structural constraints that satisfies the behavioral constraints
    - challenge: space is astronomically large
    - how does the system find the program you want?
        - how does it know it's the program you want?
        - how can it leverage structural constraints to guide the search?
        - how can it leverage behavioral constraints to guide the search
    - examples:
        - enumerative search: exhaustively enumerate all programs in the language in the order of increasing size
        - stochastic search: random exploration of search space guided by a fitness function
        - representation-based search: use a data structure to represent a large set of programs
        - constraint-based search: translate to constraints and use a solver

Course structure

- Module 1: Synthesis of Simple Programs
    - easy to decide when a program is correct
    - challenge: search in large space
- Module 2: Synthesis of Complex Programs
    - decide when a program is correct can be hard
    - search in a large space -- still a problem
- Module 3: Advanced Topics
    - quantitative synthesis, human aspects, applications

## Lecture 2

### Synthesis from Examples

- Synthesis from Examples
    - also:
        - programming by example
        - inductive synthesis
        - inductive programming

- The "Zendo Game"
    - teacher makes up a "rule", gives two examples, one passing, one not
    - students attempt to guess the rule
- 1980s/90s not much progress in learning by example (LBE)


- key issues in inductive learning
    1. how to find a program that matches observations
    2. how to know if it is the program you are looking for?
- Traditional ML emphasizes problem (2) - knowing if it is the program
  you are looking for
    - fixes the space so that (1) is easy
- modern emphasis
    - if you can do really well with (1) you can "win"
    - (2) is still important
    - decrease the search space as much as possible


- key idea: parameterize the search by structural constraints - make the
  program space domain-specific

### Syntax-Guided Synthesis

- context-free grammars

```
L ::= sort(L) |
      L[N..N] |
      L + L   |
      [N]     |
      x
N ::= find(L, N) |
      0
```

- Grammar composed of
    - terminals,
    - nonterminals
    - rules (productions)
    - starting nonterminal
- $$&lt;T, N, R, S&gt;$$
- Sentential forms $${\alpha \in (N\cup T)^*}$$
- Rewrites to : $$\alphaA\Beta \rightarrow \alpha \gamma\Beta == (A \rightarrow \gamma \in R$$
- (Incomplete) terms/programs: $${\alpha \in (N\cup T)^* | A\rightarrow^* \alpha}$$
- Ground programs
    - programs without holes (complete)
- Whole programs
    - roughly, programs of the right type


- context-free grammer (CFG) define the space of programs which represent all
  **ground** and **whole** programs


- How big is the space of problems for a simple CFG?

CFG
```
E ::= x | E @ E
```

For a given syntax tree of depth $$N$$, the number of programs that can be
generated for a given depth is $$N(d-1)^2 + 1$$

Size explodes as complexity of program increases. For more complex grammars it
is even larger

### Enumerative Search

- Idea: Sample programs from the grammar one by one, and test them
  against examples
- Challenge: how to systematically enumerate all programs?

- techniques:
    - bottom-up enumeration
    - top-down enumeration


- Bottom-up enumeration
    - start from nullary production (variables and constant)
    - combine sub-programs into larger programs using productions

Given a grammar: $$(\lt T, N, R, S\gt, [i \rightarrow o])$$

```
bank := [t | A ::= t in R]
while (true)
 for all (p in bank)
    if (p([i]) = [o])
        return p
  bank += grow(bank)

grow(bank) {
    bank' := []
    forall (A ::= rhs in R)
        bank' += [rhs[B -> p] | p in bank, B -> *p]
    return bank
}
```


## Lecture 3

### Enumerative Search -- Cont'd

- The top-down algorithm (could be searched breadth/depth first) from root
of tree down to further depths

```
def topDown(<T, N, R, S>, [i --> o]) {
    wl := [S]
    while (wl != []) {
        p := wl.dequeue();
        if (ground(p) && p([i]) = [o]) {
            return p;
        }
        wl.enqueue(unroll(p));
    }
}

def unroll(p) {
    wl' := []
    A := left-most non-terminal in p
    forall (A ::= rhs in R) {
        wl' += p[A --> rhs]
    }
    return wl'
}

```



- bottom up:
    - candidates may be ground, but might not be whole programs
        - can always run on inputs
        - may not be able to relate to outputs
- top-down:
    - candidates are whole, but might not be ground
        - cannot always run on inputs
        - can always relate to outputs


### Search Space Pruning

- When can we discard a subprogram?
    - it's equivalent to something we already explored:
        - e.g. `sort(x)` vs `sort(sort(x))`
        - (equivalence reduction/symmetry breaking)
    - no matter what it is combined with, it will not satisfy the
      specification


- how to do equivalence reduction with an expensive "equiv" check?
    - expensive for SyGuS problems
    - expensive checks on every candidate defeats purpose of space pruning
      checks
- observational equivalence
    - in PBE we only care about equivalence on given inputs
        - easy to check efficiently
        - many more programs equivalent on particular inputs than on the
          entire input space
- term-re-writing system (TRS)
    - transform one term into another form of a term
    - issues with deciding which terms should be included, or when to use
        - e.g. commutativity rules could result in an infinite re-writing of a
          term - leading to an infinite loop
- just don't enumerate over non-normal form expressions - remove as soon as an
  equivalence is identified


- Built-in equivalences
    - for a set of ops - equivalence reduction can be hard-coded into the tool
      _or_ directly into the grammar
        - can modify the grammar so that equivalences are not generated

- Equivalence reduction:
    - Observational:
        - Pros:
            - very general, no additional user input
            - finds more equivalences
        - Cons:
            - can be costly (many large examples, large outputs)
            - new samples added, search must be restarted
    - user-specified
        - Pros:
            - fast
        - Cons:
            - requires equations
    - built-in:
        - Pros:
            - even faster
        - Cons:
            - restricted to built-in operators
            - only certain symmetries can be eliminated by modifying the grammar

## Lecture 4

- when to discard subprogram?
    - equivalent to something already explored
    - the program doesn't fit the spec


- _idea_: once we pick a production, infer specification for subprograms


- property of `cons` (list construction operator) that allows decomposition?
    - **injective**: for every input, there is exactly one output

- property of an operator that makes it good for top down enumeration?
    - works when a function is "sufficiently injective"
        - output examples have a small pre-image

- Conditional Abduction
    - form of top-down propagation

### eusolver discussion:

- constraints:
    - behavioral
        - linear arithmetic, first order logical formulas
            - f(x, y) >= x && f(x, y) >= y && ...
    - structural
        - conditional expression grammar
    - search strategy
        - enumerative search strategy

- specification needs to be pointwise
    - what is pointwise vs non pointwise?
        - f(x) > f(x+1)
    - how would it break the enumerative solver?
        - CEGIS (counterexample guided inductive synthesis)
        - if the spec is not monotonically increasing/decreasing, CEGIS
          would break

- What are pruning decomp techniques EUsolver uses to speed up search
    - condition abduction + special form of equivalence reduction
    - why generate additional terms when all inputs are covered
        - the predicates might not cover all the terms in a manner which solves
          the problem

- naive alternative to decision tree learning for synthesizing branch
  conditions?
  - learn atomic predicates that precisely classify points
    - why worse? -
    - as bad as Esolver? - enumerate over many possible conditions not
      much better
  - next best is decision tree learning w/o heuristics
    - why is this worse? more space used by decision tree

## Lecture 5

- Order of search:
    - enumerative search explores programs by depth/size
        - good default bias: small solution is likely to generalize?
        - result?
            - scales poorly with the size of the smallest solution to a given
              spec
            - if spec is insufficient: plays monkeys paw

- biasing search:
    - idea: explore programs in order of likelihood rather than size
        - q1: how do we know which programs are likely
            - learn some kind of statistical/probabilistic models
        - q2: how do we use that information to guide search
- statistical language model:
    - originated in NLP
    - in general, a probability distribution over sentences in a language
        - $$P(s) \for s \in L$$
        - what are the natural programs (sentences) in a DSL?
    - kinds of corpora:
        - all programs from DSL: what is natural in DSL?
        - solutions to specific tasks
        - spec-program pairs: what are the likely programs?
    - kinds of ____:
    - example:
        - Code completion with statistical language models: SLANG
            - predicts completions for a sequence of API calls
            - treats programs as a set of abstract histories
            - training learns bigrams, n-grams, RNNs on histories
                - inference: given a history with holes
                    - bigrams to get some possibilities
                    - n-grams/RNN to rank them
                    - combined history completions into a coherent program
                - features: fast
                - limitations: all invocation pairs must appear in the training
                  set
        - neural corrector for MOOCs
            - input: incorrect program + test suite
            - output: correct program
            - treats programs as sequence of tokens, abstracting variable names
            - uses skipgram model to predict which statement is most likely to
              occur between the begin and end statement
            - features: repair syntax errors; limitations: needs all
              algorithmically distinct solutions to appear in the training set
        - DeepCoder:
            - algorithm with a neural net that given input/output examples
              attempts to guess which operations and operators are the most
              likely to be in the part of the grammar.
            - features:
                - trains on synthetic data:
                - can be easily combined with any enumerative search
                - significant speedsups for small list DSL
- weighted top-down search
    - idea: explore programs in the order of likelihood, not size
    - assign weights to edges such that if one distance from the root is less,
      the program is more likely.
        - can search with dijkstra's (or A*)


## Lecture 6

- PCFG doesn't provide enough information to synthesize
    - instead, use a PHOG which represents more information than a PCFG.
        - contains a context object
- synthesize a program that describes the program
    - start with CFG
    - synthesize into AST
    - learn a "context" and probability for the grammar
- PHOGs supposed to be good for:
    - code completion, deobfuscation, PL translation, statistical bug detection
- Euphony contraints:
    - structural: input/output examples
    - behavioral: PHOGs
    - search strategy: top-down enumerative w/ A*
- euphony q2: what would productions of `Rep(x,"-", S)` look like if we use a PCFG, 3-gram? `| S --> "-" = 0.2 | S --> "." = 0.3 | ...`
- equivalence vs weak equivalence
    - equivalence: no matter how holes are filled, the programs are equivalent
    - weak equivalence: subsequences of sentential forms are equivalent
      on some points
- paper contributions
    - efficient way to guide search by probabilistic grammar
    - transfer learning for PHOGs
    - extend observational equivalence to top-down search
- weaknesses
    - requires high-quality training data
    - transfer learning requires manually designed features

## Lecture 7

- representation-based + stochastic search
- representation-based search
    - idea:
        - build a data structure that represent a large part of the search space
        - search within the data structure
    - useful when
        - need to return multiple results/rank results
        - can pre-process search space / use for multiple queries
    - tradeoff: easy to build vs easy to search
- representations
    - version space algegra (VSA)
    - Finite Tree Automaton (FTA)
    - Type Transition Net (TTN)
- VSA
    - build a data structure succinctly representing the set of programs
      consistent with examples
    - operations on version spaces:
        - `learn <i, o> -> VS`
        - `VS1 \intersect VS2 -> VS`
        - `pick VS -> program`
    - version space is a DAG
        - leaves: set of programs
        - union nodes: represents two sets of programs together
        - join node: corresponds to an operation on pairs of programs
        - join/union can be anywhere
    - Volume of a VSA --> V(VSA) (number of nodes)
    - Size of a VSA --> |VSA| (number of programs)
    - VSA DSL restrictions
        - every operator has a small easily computable inverse
        - every recursive rule generates a strictly smaller subproblem
- PROSE framework

## Lecture 8

- Finite Tree Automaton (FTA)

example:
```
N ::= id(V) | N + T | N * T
T ::= 2 | 3
V ::= x

spec:
1 -> 9
```

- FTA is $$ A = {Q, F, Q_f, \delta} $$
    - states + alphabet, final states, transitions
- FTA can have a lot of states, even with simple grammars
    - idea: instead of one state/one value:

- Type transition net (TTN)
- context: component-based synthesis
    - given a library of components + query: type + examples
        - synthesize composition of components
- idea: build a compact graph of the search space using
    - types as nodes
    - components as transitions
- generate a petri net (special graph) that can define reachability of a
  particular program
- petri-nets
    - bipartite graph
        - two node types, edges only between each type
    - transitions consume tokens
        - produce token for return type
- representation-based vs enumerative
    - enumerative unfolds the search space in time, while representation-based
      stores the search space in memory
    - FTA --> bottom-up
        - with observational equivalence
    - VSA --> top-down
        - with top-down propagation


## Lecture 9

- topics
    - stochastic search
    - constraint solvers
    - constraint-based search
- search techniques
    - CFG - enumerative, good for small programs
    - PCFG/PHOG - weighted enumerative search. Still quite large exploration
    - local search -
- naive local search
    - to find the best program
    - pick a starting point, mutate and check if new mutation is better than old.
    - likely results in getting stuck at a local minima
    - MCMC sampling to give chance to get out of a local optima
- stochastic sampling requires a good cost function
- $$C_s(p) = eq_s(p) + \text{perf}(p)$$
    - components
        - source program
        - penalty for wrong results
        - penalty for being slow
- local search:
    - can explore program spaces with no a-priori bias
    - limitations?
        - only applicable with a good cost function
        - counterexample: round to next power of two

- Intro to SAT and SMT solvers
    - synthesis is combinatorial search, so is SAT
    - SAT solvers are quite good
    - ???
    - profit!!

- Boolean SATisfiability
    - logical operators/propositional variables
    - (gin \or tonic) \and (minor => !gin) \and minor
- SMT - Satisfiability Modulo Theories
    - not all formulas are modeled as boolean variables
    - SMT support for non-boolean problems

- Using SMT solvers
    - example: Array partitioning
        - partition array on N **evenly** into P sub-ranges
        - what happens when N is not divisible by P?
            - sizes of partitions don't differ by more than 1
- popular theories
    - equality and uninterpreted functions (axioms of equality and congruence)
    - linear integer arithmetic - e.g. {0, 1,, .... +, -}
    - Arrays - select/store at particular index
    - theories can be combined

## Lecture 10


- Constraint-based search
    - idea: encode synthesis problem as a SAT/SMT problem
    - program space can be encoded into parameters
    - two constraints by default: semantic correctness constraint + well-formed constraint
    - phi(C, i, o) - behavior/function of program

- how to define an encoding
    - define parameter space => C = {c1, c2, c3, ..., cn}
        - encode: program --> C
        - decode: C --> program
    - define a formula wf(c1, ..., cn)
        - must hold iff decode[C] is a well-formed program
    - define a formula phi(c1,...,cn, i, o)
        - holds iff (decode[C])(i) = 0

- properties of a good encoding
    - sound: well-formed and semantically correct
    - complete: decode[C] is a solution to the w-f and semantic constraints
    - small parameter space to avoid symmetries
    - solver-friendly : decidable logic, compact constraint
- DSL limitations
    - program space can be parameterized with a finite set of parameters
    - program semantics should be expressible as a decidable SAT/SMT formula

## Lecture 11


- Rich specifications (why not PBE?)
    - reference implementation
        - easy to compute result, but hard to do so efficiently or under
          structural constraints
    - assertions
        - hard to compute result, but easy to check desired properties
    - pre-post conditions
        - assign logical expression to be true before and after
    - refinement types
        - push the logical expression to be inside the types.
- why else to go beyond examples?
    - examples contain too-little information
    - output can be difficult to construct (outputs hard to compute)
- reasoning about non-functional properties (e.g., security protocols)

- why is this hard?
    - compute GCD -- infinitely many inputs
        - also, infinitely many paths (loops)
- constraint-based synthesis from specifications
    - how to solve constraints on infinitely many inputs?
        - CEGIS/SAT Solver
    - CBS from specifications
        - for all i/o examples, relate them by a function $$phi$$
        - there exists a program, such that for all inputs if the program
          satisfies the constraints, then it must also solve the program $$phi$$
- CEGIS, formally
    - given a harness create an encoding:
    - there exists [unknown variable set] for all [input/output examples] such that the [program] implies [an assertion/constraint]
    - linear constraint guaranteed to satisfy bounded observation hypothesis
      (linear constraint only needs two)
    - how to find the "good" inputs for which BOH holds
        - rely on oracle to generate counterexamples
- constraint-based synthesis from specifications
    - different constraints for different problems
        - CFGs
        - Components
        - "just figure out the constants"

    - generators in sketch: function with hole, always filled in the same way
        - generator can be filled in multiple ways, depending on the context where it is called.

## Lecture 12

- Encoding arbitrary semantics in programs with holes/constraints
- symbolic execution: no concrete values for each variable
    - variables represented as formulas instead of concrete values
- expression reads a state and produces a value
- states are modeled as a map (sigma) from variables to values
    - using lambda calculus to model Alpha, Sigma, Psi (symbols, state,
      constraints of valid executions)
    - $$A[[x]]\sigma$$ -- value of x at current state
    - $$C[[command]]<\sigma, \psi> = \langle {State}, {Assertions} \rangle $$

## Lecture 13

- Type-driven program synthesis
- specification --> types (programmer-friendly, informative)
- use types to prune search space
- synthesis accepts a type, a context, and produces a program
- Type systems
    - deductive system for proving facts about programs and types
    - defined using inference rules _over_ judgements
    - "under context $$\Gamma$$, term $$e$$ has type $$T$$
- a simple type system:
    - $$e ::= 0 | e + 1 | x | e e | \lambda x.e$$
    - types: $$ T ::= Int | T \rightarrow T $$
    - contexts: $$ \Gamma ::= . | x : T,\Gamma $$
- syntax guided enumeration
     - only enumerate programs with the proper types
        - still need to type check all programs before skipping
    - better idea: type-guided enumeration
        - enumerate all derivations generated by the type systems
        - extract terms from derivations
    - three ways to do typing judgements
        - type checking (term and type known)
        - type inference (term is known, type is unknown)
        - synthesis (term unknown, type is known)
    - search strategy: recursively transform synthesis goal using typing rules until finding a complete derivation
- generating some programs can result in equivalencies
    - only generate programs in normal form
    - restrict the type system to make redundant programs ill-typed
- type-driven synthesis in 3 steps:
    - annotate types with extra specification(examples, logical predicates,
      resources)
    - design a type system for annotated types (propagate as much info as
      possible from conclusion to premises)


## Lecture 14

- polymorphic types:
    - type that takes a can take a type as a parameter
    - e.g. generics in Java
- refinement types: augment types with predicates
- more complex types:
    - dependent function types: name arguments of a function, then use
      arguments later in the function/predicate
        - example, max function: `max :: x : Int -> y: Int -> {v: Int | x <= v && y <= v }`
- subtyping: T' is a subtype of T is all values of type T' also being to T
    - written `T' <; T`
- synquid limitations
    - user interaction:
        - types can be large and hard to write
        - components need to be annotated
    - expressiveness
        - specs are tricky/impossible to express
        - cannot synthesize recursive auxiliary functions
    - condition abduction is limited to liquid predicates
    - cannot generate arbitrary constraints
- synquid questions
    - behavioral: refinement types?

## Lecture 15

- constraint-based synthesis
    - loops are hard!
        - loops create many paths, integers gives many inputs
    - sketch handles loops by unrolling to a particular depth
    - if we can't unroll enough, get an unsatisfiable program

- Hoare logic: a logic for simple imperative programs
    - particularly loop invariants

The imp language:

```
e ::= n | x |
      e + e | e - e | e * e |
      e = e | e < e | e > e | !e | e && e
+ assignment/if/then/while logic
```

Hoare triples

- judgments grouped as {P} c {Q}
    - precondition P
    - if execution of c
    - postcondition Q
- if P holds in initial state $$|sigma$$, and if the execution from c from $$\sigma$$ terminated in a state $$'\sigma$$, then Q holds in $$'\sigma$$
    - "partial correctness" --> program may not terminate
    - "full correctness" --> program terminates
- need to be able to express values in final states are equivalent to values in beginning states
    - solution: introduce logical variables which don't appear in the
        actual program
    - this can make expressions for pre/post conditions more expressive

- "skip" --> {P} skip {P} --> state holds before and after
- assignment semantics in hoare logic
    - `{P[x --> e]} x := e {P}`
    - `(P[x->e]\sigma)` --> P holds in state
- semantics of compositions
    - need to invent intermediate insertion
- rule of consequence --> if you "need" less to prove in the beginning
  but can "prove" more at the end, it is essentially just as strong as
  the original preconditions

## Lecture 16

- synthesizing C code: verbose, unstructured, pointers (yuck)
- hoare logic is not enough to prove heap manipulating programs in the case
where two variables might point to the same location in memory

- suslik approach:
    - separation logic --> deductive synthesis --> provable program
- suslik swap example:

Precondition: `{x -> A * y -> B}`
Postcondition: `{x -> B * y -> A}`
- special part of separation logic: `*`
    - items joined with operator must be in disjoint heap locations

- judgement of a program:
    - P --> Q | c
    - A state satisfying P can be transformed into a state satisfying Q using a program c
- use rules to deduce /simplify pre-postconditions until "skip" (do nothing)
- proof search

## Lecture 17

- Nope paper contributions:
    - first to prove unrealizability for infinite program spaces
    - CEGIS for unrealizability
    - sound for synthesis and unrealizability
- NOPE limitations:
    - can timeout/run forever
    - situations where it can fail to terminate:
        - program that works on finitely many examples (but not infinite)
        - very large/difficult problem.
    - in practice.. works on < 1/2 benchmarks
    - limited to SyGuS


## Lecture 18

- Programming with humans
    - snippy: projection boxes to show programming sate while writing
      the program
    - live programming is a good environment for synthesis
        - inputs already exist (for simple programs)
        - encourages "small-step" PBE (easier for synthesizer)
    - snippy was more limited, but faster, lower cognitive burden, and synthesized
    more compact solutions
    - hoogle+
        - synthesize from types, but difficult because they are ambiguous
            - if output all programs with correct type, too many irrelevant programs
        - which solution is the right one?
            - use a test-based filtering
        - help beginners with types
    - RESL
        - REPL but with synthesis
            - syntactic specs + sketching + debugger
            - give granular feedback about grammar to synthesizer


## Lecture 19

- synthesis with users cont'd
- today: Rousillon, Wrex, Regae

- Rousillon / Helena
    - web scraping tool for social scientists
    - problem with traditional scrapers: need to reverse-engineer web-page DOM
    - types of web data:
        - distributed: user must naviagete between many pages, click, use forms, etc
        - hierarchical: tree-structured data
- Wrex:
    - goals: for data scientists, not a black box:
    - users create a data frame and sample it
    - Wrex has an interactive grid where users derive a new column and give
      transformation examples
    - give code window with synthesized code
    - apply synthesized code to full data frame and plotting.

-
