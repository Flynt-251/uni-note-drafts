# OS6 - Deadlocks

A deadlock is a scenario where two or more processes are waiting indefinitely for an event that is triggered by another process that is also waiting. It's like that awkward moment when you're texting a friend and you see them typing, so you wait for them to send their message first. Unbeknownst to you, they've done the same: they're waiting for you to stop typing as well. Now neither of you are talking!

Let's look at a (fairly simple) example.

- Process $P_1$ accesses Lock $L_1$, and Process $P_2$ accesses Lock $L_2$
- Process $P_1$ accesses Lock $L_2$, and Process $P_2$ accesses Lock $L_1$
- Both processes complete their critical section.
- Process $P_1$ unlocks Lock $L_1$, and Process $P_2$ unlocks Lock $L_2$
- Process $P_1$ unlocks Lock $L_2$, and Process $P_2$ unlocks Lock $L_1$

See the problem? Hint, you only need to look at the first two lines.

The first line is fine, no process has the locks for $L_1$ or $L_2$, but immediately afterwards, both processes try to access the lock that the other has. This causes them both to wait for each other, meaning we've stagnated.

## How it happens

- **Mutual Exclusion** - Only one process is allowed to access a unit of a resource.
- **Hold and Wait** - A situation must occur, where a process is holding a resource, and is waiting for more to release from other processes.
- **No Preemption** - A resource cannot be forcibly revoked from a process, it must surrender it on its own.
- **Circular Wait** - There is a set of processes where A waits for B, B waits for C, and so on, until the last process of the set is waiting for A.

## Resource Allocation Graph

Resource Allocation Graphs are directed, unweighted graphs that allow us to identify potential deadlocks both mathematically, and visually. There are two types of vertices: process vertices ($P$), and resource vertices ($R$) (which could be grouped if multiple instances are accessible). A **request edge** is an edge $P \rightarrow R$, and an **assignment edge** is an edge $R \rightarrow P$.

If there are no cycles in a resource allocation graph, there can never be a deadlock. But, even if there is a cycle, this is not sufficient evidence to show that a deadlock can occur.

(Hopefully some examples tomorrow...)

## Deadlock Detection Algorithm

- Create three table of $|P| \times |R|$ size, each called "Allocation", "Currently Available" and "Request".
  - The Allocation table contains all resource allocations that are granted to each process (given an allocation graph, for example). This may be resources that the processes immediately call `lock()` for.
  - The Request table contains all requests processes are making for other resources. This may be resources that the processes later request using `lock()`.
  - The Currently Available table contains all resources that have not been allocated.
- Additionally track boolean array "Finish" of size $|P|$. Set all values as false.
- Repeat until all elements in the Finish array are true:
  - Find a process $P_i$ in the Request table, where no resources are being requested. Set Finish[$i$] to true.
  - Add the values of the Allocation of $P_i$ to the table of Currently Available resources, then set the values of the allocation to 0 (i.e. $P_i$ surrenders its resources).
  - Check the Currently Available resources table to see if there is a process $P_j$ whose requests can be satisfied.
  - Subtract the requested resources from what is Currently Available, and add it to $P_j$'s current allocation.
- If at any point we cannot proceed, we have encountered a deadlock.

## Preventing Deadlocks

A lot of the time, we can stop deadlocks from happening by planning ahead and making good design choices. We will refer back to the necessary conditions for a deadlock.

We cannot stop *mutual exclusion*, as this is also a property of an ideal system using synchronisation. However, we can prevent *Hold and Wait* and *No Preemption* by ensuring that when a process requests resources, it does not currently hold any resources. If a process requests resources while it holds others, and such resources cannot be immediately allocated, then it should drop the resources it currently has. Furthermore, we can prevent *Circular Waits* by, for a numbering of processes 1 to $n$, ensuring that where process holding resource $i$ requests a resource $j$ held by another process, $i > j$.

It's important to remember that deadlock prevention is *restrictive*, as it relies on certain criteria being met, and may not allow some programs to operate in their intended manner. Furthermore, techniques such as prevention of circular wait may block a large number of harmless accesses to resources.