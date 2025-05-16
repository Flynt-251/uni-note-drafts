# First Order Logic

By now, we can quite easily work with boolean predicates, operators and proof systems. But what do we do when we don't just have true and false values? What happens when we start to work with numbers? Then we start working with **First Order Logic**.

## Babe wake up, new symbols dropped

We've already seen *propositional connectives*, i.e. $\wedge$, $\vee$ and $\rightarrow$, and constants. Now we introduce quantifiers, variables, constants, functions and relations.

- **Quantifiers:** $\exists, \forall$ - meaning "There Exists at least one", and "For all".
- **Variables:** $x, y, z$, etc. - Any symbol that isn't restricted to a single value.
- **Relations:** $=, <, >$ - Any function or operation that takes in values and gives a boolean value.
  - e.g. Equality, $x = y$.
- **Functions:** $s(x), f(x), x + y$, etc. - Something that outputs a new value based on its inputs
  - e.g. the successor function $s(x)$.
- **Constants:** $1,2,3$, etc. - Any symbol of a predetermined value.

## Je ne peux pas parle la français

A **First Order Language** defines the basic notation of first order logic, and is represented as a 3-tuple, $L(R, F, C)$. Generally this defines how we write mathematical notation.

- $R$ - The set of relation symbols. This is finite or countably infinite.
- $F$ - The set of function symbols. This is also finite or countably infinite.
- $C$ - The set of constant symbols. This too, is also finite or countably infinite.

Then, from this language, we can construct a set of **terms**, which consist of any variables, constant symbols and functions with all arguments filled (i.e. $f(x,y,z)$ where all values are terms). The **family of terms** is the smallest set which allows us to create any term we want.

**Atomic Formulas** are strings containing a relation of n terms, all of which are filled, such as $x = y$ or $\text{isPrime}(5)$. There is then a **family of formulas** which satisfy the following:

- Any atomic formula is a formula of $L(R,F,C)$.
- If $A$ is a formula, then so is $¬A$.
- If $\circ$ is some binary connective, then we can combine formulas $A$ and $B$ to get $A \circ B$, which is also a formula.
- If we have formula $A$ and variable $x$, then $\forall x A$ and $\exists x A$ are also formulas.

It's also important that we keep track of variables, as these can be **free** or **bounded**. We define **free-variable occurrences** as such:

- If the formula is atomic, all variables are free-variable occurrences.
- Free-variable occurrences in $A$ are also free-variable occurrences in $¬A$.
- For $A \circ B$, any free-variable occurrences in $A$ and $B$ are also in the new, joined formula.
- If we have $\forall x A$ or $\exists x A$, then any variables that are **not** $x$ are free-variable occurrences.

So now we know how to write first-order logic. But how can we interpret meaning from this?

## Notice how they never smile for the camera?

We create a **model**, $M = (D,I)$ for a language $L(R,F,C)$, where the non-empty set $D$ is the **domain** of the model, and $I$ is essentially a mapping function from the language to the domain.

- For any constant $c \in C$, this constant maps to a $c^I \in D$.
- Any function $f \in F$ with $n$ arguments, maps to some function $f^I : D^n \rightarrow D$.
- Any relation $\mathbf{R} \in R$ with $n$ arguments, maps to some relation $\mathbf{R}^I \subseteq D^n$.

We can then make **assignments** of sets of variables to the set $D$. When we do this, this is called a **mapping**. Where we define a mapping $A$, on a variable $x$, we denote that this value has been mapped using the notation $x^A$.

Constants are not affected by mappings, so $c^{I,A} = c^I$. Variables don't have a preset value in a model, so we have to rely on a mapping for them to be useful in a use case, so $x^{I,A} = x^{A}$. And for functions, we immediately interpret their meaning from the model, so $(f(x_1, x_2, ..., x_n))^{I,A} = f^I(x_1^{I,A}, x_2^{I,A}, ..., x_n^{I,A})$.

Now that we have some meaning in place, we can determine if certain formulas are true or not.

- $R(x_1, x_2, ..., x_n)^{I,A}$ is true, *iff* $(x_1^{I,A}, x_2^{I,A}, ..., x_n^{I,A}) \in R^I$
- $(¬X)^{I,A} = ¬(X^{I,A})$
- $(X \circ Y)^{I,A} = X^{I,A} \circ Y^{I,A}$
- For some formula $\phi$...
  - $(\forall x \phi)^{I,A}$ is true, *iff* $\phi^{I,A'}$ is true **for each** assignment of $x$ in $A$, which we express as $A'$.
  - $(\exists x \phi)^{I,A}$ is true, *iff* $\phi^{I,A'}$ is true **for some** assignment $x$ in $A$, which we express as $A'$.

## Application time

Now we're almost ready to apply this knowledge to solving resolution and tableau proofs, but we just need a couple more definitions, namely $\gamma$- and $\delta$-functions, which allow us to temporarily assign a variable under a universal ($\forall$) or existential ($\exists$) quantifier respectively.