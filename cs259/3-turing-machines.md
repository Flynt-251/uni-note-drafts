# 3 - Turing Machines

Turing machines are a lot like the computers we use today. That's why we call programming languages *turing complete*. Both of these run on the same idea: you have some amount of memory you have read-write access to, a number of possible states, and the ability to accept or reject an input, or possibly run forever. The main difference is the memory for a Turing Machine is infinite, but we all know that in reality, such a thing is impossible. To create a Turing Machine, we must first create the universe. Then knowing the notation is a good start.

$T = (Q, \Sigma, \Gamma, \delta, q_0, q_{accept}, q_{reject})$

Some of these terms will be familiar, namely $Q, \Sigma, \delta, q_0$ and $q_{accept}$, denoting the set of states, input alphabet, transition function, initial state, and accepting state.

But what are these new symbols? $\Gamma$ is the **tape language**. 