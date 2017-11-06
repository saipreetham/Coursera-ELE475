## Problem 1

> Compute	the	Clocks	Per	Instruction	(CPI)	of a machine which has an	average	CPI	for	ALU	operations of 1.1, a CPI for branches/jumps of 3.0, and	a	hit	rate of 60% in the	cache. A hit in the cache	takes	1	cycle	pipelined	and	a	cache	miss takes	120	cycles. Assume 22% of instructions are loads, 12% are	stores,	20%	are	branches/jumps and the balance are ALU operations.

Let's list the information:

- 22% are loads and 12% are store, that is, memory operations. From these, 60% are cache hits (1.0 CPI) and 40% are cache misses (120.0 CPI);
- 20% are branches/jumps, each with CPI of 3.0;
- 46% are ALUs, with CPI 1.1.

Then, we have:

![Problem 1 CPI resolution](https://github.com/MarcioJales/Coursera-ELE475/blob/master/Problem-1.png)

## Problem 2

> You	are	a	processor	designer	and	have	to	make	a	decision	between	building	a	processor	which	executes	at	1GHz	and	has	an	average	CPI	1.2	and	a	processor	which	executes	at	2GHz,	but	has	a	CPI	of	2. Which	is	better	to	build	and	why?

The measure we want is "Time per Instruction". What we have is:

- CPI, measured in "Cycles per Instruction";
- and Clock Rate, measured in "Cycles per Time".

What we need, then, is to divide CPI by the Clock:

1. 1.2 CPI and 1 GHz yields to 1.2 nanoseconds for each instruction.
2. 2 CPI and 2 GHZ yields to 1.0 nanoseconds for each instruction.

We decide for the second design, assuming the same number of instructions for a program.

## Problem 3

> (Page	C-82	in	H&P5,	Problem	C.1	a,b,c,d,e,f,g) Use the following code fragment:

![C-82-P1](https://github.com/MarcioJales/Coursera-ELE475/blob/master/c82-1.jpg)

Figure C-5:

![Figure-C-5](https://github.com/MarcioJales/Coursera-ELE475/blob/master/figure-c-5.jpg)

Figure C-6:

![Figure-C-6](https://github.com/MarcioJales/Coursera-ELE475/blob/master/figure-c-6.jpg)

### a)

> Data hazards are caused by data dependences in the code. Whether a dependency causes a hazard depends on the machine implementation (i.e., number of pipeline stages). List all of the data dependences in the code above. Record the register, source instruction, and destination instruction; for example, there is a data dependency for register R1 from the LD to the DADDI.

- The second instruction `DADDI` reads `R1`. Then, it depends on the result of the first instruction `LD`, which writes to `R1`;
- Third instruction `SD` depends on the previous `DADDI`, since this one writes to `R1` and the later stores this value at the address pointed to by `R2`.  
- Fifth instruction `DSUB`reads from `R2` and, then, depends on the fourth `DADDI`, which writes to it.
- Sixth instruction `BNEZ` needs to compare the value of `R4`, which is written in the previous `DSUB`. That is, another dependency.
- Since it is a loop, `LD` depends on the second `DADDI`, so that `SD`. Both uses values written to `R2`, which happens at the second `DADDI`.

| Register | Source instruction | Consumer instruction |
| -------- | -------- | -------- |     
| R1 | LD | DADDI (1) |
| R1 | DADDI (1) | SD |
| R2 | DADDI (2) | DSUB |
| R4 | DSUB | BNEZ |
| R2 | DADDI (2) | LD |
| R2 | DADDI (2) | SD |

## b)

> Show the timing of this instruction sequence for the 5-stage RISC pipeline without any forwarding or bypassing hardware but assuming that a register read and a write in the same clock cycle “forwards” through the register file, as shown in Figure C.6. Use a pipeline timing chart like that in Figure C.5. Assume that the branch is handled by flushing the pipeline. If all memory references take 1 cycle, how many cycles does this loop take to execute?

Every decode stage may proceed only when the proper write-back on which it depends has been performed:

![PS1-3B](https://github.com/MarcioJales/Coursera-ELE475/blob/master/ps1-3b.png)
