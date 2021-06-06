## Introduction

- two new techniques:
    - interaction model to specify how to treat different parts of a program
    - augmentation of existing examples
- domain of regex
- semantic augmentation + data augmentation
- when provided examples fall short, older systems require user to input new
  examples
- other interactive approaches rather than simply generating examples:
    - specify latent structure in string
- semantic augmentation marks portions of inputs as literal/general
- convert input example to a deterministic finite automaton to decide the class
  of string represented by the regex
-


## Questions

### Summary

The key idea in this paper is to guide synthesis with constraints, but do so by incorporating user interactions. By incorporating user interaction into the model, the user may be able to make better judgements than a computer or algorithm can, but prevent them from needing to perform a more difficult task (e.g. writing a program). In this work the authors present a synthesis model where a PBE synthesizer is augmented. There are two new changes. First is the semantic augmentation that allows users to annotate particular examples. The second augmentation is the data or constraint augmentation where the synthesizer also helps generates additional examples that the user can choose to use to validate the synthesized program.

### Strengths

- humans can make better decisions than algorithms on some problems.
- DFA used to generate distinguishing input exampels

### Weaknesses

- Limited to simple grammar with regexes
- not as fast as synthesis without users
- strings only

### What does Regae use as behavioral constraints? Structural constraint? Search strategy?

- behavioral
  - annotated examples and counterexamples
- structural
  - regex DSL
- search strategy
  - top-down enumerative synthesis

### Does the semantic augmentation technique help enhance behavioral or structural constraints (or something else)? What about data augmentation?

the semantic augmentation enhances the behavioral constraints, which is then taken account of during the search in order to determine which parts of a program should generalize

### Line 10 in Algorithm 1 prunes a partial regex when it is infeasible, but the paper does not focus on how infeasibility is determined. Can you come up with a condition when a partial regex can be soundly rejected? Use the regex concat(<num>,e2) as an example.

partial regexes can be rejected if no matter what, the regex can't possibly match on the input examples. e.g. if concat(<num>, e2) is the partial regex, but no `<num>` exists in any of the input constraints, then there is no sense in expanding this term.

### In the user study, they randomize the order in which the two tool variants are presented to a participant (i.e. control then treatment or treatment then control). Why is this important?

The order in which questions or experiments are presented is important, because learning about one method (e.g. guiding examples) versus writing the program beforehand could bias the results of the 2nd group once learning about the new (easier) or old (more difficult) techniques