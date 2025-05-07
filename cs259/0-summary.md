# 0 - Summary of content

## Notation

$\Sigma$ - The entire set of symbols in a language (alphabet)

$\delta$ - Function used in an automaton which takes in the current state and input symbol to produce a new state

$\epsilon$ - Null or empty string

$\Gamma$ - A stack in a PDA, or a set of tape symbols for a Turing machine

## Regular Language

Any language, which can be interpreted by reading a string left-to-right, while only keeping track of a certain state.

### (Non-) Deterministic Finite Automaton

An automaton consisting of a 5-tuple $(Q, \Sigma, \delta, q_0, F)$, such that...

- $Q$ - Finite set of states
- $\Sigma$ - Alphabet
- $\delta$ - Transition function, such that $\delta : Q \times \Sigma \rightarrow Q$
- $q_0 \in Q$ - Initial/Starting state
- $F \subseteq Q$ - Set of accepting states