## Problem 1

> Compute	the	Clocks	Per	Instruction	(CPI)	of a machine which has an	average	CPI	for	ALU	operations of 1.1, a CPI for branches/jumps of 3.0, and	a	hit	rate of 60% in the	cache. A hit in the cache	takes	1	cycle	pipelined	and	a	cache	miss takes	120	cycles. Assume 22% of instructions are loads, 12% are	stores,	20%	are	branches/jumps and the balance are ALU operations.

Let's list the information:

- 22% are loads and 12% are store, that is, memory operations. From these, 60% are cache hits (1.0 CPI) and 40% are cache misses (120.0 CPI);
- 20% are branches/jumps, each with CPI of 3.0;
- 46% are ALUs, with CPI 1.1.

Then, we have:

![Problem 1 CPI resolution](https://github.com/MarcioJales/Coursera-ELE475/blob/master/Problem-1.png)

## Problem 2

The measure we want is "Time per Instruction". What we have is:

- CPI, measured in "Cycles per Instruction";
- and Clock Rate, measured in "Cycles per Time".

What we need, then, is to divide CPI by the Clock:

1. 1.2 CPI and 1 GHz yields to 1.2 nanoseconds for each instruction.
2. 2 CPI and 2 GHZ yields to 1.0 nanoseconds for each instruction.

We decide for the second design, assuming the same number of instructions for a program.
