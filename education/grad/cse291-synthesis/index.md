---
---


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


- context-free grammer (CFG) define the space of programs which represent all **ground** and **whole** programs


- How big is the space of problems for a simple CFG?

CFG
```
E ::= x | E @ E
```

For a given syntax tree of depth $$N$$, the number of programs that can be
generated for a given depth is $$N(d-1)^2 + 1$$

Size explodes as complexity of program increases. For more complex grammars it is even larger

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
    - expensive checks on every candidate defeats purpose of space pruning checks
- observational equivalence
    - in PBE we only care about equivalence on given inputs
        - easy to check efficiently
        - many more programs equivalent on particular inputs than on the
          entire input space
- term-re-writing system (TRS)
    - transform one term into another form of a term
    - issues with deciding which terms should be included, or when to use
        - e.g. commutativity rules could result in an infinite re-writing of a term - leading to an infinite loop
- just don't enumerate over non-normal form expressions - remove as soon as an equivalence is identified


- Built-in equivalences
    - for a set of ops - equivalence reduction can be hard-coded into the tool _or_ directly into the grammar
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
        - the predicates might not cover all the terms in a manner which solves the problem

- naive alternative to decision tree learning for synthesizing branch
  conditions?
  - learn atomic predicates that precisely classify points
    - why worse? - 
    - as bad as Esolver? - enumerate over many possible conditions not
      much better
  - next best is decision tree learning w/o heuristics
    - why is this worse? more space used by decision tree