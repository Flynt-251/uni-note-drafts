# 1 - Regular Languages

In vague colloquial terms, a regular language is a language where we can determine a valid string, without any prior knowledge as we are reading. In other words, as we read a string, we are in one state at a time, and determining whether or not the string is valid, is dependent on this state and the letter we are currently reading. This definition hints at the use of a **Deterministic Finite Automaton** (DFA), which we can use to represent a regular language. We can also represent a regular language using regular expressions.

## Deez Figurez r Amuzing

A **Determinisitic Finite Automaton** (DFA) bears resemblance to a directed graph, in that we have states (vertices) that we can transition between (like edges). A DFA is defined with the following notation:

$D = (Q, \Sigma, q_0, F, \delta)$

So it's a five-tuple with five symbols we've never seen before. So let's go over what each of these mean.

- $Q$ - The set of all possible states. This is a *finite* set.
- $\Sigma$ - The alphabet, i.e. all characters that a string can be made up of.
- $q_0$ - The initial state. This is where we start when validating a string.
- $F \subseteq Q$ - The set of accepting states. If we fully read a string and end up in this set, the string is valid.
- $\delta : Q \times \Sigma \rightarrow Q$ - The transition function (🏳️‍⚧️?). This defines how we can move between states, given what input we have.
  - $\delta(q_a, \alpha) = q_b$ where $q_a, q_b \in Q$ and $\alpha \in \Sigma$

As you can imagine, the transition function does a lot of heavy lifting for us. We can represent its *function*ality using either a state diagram, or a state transition table.

If a DFA $M$ represents a regular language $R$, then we can state $R = L(M)$, where $L$ is some arbitrary function which returns the set of all valid strings produced by the DFA.

### Even more Trans

Using the transition function, we can then also define $\hat{\delta} : Q \times \Sigma* \rightarrow Q$, which is the **Extended Transition Function**. Simply put, we use this to determine the final state of a whole string (that's what $\Sigma*$ means, it'll make more sense later). The definition of this function is recursive, and as we all know, recursive functions are great for making recursive functions, just like how all toasters toast toast.

All recursive functions need a base case (unless you with to curse said function with immortality), and we do this with $\epsilon$, which represents the **empty string**. Funny things can happen when we use $\epsilon$. For our recursive step, we have an input string which we'll define as $w = w' \circ a$.

$\hat{\delta}(q, \epsilon) = q$

$\hat{\delta}(q_a, w' \circ a) = \delta(\hat{\delta}(q_a, w'), a)$

So let's say we were to check if the word $``wolf"$ was in our language. The fully expanded function would look like this:

$\delta(\delta(\delta(\delta(q_0,w),o),l),f)$

When we're checking the validity of a string in a language, this is called a **run** of the language on that word. In our above example, this is a run of some language $L$ on "wolf". If we end up in an accepting state, we then call this an **accepting run**, so we'd be able to state $wolf \in L(M)$.

## The worst nightmare of every developer

No, not AI taking over your job, we all know that's never going to happen, I'm talking about **Regular Expressions**, otherwise known as *RegEx*. I'll give you a minute to collect yourself.

Done? Good. Let's move on. You'll be pleased to know that, this time, regular expressions aren't that complicated, only using a few different expressions.

- $R = a$, where $a \in \Sigma$
- $R = \epsilon$, i.e. $R$ contains only the empty string.
- $R = \emptyset$, i.e. $R$ is empty. *Think about how this is different to the above.*
- $R = R_1 + R_2$, meaning $R$ consists of all elements from $R_1$ and $R_2$, basically like a union operation. When constructing regular expressions, this colloquially translates to "or".
- $R = R_1 \cdot R_2$, i.e. concatenation. May be represented without the dot.
- $R = R_1*$, called **Kleene star**, meaning any number of copies of $R_1$.
- $R = R_1^+$, called **Kleene plus**, meaning at least one string in $R_1$.

## We're gonna need a bigger coffee

That's right, we can go above a regular language. Be careful to not call such languages *irregular*, they're **Non-regular languages**. So trivially, these are languages which we can't represent using a DFA or RegEx.

For instance, let's look at $L = \{0^n1^n : n \in \mathbb{N}\}$.

In plain english, this is the set of all strings of zeros, followed by an equal number of ones. It's impossible to create a RegEx for this, and for a DFA/NFA, we would need to keep track of up to $n$ zeros. Since we'd need to track at most an infinite number of zeros, this can't be a finite automaton! So now we know that we can't make a DFA, NFA or RegEx, but unfortunately, because mathematicians are very pedantic, we now have to answer: well how do you know? We simply can't just give up without cause.

### It's my hill, Nero.

We can prove non-regularity using the **Myhill-Nerode Theorem**. Let's go back to our extended transition function. If we have a set of strings that all end up in the same state, like this...

