## Notes

- Extend SyGuS grammar of possible programs with probabilistic model on likelihood of each program.
- higher order grammar from known solutions

- Typical solvers don't look at _likely_ candidates
- A* search algorithm down the tree
    - requires a good search heuristic
- algorithm works on a directed weighted graph, created on the fly.
- generate graph by appling production rules, then follow the graph path with
  higher probabilities the production rule moves toward the solution.
  - dijkstra's doesn't work because it needs to expand a large number of paths
  - A* is much faster and is heuristic based -- no need to expand so many paths.
- PHOGs - Probabilistic Higher-Order Grammars, new learning method.
  - captures positional position of production
  - can suffer from overfitting
    - new learning method for PHOG alleviates overfitting (inspired by **transfer learning**)
- key idea for PHOGs is to design a _feature map_
- paper only considers "leftmost derivations" (production always applied to
  leftmost non-terminal symbols)
- probabilistic program model given rather than a CFG
  - not pure SyGuS?
- heuristic never overestimates the distance to a goal
- PCFG: probabilistic context-free grammar

## Summary

This paper presents a new approach to solving a type of synthesis problem using
probabilistic grammars and A* search. One of the main optimizations the authors
present in this paper is the structuring of the synthesis problem as a graph
search, where an algorithm like A* may be applied to the search. The authors
also introduce the idea of PHOGs (probabilistic higher-order grammars) which
allow assignment of probabilities to the problem grammars in order to search for
solutions efficiently. Given the right inputs the combination of these two
techniques results in a fast search for synthesis solutions. The authors are
also able to apply previously studied optimization techniques in SyGuS problems
such as equivalence purning and divide-and-conquer enumeration (EUSolver) to
further improve the performance of their system.


## Strengths

- optimization on the graph search using A* - seems like a good way to avoid
  enumerating bad programs. Clear advantage over dijkstra's
- heuristic is interesting, because it can have a great effect on the search time
  - tuning of heuristic could drastically improve the performance without much
    more effort

## Weaknesses

- Isn't based on the original SyGuS problem - it requires a statistical program
  model
- the paper itself doesn't consider all types of program derivations - the
  authors only consider "leftmost derivations"
## Questions

- What does Euphony use as behavioral constraints? Structural constraint? Search
  strategy? How are they different from EUSolver?
  - Behavioral Constraints
    - input/output examples
  - Structural Constraints
    - PHOGs
  - Search Strategy:
    - A* search guided by PHOGs

- This differs from EUSolver as it doesn't utilize (conditional) context-free
  grammars, but rather a probilistic higher-order grammar. Also, it attempts to
  guide the search further in the tree if the probabilistic model, rather than
  trying shorter or more basic solutions first

- Consider Fig 2b, where the synthesizer is unrolling the sentential form
  Rep(x,"-",S). When the search is guided by a PHOG, it considers the weighted
  productions shown in Fig 2a (top). What would these productions look like if
  we replaced the PHOG with a PCGF? With 3-grams? Do you think these other
  probabilistic models would work as well as a PHOG?

I think this question has a typo. I am going to assume that "PCGF" is actually
"PCFG" and stands for "probabilistic context free grammar" assuming the G and F
in the acronym were switched.

e.g. for the first line, the production would look like:

Pr(S --> "." | Rep(x, "-", S)) = ...

I've never learned about 3-grams before. But presuming the wikipedia page is
accurate, then you might represent the 3-gram as the probability of a variable x being

Pr(S --> "." | (Rep, "-", S))

I think these other probabilistic models could at minimum be as accurate if they
capture as much or more information than the PHOG. The PHOG attemtps to capture
the surrounding context, so for a particular n-gram or PCFG were able to capture
the same context, then they could be as accurate. PCFGs don't seem to capture
the search context which would lead me to believe they probably can't achieve
the same performance.

- Consider Example 3.2. Explain in English the intuitive meaning of h(S) = 0.1
    and why it is the case.

Plainly, h(S) = 0.1 means 0.1 is the upper bound on probability that S or an
expression derived from S could be the solution. Part of the reason is the
specification of the grammar. The reason this is the case is that the grammar
specifies the production rule of S --> c being 0.1

- Consider Theorem 3.7. Give an example of sentential forms n_i, n_j and set of
  points pts such that n_i and n_j are equivalent on pts but not weakly
  equivalent.

pts: {"--__"}
n1 = Len(Rep(S, "-", ""))
n2 = Len(Rep(S, ".", ""))