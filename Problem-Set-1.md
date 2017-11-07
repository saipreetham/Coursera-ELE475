## Problem 1

> Compute	the	Clocks	Per	Instruction	(CPI)	of a machine which has an	average	CPI	for	ALU	operations of 1.1, a CPI for branches/jumps of 3.0, and	a	hit	rate of 60% in the	cache. A hit in the cache	takes	1	cycle	pipelined	and	a	cache	miss takes	120	cycles. Assume 22% of instructions are loads, 12% are	stores,	20%	are	branches/jumps and the balance are ALU operations.

Let's list the information:

- 22% are loads and 12% are store, that is, memory operations. From these, 60% are cache hits (1.0 CPI) and 40% are cache misses (120.0 CPI);
- 20% are branches/jumps, each with CPI of 3.0;
- 46% are ALUs, with CPI 1.1.

Then, we have:

![Problem-1](https://github.com/MarcioJales/Coursera-ELE475/blob/master/problem-1.png)

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

There is a error on the "store" instruction. Instead of `SD R1,0,(R2)`, it is `SD R1,0(R2)`, since we deal with a I-type instruction.

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

### b)

> Show the timing of this instruction sequence for the 5-stage RISC pipeline without any forwarding or bypassing hardware but assuming that a register read and a write in the same clock cycle “forwards” through the register file, as shown in Figure C.6. Use a pipeline timing chart like that in Figure C.5. Assume that the branch is handled by flushing the pipeline. If all memory references take 1 cycle, how many cycles does this loop take to execute?

We assume that the loop will continue when reach the end for this case and letters "c" and "d".

Every decode stage may proceed only when the proper write-back on which it depends has been performed. It important to notice the structural hazard as well. Since we simply flush the pipeline to solve the branch, The `LD` for the next iteration will stall until the `ID` stage of `BNEZ`, where finally the processor finds out where to go (considering the MIPS microarchitecture).

The loop takes 18 cycles to execute:

![Problem-3b](https://github.com/MarcioJales/Coursera-ELE475/blob/master/problem-3b.png)

### c)

> Show the timing of this instruction sequence for the 5-stage RISC pipeline with full forwarding and bypassing hardware. Use a pipeline timing chart like that shown in Figure C.5. Assume that the branch is handled by predicting it as not taken. If all memory references take 1 cycle, how many cycles does this loop take to execute?

For this case, we also have some structural hazards. Now, dependencies on I-type mat proceed after its `MEM` stage. For instance, the second `DADDI` executes only when `LD` reads the memory to set `R1`. Also, dependencies on R-type instructions may proceed after its `EX` stage.

We predict that branch is not taken. The next fetched instruction (omitted from the chart) will be flushed and the `LD` will be placed after we decode the `BNEZ` command.

The loop takes 11 cycles to execute:

![Problem-3c](https://github.com/MarcioJales/Coursera-ELE475/blob/master/problem-3c.png)

### d)

Almost the same case as "c", but the "taken" prediction does the `LD` to be the next instruction in the pipeline.

![Problem-3d](https://github.com/MarcioJales/Coursera-ELE475/blob/master/problem-3d.png)
