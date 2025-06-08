# Natural Deduction

> Note: Multi-line KaTeX rendering is a pain, so proofs will be shown like this:
> $X [ Z \cdot \cdot \cdot ] Y$
> Normally, you'd place expressions vertically.

Unlike a resolution proof or semantic tableau, it's not feasible to solve a boolean expression through natural deduction by means of a computer: it does not involve breaking down an expression into a simpler structure, rather it focusses on building assumptions to prove a consequence. Natural Deduction requires some level of "Working Backwards", so it may not be initially intuitive to work out at first.

Natural deductions work by deducing assumptions inside boxes. At the top of a box, we make an assumption. Then, at the bottom, we have some outcome. Then, outside of this box, we can say the assumption implies its outcome. It looks like this:

$[X \dotsb Y] X \rightarrow Y$

Which says that, from $X$, we can conclude $Y$, hence $X$ implies $Y$.

## Complete Rules

### Constant

Or, what to do if you can do with a top or a bottom (make sure you've washed first).

$[... \bot] X$

So if an assumption is not satisfiable, we can make any conclusion we want to.

$[...] \top$

And we can produce top from any expression, since we've said our assumption was true anyway.

### Negation Rules

Because you can't not not mess with nature. Confused? Me too.

$[X, ¬X] \bot$

If we immediately draw the negation of an expression, we can conclude bottom.

$[X \dotsb \bot] ¬X$
$[¬X \dotsb \bot] X$

And if we reach bottom from an assumption, then it clearly cannot hold and so we should conclude its negation.

### Alpha and Beta rules

Are we really talking about this? Surely people don't talk about the Omegaverse anymore...

**Alpha Elimination** - $[\dotsb \alpha] \alpha_1$ or $[\dotsb \alpha] \alpha_2$
**Alpha Introduction** - $[\dotsb \alpha_1, \alpha_2] \alpha$

So if we have an alpha (conjunctive) expression, we can either only conclude one of its components, or combine both to give the full expression.

**Beta Elimination** - $[¬\beta_1, \beta] \beta_2$ or $[¬\beta_2, \beta] \beta_1$
**Beta Introduction** - $[¬\beta_1 \dotsb \beta_2] \beta$ or $[¬\beta_2 \dotsb \beta_1] \beta$

So for *elimination*, if we find that one portion of the beta (disjunctive) expression is false and immediately find that the beta expression is true, then we have to conclude that the other half is true. *Introduction* does the opposite, where if we assume the negation of one half, and conclude the other half is true, then we can we can infer the whole beta expression.

## Derrived Rules

You don't have to remember these rules, since you can use the previous info we know already to reach them, but they are very useful.

### Double Negation

Because you can't not not not not mess with nature. ...what?

$[\dotsb ¬¬X] X$
$[\dotsb X] ¬¬X$

### Copy Rule

Because you can't not not mess with nature. Confused? Me too.

I just pasted whatever was in my clipboard for that one.

$[X] X$

### Implication

Did you consider them when choosing your career? I hope you did!

$[X \dotsb Y] X \rightarrow Y$

### Modus Ponens and Tollens

Ew, Latin.

$[X, X \rightarrow Y] Y$

$[¬Y, X \rightarrow Y] ¬X$

### Excluded Middle

It's no fun being excluded, especially as a middle child. Just read Diary of a Wimpy kid (only the good ones tho).

$[\dotsb] X \vee ¬X$