$\hat{\delta}(q_0, \text{wolf}) = \hat{\delta}(q_0, \text{fox}) = \hat{\delta}(q_0, \text{coyote})$

Then we say that "wolf", "fox" and "coyote" are all equivalent. I mean, technically they are but they also aren't. That's a different topic though. What's important here though, is *they all end up in the same state*. So for each of these, if we were to append the same suffix to each string, they'd still end up in the same state, although the contrapositive doesn't always hold: two strings don't have to end in the same state for their suffix to bring them together.

#### Foxes and Wolves are different

Now let's dust off some more trauma and bring back some CS130 knowledge. Remember **equivalence relations and classes**? To put it simply, an equivalence relation is used to state that two elements are somehow equivalent (WHAT!?), and this is a reflexive, symmetrical and transitive relation. Then, an equivalence class is a set of sets of elements that are all "equal" to each other. Here's an example to jog your memory.

$X = \{\text{wolf}, \text{fox}, \text{coyote}\}, R = \{(\text{wolf},\text{wolf}), (\text{fox}, \text{fox}), (\text{coyote}, \text{coyote}), (\text{wolf}, \text{fox}), (\text{fox}, \text{wolf})\}$

Let's first check that $R$ is an equivalence relation. We can see it's reflexive, since for each single element $x \in X$, we can see $(x,x) \in R$. We also have $(\text{wolf}, \text{fox})$, and so by symmetry, we must also have $(\text{fox}, \text{wolf})$, which we do. There's nowhere that transitivity needs to apply, but if you're feeling pedantic, you could technically say $(\text{wolf}, \text{fox}) \wedge (\text{fox}, \text{wolf}) \rightarrow (\text{wolf},\text{wolf})$.

So now we know $R$ is an equivalence relation. Then, we can construct a set of equivalence classes $=_R$, which looks like this...

$\{\{\text{wolf}, \text{fox}\}, \{\text{coyote}\}\}$

I guess foxes and wolves aren't so different, since in this example, this means that "wolf" and "fox" are equivalent to each other.

#### So what's the point of all that?

Now to get back to introducing more new terms. Remember how we said that if two strings are equivalent, we can give both of them the same suffix and they will be in the same state? Well, we can always make a suffix that makes both strings valid for that D/NFA. To the automaton, these two strings are **Indistinguishable**, because they both produce accepting runs. Then, if we have two strings, one of which *does* give an accepting run, and one which *doesn't*, then these are **distinguishable**. Note that if both strings don't give accepting runs, then they are indistinguishable.

Let's write this formally. If we have two strings $x, y \in \Sigma*$, and a suffix $z \in \Sigma*$, which we call a **certificate**, then $x$ and $y$ are indistinguishable if $xz, yz \in L$, or $xz, yz \notin L$. Naturally then, $x$ and $y$ are distinguishable if $xz \in L$ and $yz \notin L$, or $xz \notin L$ and $yz \in L$.

So, if $x$ and $y$ are indistinguishable, then they are essentially equivalent. We write this as $x \equiv_L y$, and $x$ and $y$ must be in the same equivalence class. Likewise, if these are distinguishable, but it's possible to construct separate suffixes for both strings such that they are in $L$, then they must be in different equivalence classes.

#### Tying it together

**Myhill-Nerode Theorm states that a language $L$ is only regular, iff $\equiv_L$ is finite.** Which is to say, there must be a finite number of equivalence classes. So how can we apply this to prove a language isn't regular?

First, we need to construct an infinite set of strings (any finite language is regular, just imagine mapping every string to a DFA), then we can either...

- **Find a certificate that distinguishes every string from each other**, i.e. find a regular language which, for each element, creates a unique suffix for each string, that is incompatible with any other string.
- **Find separate certificates that distinguish every pair of strings**, i.e. find a general pattern for each string, and then use this to create a certificate that distinguishes the two.

Enough talk, let's apply this!

#### Examples

##### Example 1 - Equal 0s and 1s

Let's start with our first example, $L = \{0^n1^n : n \in \mathbb{N}\}$.

First we construct an infinite set of strings. Easy enough, let's do $0^n$.

Now we need to either make one certificate to distinguish all strings in this set, or generalise the structure of each pair of strings and make a certificate for each. The former is clearly the easier option, we can just make $1^n$.

To show that this works, we'll use a constant $k \in \mathbb{N}$. We know that $0^k1^k$ is a valid string, but for some $m \neq k$, then $0^m1^k$ is not. Clearly each value of $n$ needs its own unique verifier, even if we can spot a pattern! This has to mean then, that there's an infinite number of states that need their own route to be part of an accepting run. *Infinite States you say*? This must not be regular then!

##### Example 2 - Primes

Let's use an example that will require us to use the latter approach, $L = \{1^n : n \text{ is prime}\}$.

