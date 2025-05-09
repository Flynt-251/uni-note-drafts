# 2 - Memory

We know that, at a basic level, the CPU processes data and runs instructions, and the memory holds instructions that are to be ran by the CPU. In this section we look at how memory has been optimised to be as efficient as possible.

## Everything in its right place

We can define five types of memory: registers, cache, main memory, SSDs and magnetic media. In this order, these decrease in speed but additionally decrease in cost, i.e. at each level, the storage becomes slower to access, but becomes less expensive at higher capacities. For instance, a gigabyte of cache would be significantly more expensive than a gigabyte hard drive. To get the most out of our system, *we want to balance speed and cost*. In an ideal world, we would have infinite instant-access memory, but no such thing exists, so this **Memory Hierarchy** exists as a compromise.

- Registers
- Cache
- Main memory
- Solid State
- Magnetic Media

Having this hierarchy allows us to store information that will be accessed frequently. More specifically, it ensures we take advantage of **Temporal locality**, which states that the same memory location is likely to be accessed multiple times, and **Spatial locality**, which states that neighbouring locations are likely to be accessed within close time proximity. In fact, we can generally presume that 90% of memory access are within 2KB of the last access.

## Chrome has entered the chat

Yes, we're first talking about RAM. Or rather, **solid state devices**. This basically means anything that doesn't whir, click or spin. These devices are built up of many semiconductors, and come in the following flavours:

- **Random Access** (RAM) - Read-writable and volatile.
- **Read-only** (ROM) - As it says on the tin.
- **Programmable ROM** (PROM) - Like ROM, but written electrically, not via a mask.
- **Erasable PROM** (EPROM) - Can be rewritten all at once.
- **Electrically Erasable PROM** (EEPROM) - Can be rewritten at byte level.
- **Flash Memory** - Rewritable at block level.

### RAM it into the motherboard

You may have heard of RAM in different contexts, including SRAM, DRAM, DDR4, and so on. If you're thinking of sheep, you're in the wrong place. A memory cell can hold one bit of data, and has three lines: Read-Write, Select and Data. These each tell the cell to either report its value, or overwrite it, enable it to do these things, send out/receive the value respectively. **DRAM** uses a single transistor and capacitor for each cell, and needs to be refreshed periodically. This is why you may see a clock speed on this type of RAM. By contrast, **SRAM** uses two capacitors and four transistors to store a single bit, but does not need refreshing. As you can imagine, SRAM is the more expensive option of the two.

### Boxes of boxes

So, we have these memory cells. how do we divvy them up? Well, we can arrange them in a 2D grid, so that each row is called a *word*, and the number of columns is the *word size*. We then use some additional circuitry to allow us to fetch all of the values of a single row, by decoding an $N$ bit value to give us the value of one of $2^N$ words. $N$ is the number of **address** lines we are using. If we have word length $W$, then the number of memory cells we are storing is equal to $2^N \times W$. For example, if we have 4 address lines, and each word is 8 bits wide (1 byte), we are using 128 memory cells. We *could* alternatively use 7 address lines and have a word length of 1 bit, but then we need to use a decoder that takes in 7 address lines, which is not only quite awkward, but an inefficient use of space. We can further minimise the number of lines we need by using multiplexers, such that each row has multiple words, and we have some additional address lines which specify what word to output.

### A few offshore accounts

**Interleaved memory** is where we utilise multiple **banks** of memory in parallel, so data is split up across $N$ banks, meaning we can respond to $N$ data access at once. This is otherwise known as N-way interleaving. Interleaving is particularly effective when the number of receiving caches we have is a multiple of the number of banks we have.

### Why does everyone keep talking about rhythm games?

**SDRAM**, or Synchronous DRAM uses multiple banks of RAM, and exchanges data with the processor, using the timing of a clock signal close to that of the CPU. This essentially acts like a pipeline, since the CPU has to wait for a read/write to go through, hence SDRAM enables better utilisation of the memory overall. Each time, data is sent on the rising edge of each clock signal, known as **SDR**. But what if we also sent data on the falling edge? Well, congrats, you've just discovered **DDR** memory, which you'll have heard of if you've ever put together your own PC. Because we're now sending out double the rate of data, we use an additional **prefetch buffer** to hold it before we send it out. 

### What's with all this dynamic typing nowadays?

Okay great, so now we're up to date as to how DRAM is used, but where does SRAM fit into this? Remember that SRAM is more expensive than DRAM, but less complicated to work with, so we usually use SRAM in cache, which comes higher up on the memory hierarchy.

## How quickly are we forgiving and forgetting?

Having a hierarchy in place is good, but it can always be unbalanced, which can lower performance, give us low space to work with, or worst of all, drain our pockets unnecessarily. Keep in mind the following properties for cost, access time and capacity for each level $i$ and its parent.

- **Cost** - The parent is always cheaper per unit, i.e. $C_i > C_{i+1}$
- **Access time** - The parent is always slower, i.e. $t_{Ai} < t_{Ai + 1}$
- **Capacity** - The parent always has larger capacity, i.e. $S_i < S_{i+1}$

Performance is also dependent on the following factors:

- How are addresses being referenced? Lots of sequential accesses or random?
- How big is each block we move between levels?
- How do we allocate space? How does the algorithm swap out data?

### Average Cost

$C_S = \frac{C_1 S_1 + C_2 S_2}{C_1 + C_2}$

We aim to make $C_S$ approach $C_2$. Since we know that $C_1 > C_2$, we should use the fact $S_1 < S_2$.

### Hit me

Let $H < 1$ be the hit ratio. The average access time between two layers is:

$t_{average} = Ht_1 + (1 - H) (t_1 + t_2)$

### Access Efficiency

Let $r = \frac{t_2}{t_1}$, and access efficiency $e = \frac{t_1}{t_{average}}$

Hence $e = \frac{t_1}{1 + (1 - H)r}$

We aim to have $e$ approach 1, i.e. get as close to 100% efficiency as possible.

### Give me some space

Utilisation $u = \frac{S_u}{S}$

Where $S$ is total capacity, and $S_u$ is occupied memory. We want to ensure as much utilisation as possible, without increasing miss ratio.

Any 'wasted' space may be due to fragmentation, inactive regions of data that are never used by the CPU, and system overhead.