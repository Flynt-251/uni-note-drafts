# Pipelining

Let's say you're at home, and you have a few different things that need doing. You need to study for an upcoming exam (how meta), do your laundry, and cook some food for the week. You *could* do one task at a time, waiting for the laundry to finish so you can put it up to dry, then prepare the food and wait for it to cook, then go and study. However, there's a lot of dead time doing that. So *instead*, what you do is put your laundry on, and as the machine does its work, you go and get started on cooking. Once you've started letting it simmer/roast/bake, your hands are then free to study. You'll empty the washing machine out once it's done, and cool the food off once it's finished cooking. This is the basic concept of **pipelining**.

To translate this into computer terms, recall that a CPU is split into different units, such as an ALU, a CU and registers. So while some data is being calculated in the ALU, it makes sense for the CPU to also fetch a new instruction as it's calculating. Unlike humans, computers are very good at multitasking. We split each step of an instruction, such that each step can occur at the same time: each instruction being carried out must also switch to a new step at the same time, and often the timing of this is determined by the slowest stage. This is called a **processor cycle**.

We can measure the performance of pipelining based on the **throughput**, determined by the frequency at which an instruction finished executing, and we want to design a pipeline that ensures that the length of each step is as equal as possible, i.e. as close as we can get to splitting the overall instruction time into equal time splits.

## Table-side service

*Imagine how much better life would be if we all just had drink fountains installed into our tables*

We've seen the Fetch-Decode-Execute cycle before, but have you seen the IF-ID-EX-MEM-WB cycle before? No? Probably because it's far less catchy. But this describes a common architecture of using a five-stage pipeline, where instructions are broken down into each of these steps:

- **Instruction Fetch (IF)** - Get the value of the PC to fetch the instruction from memory, and increment the PC.
- **Instruction Decode (ID)** - Find out what the instruction is saying, get data from the necessary registers and compute possible branches. These are all done in parallel as they use different parts of the processor.
- **Execution/Effective address cycle (EX)** - Perform arithmetic and bitwise operations on data on the ALU.
- **Memory Access (MEM)** - If we're loading from a calculated memory address, fetch the data, or if we're storing to a location, write to the register file.
- **Write-back (WB)** - Output the results of the operation to the register file, whether this was a memory access or a calculation.

We may place these steps and instructions into a table to visualise how instructions are carried out.

|Inst.| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|-----|---|---|---|---|---|---|---|---|---|
|  1  |IF |ID |EX |MEM|WB |   |   |   |   |
|  2  |   |IF |ID |EX |MEM|WB |   |   |   |
|  3  |   |   |IF |ID |EX |MEM|WB |   |   |
|  4  |   |   |   |IF |ID |EX |MEM|WB |   |
|  5  |   |   |   |   |IF |ID |EX |MEM|WB |

## Things that might bust a pipe

*Please don't.*

Of course, being able to do multiple things at the same time is great, but we need to be weary, as if we're not careful, things can go awry quickly. We need to make sure the pipeline is **executing instructions correctly, keeping them moving, and ensuring the CPU is always busy**! Namely, we need to keep an eye on...

- **Pipeline Latency** - If instruction duration isn't changing, then pipelining is having a more limited effect.
- **Stage time imbalance** - If one stage is taking exceptionally longer than the others, this stalls the clock as a whole, creating an overhead.
- **Pipelining Overhead** - There's some additional delay in managing registers and clock skew, this should be mitigated as much as possible.

### Cycle Time

$\tau = \max_i[\tau_i] + d = \tau_m + d$ where $1 \leq i \leq k$

Cycle time determines how long it takes the processor to advance all instructions to their next step, where each term is...

- $\tau_i$ - Time delay on the $i$th instruction
- $\tau_m$ - Maximum delay of all stages
- $k$ - Number of stages, so five for our example.
- $d$ - Time delay of latch, i.e. the pipeline overhead.

So to put it simply, just get the time of the stage that takes the longest, and then add on the latency of switching over to the next step to get the cycle time.

### Time to execute $n$ instructions

If we have a $k$-stage pipeline, and are executing $n$ instructions, then the time to complete them all is represented as $T_{k,n}$ and is calculated as such:

$T_{k,n} = (k + (n-1)) \times \tau$

Where $\tau$ is the cycle time. So, if we were executing 5 instructions on a 5-stage pipeline, where $\tau = 200 \text{ns}$, this would take 1.8 milliseconds, or $9 \tau$.

To see the speedup compared to purely sequential execution, we use $T_{1,n} = nk\tau$.

$S_k\frac{nk\tau}{(k + (n-1)) \tau}$

So for our example, we can expect a speedup of ~2.78.

## Slippy when wet

We may encounter **pipeline hazards** on occasion, which are instances where we have to stall the pipeline because we've hit a condition where we need to wait for something to finish before we can proceed. There are three types of hazards: Resource (also called Structural), Control and Data hazards.

### Why can't we all just get along?

A **Resource Hazard** is where two or more instructions attempt to access a resource at the same time, i.e. a resource conflict. This could happen when two resources use the same memory address or register. This can be a problem as it could cause a race condition, throwing our execution out of order and potentially making things unstable. So, we should allow the earlier instruction to perform its action first, then allow the next.

#### Wait a minute...