So, infinite set of strings. We'll use $1^n$. Because there's not necessarily an easy sequence to work with when dealing with primes, we can't create a general certificate, so we should use the latter approach of showing every pair of strings is distinguishable.

We can quite easily pull out a pair of elements, let's make these $1^i$ and $1^j$, and $i < j$. Now let's make a prime number, which is smaller than $i$, and greater than 2. Clearly $1^p$ is in $L$. Now we need to show that we can't use the same verifier for the pair of strings $1^i$ and $1^j$, by showing how we may extend $1^p$, with more ones.

$p + 0(j - i) = p$, so this is prime, but $p + p(j - i)$ is clearly not prime.

So somewhere along the line, we switch from prime, to not prime, in the sequence $p + 0(j - i), p + 1(j - i), \dots, p + p(j - i)$. Let's define $k$ which captures this switch, so $p + (k-1)(j-i)$ is prime, but $p + k(j-i)$ is not.

So we've established that $p + (k-1)(j-i)$ is prime, so this number of ones must be in $L$. Hence, let's make a certificate $z = 1^{p + (k-1)(j-i) - i}$.

Of course, this fits $1^i$ quite well, as $p + (k-1)(j-i) - i + i = p + (k-1)(j-i)$, which we know is prime.

But, what about $1^j$? Well, we get $p + (k-1)(j-i) - i + j = p + (k-1)(j-i) + (j-i) = p + k(j-i)$ which isn't prime.

So we have made a general proof that it doesn't matter what numbers we pick, *no two strings can have the same certificate*.

### I'm not even gonna try making a joke out of this.

It's just gonna be flat out inappropriate. We're talking about **Pumping Lemma**. You'll be pleased to know that this requires much less setup than Myhill-Nerode, but it can still be a pain to get your head around.

For pumping lemma, we start with a **pumping number**, which we define as such: $m \in \mathbb{N} \setminus \{0\}$. We say that there exists such an integer $m$ where for any string $w \in L$ such that the length of $w$ is at least $m$, then we can split $w$ into three components to get $xyz$. The following properties then apply to this:

- $|xy| \leq m$ - simply, $xy$ is short
- $|y| \geq 1$ - $y$ cannot be an empty string
- $\forall i \in \mathbb{N}, xy^iz \in L$ - $y$ can be "pumped", or repeated any number of times.

I'm not even sure I want to dwell on what else "pumping" could be interpreted as.

But again, simply put, once strings reach a certain length, they will follow a structure where the middle section is simply a loop repeated over and over again. It should be noted that **you can only use pumping lemma to prove a language is non-regular**.

#### Wait, we can already make a proof?

Yes! Told you it wouldn't take that long! Since pumping lemma gives us a property of regular languages, we're building a proof of contradiction. Then, we need to pick a pumping number. Remember that the length of the whole string that we test needs to match or exceed the value of this number. Then, we can choose this string, keeping in mind we will be splitting it into the three components $xyz$. Then, find a value of $i$ such that $xy^iz \notin L$.

##### Example 1 - The return of the primes

Would kids be better at maths if they talked more about primes? I dunno, something something KSI and mouldy cheese. Anyway, let's return to trying out $L = \{1^n : n \text{ is prime}\}$.

First, we'll assume this language is regular. Then there's some pumping number $m$, such that there are a subset of strings of at least length $m$ that we can pump. We'll construct $w$, such that $w= xyz = 1^p$, where $p \geq m$ and $p$ is prime. So, our string is split into three segments $1^\alpha 1^\beta 1^\gamma$, where $\alpha + \beta + \gamma$ is prime.

So now our task is to find some $i$ value which violates the lemma. We know that this is just $\beta$, so let's just use our other two values and set $i = \alpha + \gamma$. Then we get $\alpha + \alpha + \gamma + \gamma = 2(\alpha + \gamma)$. Hmm... that's obviously not prime, is it? This means we've reached a contradiction then. $\Box$

##### Example 2

$L = \{0^k 1 u 0^k : \exists k \geq 1, u \in \Sigma*\}$

Colloquially, two equal-length sequences of zeros, sandwiching an arbitrary string that starts with a 1.

Since $u$ can be any string, let's make it $\epsilon$ to keep things simple. So, $w = 0^p 1 0^p$, where $p \geq 1$.

Now to split this string. Let $x = 0^{p-n}, y = 0^{n}, z = 10^p$, where $n \geq 1$, so $xyz = 0^{p - n} 0^n 10^p = w$.

Finally, we can set a value of $i$. It should be clear that any value $i \neq 1$ will violate. No need to over-complicate, so let $i=2$. Then, $xyz = 0^{p-n} 0^{2n} 1 0^p = 0^{p+n}10^p$.

Since $n \geq 1$, then $p+n > p$, so $0^{p+n}10^p \notin L$. We have reached our contradiction. $\Box$