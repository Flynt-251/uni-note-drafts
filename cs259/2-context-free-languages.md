# 2 - Context-free Languages

## We're gonna need a bigger... context?

There are also languages *beyond* context-free, which we will cover next. But how can we prove that a language isn't context free? Well, we've already covered the fact that we can represent a context-free language as a rooted tree, and we know that if the language is finite, then it's regular, so up to a certain point, we must have part of the language repeating over and over again, extending the tree infinitely... wait. That sounds familiar. It's a lot like the **pumping lemma**! Oh. Okay, please can I ask you to contain yourselves again...

### Nope, you're getting nothing outta me!

Again, we have a **pumping number** $m$, but this time the definition of this number is a little more involved. It's still at least 1, but we can apply some semantics.

- Define $b$, which is the largest number of values in the right-hand side of a rule. This would translate to the maximum number of children on a non-terminal leaf node in our (incomplete) tree.
- Hence, we must have at most $b^h$ terminals at the bottom, where $h$ is the height of the tree.
- If $b$ is relatively small, but we have a very long string, then $h$ must be increasing. If $h$ is greater than the number of variables, then we have to be repeating variables somewhere (pigeonhole principle, coo.).
- Pumping lemma suggests that we are repeating some variable, so the pumping number is $b^{|V| + 1}$.

This is intended as a guide of how big $m$ could be. We can of course get a string smaller, or bigger than this.

Now onto the rest of it. Instead of splitting the string into three parts, we split it into *five*, giving us $w = uvxyz$. We then have the following rules:

- $|vxy| \leq m$, i.e. $vxy$ is short.
- $|vy| \geq 1$, meaning either of $v$ or $y$ are nonempty.
- $\forall i \in \mathbb{N}, uv^ixy^iz \in L$

#### Make the proof!

Again, we'll want to make a proof of contradiction. We then set our pumping lemma to $m$, pick a string we want to split into five, and show that we can't split the string in a way that we can get $\forall i \in \mathbb{N}, uv^ixy^iz \in L$. Let's do this!

#### Example 1

$L = \{a^nb^nc^n : n \in \mathbb{N}\}$

We don't have much choice in terms of our string here, we can only really go with $a^mb^mc^m$. But how do we split this into five parts? Well, we can simplify this by only focussing on what $vxy$ is. We are able to position this substring anywhere within, so it could be $a^j$, $b^j$, $c^j$, $a^jb^k$ or $b^jc^k$. Now we can set our value $i$ to be any value other than 1, and we know that this will unbalance all of the counts of letters, no matter how we split the string. $\Box$