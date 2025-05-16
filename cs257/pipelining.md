# Pipelining

Let's say you're at home, and you have a few different things that need doing. You need to study for an upcoming exam (how meta), do your laundry, and cook some food for the week. You *could* do one task at a time, waiting for the laundry to finish so you can put it up to dry, then prepare the food and wait for it to cook, then go and study. However, there's a lot of dead time doing that. So *instead*, what you do is put your laundry on, and as the machine does its work, you go and get started on cooking. Once you've started letting it simmer/roast/bake, your hands are then free to study. You'll empty the washing machine out once it's done, and cool the food off once it's finished cooking. This is the basic concept of **pipelining**.

To translate this into computer terms, recall that a CPU is split into different units, such as an ALU, a CU and registers. So while some data is being calculated in the ALU, it makes sense for the CPU to also fetch a new instruction as it's calculating. Unlike humans, computers are very good at multitasking. We split each step of an instruction, such that each step can occur at the same time: each instruction being carried out must also switch to a new step at the same time, and often the timing of this is determined by the slowest stage. This is called a **processor cycle**.

We can measure the performance of pipelining based on the **throughput**, determined by the frequency at which an instruction finished executing, and we want to design a pipeline that ensures that the length of each step is as equal as possible, i.e. as close as we can get to splitting the overall instruction time into equal time splits.

## Table-side service

*Imagine how much better life would be if we all just had drink fountains installed into our tables*

We've seen the Fetch-Decode-Execute cycle before, but have you seen the IF-ID-EX-MEM-WB cycle before? Probably because it's far less catchy. But this describes a common architecture of using a five-stage pipeline, where instructions are broken down into each of these steps:

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
|**4**|   |   |   |Stall|IF |ID |EX |MEM|WB    |
|  5  |   |   |   |   |   |IF |ID |EX |MEM|

This stall is sometimes called a **bubble**, since it's an unhelpful gap in the pipeline, like air bubbles in a water pipe.

We could also prevent structural hazards by **changing the scheduling of instructions**, which would be a responsibility of the programmer, or we could **add hardware** which allows multiple instructions to access the same resource at the same time.

### Who took my CTRL keys?

