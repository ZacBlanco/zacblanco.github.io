## Notes

- Extend SyGuS grammar of possible programs with probabilistic model on likelihood of each program.
- higher order grammar from known solutions

- Typical solvers don't look at _likely_ candidates
- A* search algorithm down the tree
    - requires a good search heuristic

- Efficient A*
    

## Summary



## Strengths

## Weaknesses

## Questions

- What does Euphony use as behavioral constraints? Structural constraint? Search
  strategy? How are they different from EUSolver?


- Consider Fig 2b, where the synthesizer is unrolling the sentential form
  Rep(x,"-",S). When the search is guided by a PHOG, it considers the weighted
  productions shown in Fig 2a (top). What would these productions look like if
  we replaced the PHOG with a PCGF? With 3-grams? Do you think these other
  probabilistic models would work as well as a PHOG?

- Consider Example 3.2. Explain in English the intuitive meaning of h(S) = 0.1
    and why it is the case.

- Consider Theorem 3.7. Give an example of sentential forms n_i, n_j and set of
  points pts such that n_i and n_j are equivalent on pts but not weakly
  equivalent.