Let's look at this pipeline table again. Suppose instruction 1 writes to the same address as instruction 4 fetches from. This is of course a resource conflict, and bad things could happen!

|Inst.| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|-----|---|---|---|---|---|---|---|---|---|
|  **1**  |IF |ID |EX |**MEM**|WB |   |   |   |   |
|  2  |   |IF |ID |EX |MEM|WB |   |   |   |
|  3  |   |   |IF |ID |EX |MEM|WB |   |   |
|  **4** |   |   |   |**IF** |ID |EX |MEM|WB |   |
|  5  |   |   |   |   |IF |ID |EX |MEM|WB |

In this case, we need to see what's causing the issue, then *stall* the later instruction, like this:

|Inst.| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|-----|---|---|---|---|---|---|---|---|---|
|**1**|IF |ID |EX |MEM|WB |   |   |   |   |
|  2  |   |IF |ID |EX |MEM|WB |   |   |   |
|  3  |   |   |IF |ID |EX |MEM|WB |   |   |
|**4**|   |   |   |*Stall*|IF |ID |EX |MEM|WB    |
|  5  |   |   |   |   |   |IF |ID |EX |MEM|

This stall is sometimes called a **bubble**, since it's an unhelpful gap in the pipeline, like air bubbles in a water pipe.

We could also prevent structural hazards by **changing the scheduling of instructions**, which would be a responsibility of the programmer, or we could **add hardware** which allows multiple instructions to access the same resource at the same time.

### Who took my CTRL keys?

A **Control Hazard**  describes what happens when we branch and jump to a new portion of memory, it essentially invalidates all of the instructions we've just queued. So if a branch required us to run `subi` instead of `addi`, here's what would happen in our pipeline:

|Inst.| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-----|---|---|---|---|---|---|---|---|
|addi |IF |ID |EX |MEM|WB |   |   |   |
| **beq** |   |IF |ID |EX |MEM|WB |   |   |
|addi |   |   |**!** |ID |EX |MEM|WB |   |

Here, we've just noticed that this is the wrong instruction. We now need to quickly swap in the new instruction.

|Inst.| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-----|---|---|---|---|---|---|---|---|
|addi |IF |ID |EX |MEM|WB |   |   |   |
| **beq** |   |IF |ID |EX |MEM|WB |   |   |
|~~addi~~ |   |   |IF | idle |idle |idle|idle |   |
|addi |   |   |   | IF |ID |EX |MEM|WB |   |

So now we've created some dead time where we can't actually do any work. Of course, it would be nice if we could just get rid of branches (I mean, with some clever programming you could), but branching is an integral part of programming, so we're a bit stuck here. Instead, here's what we can do to work with them:

- **Use Multiple Streams** - Fetch *both* instructions at the same time, swapping in the one we need. This may not be ideal as this could contend with other instructions, and multiple branches could clog registers up.
- **Prefetch the Branch Target** - If we come across a branch, fetch its target as well as the next instruction.
- **Loop Buffer** - Use a small amount of high-speed memory to store the $n$ most frequently accessed instructions in sequence, similar to an instruction cache. Also well-suited to looping instructions.
- **Branch Prediction** - Can be done via a few different techniques: static approaches such as always or never taking the branch, or using the opcode, or dynamic approaches, alternating or using a history table, which both use previously executed code to make a decision.
- **Delayed Branch** - Do nothing (see above).

### My precious precious DATA!

**Data Hazards** are instances where two nearby instructions conflict because one reads a value, and the other writes to it. This can cause unintended behaviour and completely throw off our execution. For instance, if we have the two following instructions, which are sequentially next to each other:

```
0x0...0: r3 = r1 + r2;
0x0...4: r4 = r3 - r5;
```

Then the second instruction will have to stall at the EX step to wait for the correct value of `r3` to be computed. Fixing this sort of problem can be more difficult, as this pattern is very common.

- **Schedule** - Simply *don't do data hazards* and be a better programmer, just reschedule these instructions.
- **Stall** - Wait for the first instruction to finish execution, then run the next.
- **Bypass** - Directly send the calculated value to the next instruction, while the first one is still in the pipeline.
- **Speculate** - Assume a problem will not occur, otherwise stop the second instruction and try again.

#### rawr

There are a few different *subtypes* of data hazard.

**Write After Read (WAR)**

```
read x;
write x + 1;
```

Not possible in the given 5-stage pipeline, but is possible on other types.

**Read After Write (RAW)**

```
write x + 1;
read x;
```

This creates a **flow dependence**. We can remove these by swapping the instructions, creating an **anti dependence**.

**Write After Write (WAW)**

```
write x + 1;
write x * 2;
```

Again, cannot occur in the 5-stage pipeline, but may occur in other types of pipeline.

## Sprinkling in more enhancements

**Data Forwarding** - Add lines between CPU portions to directly send data outputs to the next stage immediately. We may forward data to the execution stage after completing the memory or write-back stage, for example.

**Instruction and Data Caches** - Separate operators from their operands, removing fetch and execution conflicts.

**Separated Execution Units** - Create multiple execution units with different delays, so that we can purposefully "stall" instructions to prevent hazards.

**Reservation Station** - Relieves a bottleneck in operand fetching, by using a buffer which can load a new instruction into the processor once the relevant functional units are available and hazards have been dealt with.