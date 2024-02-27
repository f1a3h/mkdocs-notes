---
title: UCB CS61C
date: 2024-01-21T23:42:00
draft: false
math: true
description: Six Great Ideas in Computer Architecture
categories:
  - Study Notes
  - Computer Architecture
tags:
  - Computer-Architecture
---

> [!info] 
> Study notes based on UCB CS61C, [Summer 2020](https://inst.eecs.berkeley.edu/~cs61c/su20/) & [Fall 2020](https://inst.eecs.berkeley.edu/~cs61c/fa20/) online videos along with its textbooks.


1. Abstraction
2. Technology Trends
3. Principles of Locality/Memory Hierarchy
4. Parallelism
5. Performance Measurement & Improvement
6. Dependability via Redundancy

# Great Idea \#1: Levels of Representation & Interpretation

![image.png](https://fastly.jsdelivr.net/gh/f1a3h/imgs/202401302014701.png)

### Number Representation

[lec01.pdf](https://inst.eecs.berkeley.edu/~cs61c/su20/pdfs/lectures/lec01.pdf)

- Number Bases
	We use $d\times Base^i$ to represent a number, and in computer the $Base=2$ . Thus, we can also use bits to represent numbers in computer.
- Signed Representations
	- Sign and Magnitude
	- Biased Notation
	- One's Complement
	- Two's Complement
- Overflow
	- most significant bit (MSB): the leftmost bit
	- least significant bit (LSB): the rightmost bit
	- overflow: the proper result cannot be represented by the hardware bits
- Sign Extension
	- Sign and Magnitude: fill with 0's
	- One/Two's Complement: fill with MSB


??? note "Sidenote"
	
	The RISC-V word is 32 bits long.
	
	Quick computation for $-x$: $x+\bar{x}=-1$ .
	
	Foundation of complement method:
		Consider subtracting a large number from a small one, the answer is that it would try to borrow a string of leading 0s, so the result would have a string of 1s.
		This gives us the idea that leading 0s means positive, and leading 1s means negative.
		
	See [[Computer Science/Computer Architecture/CSAPP/Chapter 2|CS:APP Chapter 2]] for more.

## High-Level Language Program (e.g. C)

### C: Introduction

#### Basic Concepts

[lec02.pdf](https://inst.eecs.berkeley.edu/~cs61c/su20/pdfs/lectures/lec02.pdf)

- Compilation
	- Comparison
		- C: compilers map C programs into architecture-specific machine code
		- Java: converts to architecture-independent bytecode (run by JVM)
		- Python: directly interprets the code
	- Advantage
		- Excellent runtime performance: compiler optimizes the code for the given architecture
		- Fair compilation time: only modified files need recompilation
	- Disadvantage
		- Compiled files are architecture-specific
		- "Edit $\to$ Compile $\to$ Run \[repeat\]" iteration cycle can be slow
- Variable Types

#### C Memory Management

[lec04.pdf](https://inst.eecs.berkeley.edu/~cs61c/su20/pdfs/lectures/lec04.pdf)

The picture below shows the C memory layout.

<p align="center">
<img src="https://fastly.jsdelivr.net/gh/f1a3h/imgs/202401242323345.png" height=250px>
</p>

From top to down, we have:

- Stack
	- local variables, grows downward
	- consists of procedure frames, uses a stack pointer (SP) to tell where the current stack frame is
- Heap
	- space requested via `malloc()` and used with pointers
	- resizes dynamically, grows upward
- Static Data
	- global and static variables, does not grow or shrink
		- *string literals* $\in$ static data
			- `char * str = "hi";` belongs to, but `char str[] = "hi";` does not
	- size does not change, but sometimes data can
		- string literals cannot
- Code
	- loaded when program starts, does not change
	- typically read only


When it comes to memory management in C, there can be tons of memory problems. For a better understanding of that, refer to [[Stanford CS110L|CS11L]].

??? note "Basic Allocation Strategy: K&R Section 8.7"
	
    Each continuous spaces allocated by calling `malloc()` is called a block.
    
    To manage those blocks, we use a linked-list with each node pointing to each block.
    
    When a `free()` is called, we insert that block into the list and combine it with adjacent blocks.
    
    When a `malloc()` is called, we first search for the blocks large enough for allocation, then choose one using a method offered below:
    
    - Best-fit: choose the smallest, this provides the least concentrative distribution of small blocks yet a slowest service.
    - First-fit: choose the block with least order, this is fast but tends to concentrate small blocks at beginning.
    - Next-fit: like first-fit, but resume search from where we last left off. Also fast, and it does not concentrate small blocks at front.

### Floating Point

See [[Computer Science/Computer Architecture/CSAPP/Chapter 2#Floating Point|CS:APP Ch.2 Floating Point]].

#### Single Precision

Use normalized, base 2 scientific representation:

![](https://fastly.jsdelivr.net/gh/f1a3h/imgs/20240126000027.png)

Split a 32-bit word into 3 fields:

![](https://fastly.jsdelivr.net/gh/f1a3h/imgs/20240126000148.png)

Here, we use biased notation ($bias=-127$) for the exponent in order to preserve the linearity of value as well as represent a small number.

#### Double Precision

![](https://fastly.jsdelivr.net/gh/f1a3h/imgs/20240126000842.png)

Everything stays the same as Single Precision except the length of each field.

#### Special Cases

![](https://fastly.jsdelivr.net/gh/f1a3h/imgs/20240126001219.png)

#### Limitations

- Overflow & Underflow
- Rounding occurs when result runs out of the end of the Significant
- FP addition is not associative due to rounding errors
	- As a result, parallel execution strategies that work for integer data may not work for FP
- FP cannot represent all integers

## Assembly Language Program (e.g. RISC-V)
### RISC-V: Introduction

- Mainstream Instruction Set Architecture: x86, ARM architectures, RISC-V
- Complex/Reduced Instruction Set Computing:
	- Early trend: CISC
		- difficult to learn
		- less work for the compiler
		- complicated hardware runs more slowly
	- Later began to dominate: RISC
		- easier to build fast hardware
		- let software do the complicated operations by composing simpler ones

### Assembly Language

- RISC Design Principles: smaller is faster, keep it simple.
- RISC-V Registers: `s0-s11`, `t0-t6`, `x0`
- RISC-V Instructions
	- Arithmetic: `add`, `sub`, `addi`
	- Data Tansfer: `lw`, `sw`
		- Memory is byte-addressed
	- Control Flow: `beq`, `bne`, `j`
	- Six Steps of Calling a Function
		1. Place arguments
		2. Jump to funciton
		3. Create local storage (Prologue)
		4. Perform desired task
		5. Place return value and clean up storage (Epilogue)
		6. Jump back to caller
	- Calling Conventions
		- Caller-saved registers (Volatile Registers)
		- Callee-saved registers (Saved Registers)
- RISC-V Instruction Formats
	- ![](https://fastly.jsdelivr.net/gh/f1a3h/imgs/risc-v_instruction_format.png)
	- The Stored Program concept is very powerful.

## Machine Language Program (RISC-V)

### Translation vs. Interpretation

Interpreter: Directly executes a program in the source language.

Translator: Converts a program from the source language to an equivalent program in another language.

Translation vs. Interpretation:

- Interpreter is slower, but code is smaller
- Interpretor provides instruction set independence
- Translated/compiled code almost always more efficient and therefore higher performance
- Translation/compilation helps "hide" the program "source" from the users

### Translation

C translation steps are as follows:

![image.png](https://fastly.jsdelivr.net/gh/f1a3h/imgs/20240129162946.png)

#### Compiler

- Input: Higher-level language (HLL) code
- Output: Assembly language code (may contain pseudo-instructions)

#### Assembler

- Input: Assembly language code
- Output: Object code, information tables $\rightarrow$ Object file

Assembler does this in three steps:

- Reads and uses *directives*
	- Assembly directives: give directions to assembler, but do not produce machine instructions. (`.text`, `.data`, `.global sym`, etc)
- Replaces pseudo-instructions (`mv`, `li`, etc)
- Produces machine language
	- "Forward Reference" problem

To solve "Forward Reference" problem, we make two passes over the program.

- Pass 1:
	- Expands pseudo instructions
	- Remember positions of labels
	- Take out comments, empty lines, etc
	- Error checking
- Pass 2:
	- Use label positions to generate relative addresses
	- Outputs the object file

For jumps to external labels and references to data, assembler cannot determine yet, but it will create two tables for linker to solve that.

- Symbol Table
	- List of "items" that may be used by other files
	- Includes labels and data
- Relocation Table
	- List of "items" this file will need the address of later (currently undetermined)
	- Includes:
		- Any external label jumped to `jal` or `jalr`
		- Any piece of data

The object file format is listed below from top to down:

1. object file header
2. text segment
3. data segment
4. relocation table
5. symbol table
6. debugging information

#### Linker

- Input: Object code files, information tables
- Output: Executable code

There are three steps for the linker:

1. Place code and data modules symbolically in memory
2. Determine the addresses of data and instruction labels
3. Patch both the internal and external references

Note that the linker enables separate compilation of files, which saves a lot of compile-time.

> [!info] 
> 
>  The text segment starts at address `0000 0000 0040 0000`<sub>hex</sub> and the data segment at `0000 0000 1000 0000`<sub>hex</sub>.

Three types of addresses:

- PC-Relative Addressing (`beq`, `bne`, `jal`) $\rightarrow$ never relocate
- External Function Reference (usually `jal`) $\rightarrow$ always relocate
- Static Data Reference (often `auipc` and `addi`) $\rightarrow$ always relocate
	- `lw`, `sw` to variables in static area, relative to global pointer

#### Loader

- Input: Executable code
- Output: \<program is run\>

The loader follows these steps:

1. Reads executable file's header to determine size of text and data segments
2. Creates an address space large enough for text and data
3. Copies the instructions and data from executable file into memory
4. Copies arguments passed to the program onto the stack
5. Initialize machine registers
6. Jumps to start-up routine that copies program's arguments from stack to registers and sets the PC

!!! quote
	
    *Virtually every problem in computer science can be solved by another level of indirection.*
    <p align="right">David Wheeler</p>

## Logic Circuit Description

[lec10.pdf](https://inst.eecs.berkeley.edu/~cs61c/su20/pdfs/lectures/lec10.pdf)

Synchronous Digital Systems (SDS):

- Synchronous
	- All operations coordinated by a central clock
- Digital
	- Represent all values with two discrete values: 0/1, high/low, true/false 

### Switches and Transistors

- Switches
	- The basic elements of physical implementations
- Transistors $\rightarrow$ hardware switches
	- Modern digital systems designed in CMOS
		- [Understanding CMOS](https://www.bilibili.com/video/BV1GD4y1C7K5)
	- MOS transistors acts as voltage-controlled switches

### CMOS Networks

![image.png](https://fastly.jsdelivr.net/gh/f1a3h/imgs/202401302320718.png)

- In a word,
	- high voltage $\rightarrow$ N-channel ON, P-channel OFF
	- low voltage $\rightarrow$ N-channel OFF, P-channel ON
- V<sub>GS</sub> = V<sub>Gate</sub> - V<sub>Source</sub>; V<sub>SG</sub> = V<sub>Source</sub> - V<sub>Gate</sub>; V<sub>TH</sub> = V<sub>Threshold</sub>

In abstraction, we can simply view chips composed of transistors and wires as block diagrams.
### Combinational Digital Logic

#### Combinational Logic Gates

Digital Systems consists of two basic types of circuits:

- Combinational Logic (CL) e.g. ALUs
- Sequential Logic (SL) e.g. Registers

<figure class="half">
	<img src="https://fastly.jsdelivr.net/gh/f1a3h/imgs/202401302342969.png" width=250px >
	<img src="https://fastly.jsdelivr.net/gh/f1a3h/imgs/202401302343742.png" width=250px >
</figure>

#### Boolean Algebra

Translate truth table to boolean algebra:

- Sum of Products (SoP)
- Product of Sums (PoS)

#### Circuit Simplification

Hardware can have delays, so we should simplify boolean expressions $\rightarrow$ smaller transistor networks $\rightarrow$ smaller circuit delays $\rightarrow$ faster hardware.

!!! note "Summary" 
	
    <img src="https://fastly.jsdelivr.net/gh/f1a3h/imgs/202401302349229.png" height=350px>

### Sequential Digital Logic

[lec11.pdf](https://inst.eecs.berkeley.edu/~cs61c/su20/pdfs/lectures/lec11.pdf)

[State Handout](https://inst.eecs.berkeley.edu/~cs61c/sp21/resources-pdfs/state.pdf)

In combinational digital logic, systems are memoryless. However, when we want to sum up a series of numbers without knowing its length, this is impossible to implement with memoryless circuits. Thus, we now introduce registers.

#### Register Details

![image.png](https://fastly.jsdelivr.net/gh/f1a3h/imgs/202402011759647.png)

Inside a register are a number of *flip-flops* (FFs). Here we're talking about the most common type of flip-flops, called an *edge-triggered d-type flip-flop*.

<p align="center">
<img src="https://fastly.jsdelivr.net/gh/f1a3h/imgs/202402011802633.png" height=125px>
</p>

Take a look at a single FF, here `d` is referred as input and `q` as output.

On the rising edge of the clock, the input `d` is sampled and transferred to the output `q`. At other times, `d` is simply ignored.

When a FF feels that there is a change at the input side, it knows that it maybe time to change. However, just like combinational logic circuits, FF cannot change its value instantaneously. Also, time is needed to transfer inputs internally. The combination of these (the former one called *setup time*, and the latter called *hold time*) two create a time window when the input `d` cannot change. Also, after the FF has successfully captured the new input, it takes a small amount of time to transfer that to the output. This delay is called *clk-to-q delay*.

![image.png](https://fastly.jsdelivr.net/gh/f1a3h/imgs/202402011818177.png)

#### Pipelining--Adding Registers to Improve Performance

Consider the example in the slides, we can infer that the *maximum clock frequency* is limited by the propagation of the *critical path*.

To resolve that issue, we can employ the technique called *pipelining*. In details, that means to add more registers. Notably that this will cause a signal passing through the same path (ignore the added registers) with more cycles (while the length of a cycle is shrunk), but in the long run, it actually improves performance.

#### Finite State Machines

To represent sequential logic, finite state machines are a good idea. Correspondingly, with combinational logic and registers, any FSM can be implemented in hardware.

#### Functional Units (a.k.a. Execution Unit)

Take a look at the examples in the slides.

## Hardware Architecture Description

### CPU

#### Datapath and Control

A CPU involves two parts:

- Datapath: contains the hardware necessary to perform operations required by the processor
- Control: decides what each piece of datapath should do

![image.png](https://fastly.jsdelivr.net/gh/f1a3h/imgs/202402031403445.png)

The picture above shows the basic architecture of a single-cycle RISC-V RV32I datapath.

Single Cycle CPU: All stages of an instruction completed within one long clock cycle.

![image.png](https://fastly.jsdelivr.net/gh/f1a3h/imgs/202402031417527.png)

Processor design principles:

- Five steps:
	1. Analyze instruction set $\rightarrow$ datapath requirements
	2. Select set of datapath components & establish clock methodology
	3. Assemble datapath meeting the requirements
	4. Analyze implementation of each instruction to determine setting of control points that effects the register transfer
	5. Assemble the control logic
		- Formulate Logic Equations
		- Design Circuits
- Determining control signs

Control construction options:

- ROM
	- Can be easily reprogrammed
	- Popular when design control logic manually
- Combinatorial Logic
	- Not easily re-programmable because requires modifying hardware
	- Likely less expensive, more complex

#### Performance Analysis

![image.png](https://fastly.jsdelivr.net/gh/f1a3h/imgs/202402051411511.png)

`lw` instruction needs to do all the things, so it is the bottleneck of a single cycle. 

"Iron law" of processor performance:

$$\frac{\mathtt{Time}}{\mathtt{Program}}=\frac{\mathtt{Instructions}}{\mathtt{Program}}\cdot\frac{\mathtt{Cycles}}{\mathtt{Instruction}}\cdot\frac{\mathtt{Time}}{\mathtt{Cycle}}$$
# Great Idea \#4: Parallelism

## Pipelining

### Intro

Noticed that since we have to set the maximum clock frequency to  the lowest value to fit in `lw` instruction, there will always be chips idle when performing other instructions.

As an analogy to doing laundry, we can use pipelining to increase *throughput* of entire workload.

![image.png](https://fastly.jsdelivr.net/gh/f1a3h/imgs/202402051436443.png)

To align the five instruction stages, we place a register between each two stages as a buffer.

![image.png](https://fastly.jsdelivr.net/gh/f1a3h/imgs/202402051445337.png)

### Hazards

[lec14.pdf](https://inst.eecs.berkeley.edu/~cs61c/su20/pdfs/lectures/lec14.pdf)

With pipelining, we successfully improve single cycle performance, however, this also leads to some problems which is called *hazard*.

A hazard is a situation that prevents starting the next instruction in the next clock cycle.

1. Structural hazard: a required resource is busy
	- Solution 1: stalling
	- Solution 2: add more hardware
		- Can always solve a structural hazard by adding more hardware
		- Regfile Structural Hazards
			- Solution 1: build Regfile with independent read and write ports
			- Solution 2: double pumping
			- Read and Write during same clock cycle is okay (write first, then read)
		- Memory Structural Hazards
			- Solution: build memory with separate instruction/data units/caches
			- RISC ISAs designed to avoid such hazards
2. Data hazard: data dependency between instructions
	1. R-type instructions
		- Solution 1: stalling
		- Solution 2: forwarding (a.k.a bypassing)
	2. Load instructions
		- Solution 1: stalling = `nop`
		- Solution 2: let the compiler/assembler put an unrelated instruction in that slot (slot after a load is called a *load delay slot*)
3. Control hazard: flow of execution depends on previous instruction
	- Solution 1: stalling
	- Solution 2: move branch comparator to ID stage
	- Solution 3: branch prediction - guess outcome of a branch, fix afterwards if necessary

## Parallelism

### Flynn's Taxonomy

- Choice of hardware and software parallelism are independent
- *Flynn's Taxonomy* is for parallel hardware

![[Screenshot 2024-02-27 at 18.33.57.png]]

### SIMD Architectures

To improve performance, Intel's SIMD instructions:

- Fetch one instruction, do the work of multiple instructions
- MMX (MultiMedia eXtension)
- SSE (Streaming SIMD Extension)

In the evolution of x86 SIMD, it started with MMX, getting new instructions, new and wider registers, more parallelism.

In SSE, architecture extended with eight 128-bit data registers, which are called XMM registers.

> [!note] 
> In 64-bit address architecture, they are available as 16 64-bit registers.

Programming in C, we can use Intel SSE intrinsics, which are C functions and procedures for putting in assembly language, including SSE instructions.

|        Operations         |           Intrinsics            |                   Corresponding SSE instructions                   |
|:-------------------------:|:-------------------------------:|:------------------------------------------------------------------:|
|     Vector data type      |            `_m128d`             |                                                                    |
| Load and store operations | `_mm_load_pd`<br>`_mm_store_pd` | `MOVAPD`/aligned, packed double<br>`MOVAPD`/aligned, packed double |
|        Arithmetic         |  `_mm_add_pd`<br>`_mm_mul_pd`   |   `ADDPD`/add, packed double<br>`MULPD`/multiple, packed double    |

> [!example]- 2x2 Matrix Multiply
> ```c
> #include <stdio.h>
> // header file for SSE compiler intrinsics
> #include <emmintrin.h>
> 
> // NOTE: vector registers will be represented in comments as v1 = [a | b]
> // where v1 is a variable of type __m128d and a, b are doubles
> 
> int main(void) {
> 	// allocate A,B,C aligned on 16-byte boundaries
> 	double A[4] __attribute__ ((aligned (16)));
> 	double B[4] __attribute__ ((aligned (16)));
> 	double C[4] __attribute__ ((aligned (16)));
> 	int lda = 2;
> 	int i = 0;
> 	// declare several 128-bit vector variables
> 	__m128d c1,c2,a,b1,b2;
> 	
> 	// Initialize A, B, C for example
> 	/* A = (note column order!)
> 	   1 0
> 	   0 1
> 	*/
> 	A[0] = 1.0; A[1] = 0.0; A[2] = 0.0; A[3] = 1.0;
> 	
> 	/* B = (note column order!)
> 	   1 3
> 	   2 4
> 	*/
> 	B[0] = 1.0; B[1] = 2.0; B[2] = 3.0; B[3] = 4.0;
> 	
> 	/* C = (note column order!)
> 	   0 0
> 	   0 0
> 	*/
> 	C[0] = 0.0; C[1] = 0.0; C[2] = 0.0; C[3] = 0.0;
> 	
> 	// used aligned loads to set
> 	// c1 = [c_11 | c_21]
> 	c1 = _mm_load_pd(C+0*lda);
> 	// c2 = [c_12 | c_22]
> 	c2 = _mm_load_pd(C+1*lda);
> 	
> 	for (i = 0; i < 2; i++) {
> 		/* a =
> 		i = 0: [a_11 | a_21]
> 		i = 1: [a_12 | a_22]
> 		*/
> 		a = _mm_load_pd(A+i*lda);
> 		/* b1 =
> 		i = 0: [b_11 | b_11]
> 		i = 1: [b_21 | b_21]
> 		*/
> 		b1 = _mm_load1_pd(B+i+0*lda);
> 		/* b2 =
> 		i = 0: [b_12 | b_12]
> 		i = 1: [b_22 | b_22]
> 		*/
> 		b2 = _mm_load1_pd(B+i+1*lda);
> 		/* c1 =
> 		i = 0: [c_11 + a_11*b_11 | c_21 + a_21*b_11]
> 		i = 1: [c_11 + a_21*b_21 | c_21 + a_22*b_21]
> 		*/
> 		c1 = _mm_add_pd(c1,_mm_mul_pd(a,b1));
> 		/* c2 =
> 		i = 0: [c_12 + a_11*b_12 | c_22 + a_21*b_12]
> 		i = 1: [c_12 + a_21*b_22 | c_22 + a_22*b_22]
> 		*/
> 		c2 = _mm_add_pd(c2,_mm_mul_pd(a,b2));
> 	}
> 	
> 	// store c1,c2 back into C for completion
> 	_mm_store_pd(C+0*lda,c1);
> 	_mm_store_pd(C+1*lda,c2);
> 	
> 	// print C
> 	printf("%g,%g\n%g,%g\n",C[0],C[2],C[1],C[3]);
> 	return 0;
> }
> ```

To improve RISC-V performance, add SIMD instructions (and hardware) – V extension:

- `vadd vd, vs1, vs2`
# Great Idea \#3: Principle of Locality

## Caches

Principle of Locality: Programs access only a small portion of the full address space at any instant time

- Temporary Locality (time)
- Spatial Locality (space)

![image.png](https://fastly.jsdelivr.net/gh/f1a3h/imgs/202402092218872.png)

In typical memory hierarchy, from top to down we have:

- RegFile
- First Level Cache (consists of Instr Cache & Data Cache) $\rightarrow$ `$L1`
- Second Level Cache (SRAM) $\rightarrow$ `$L2`
- Main Memory (DRAM)
- Secondary Memory (Disk or Flash)

Upper above the Main Memory are On-Chip Components.

### Fully Associative Caches

Fully associative: each memory block can map anywhere in the cache.

![image.png](https://fastly.jsdelivr.net/gh/f1a3h/imgs/PicGo/202402092234541.png)

In the cache:

- Each cache slot contains the actual data block ($8\times 2^{\mathrm{Offset}}$ bits)
- `Tag` field as identifier ($\mathrm{Tag}$ bits)
- `Valid` bit indicate whether cache slot was filled in ($1$ bit)
- Any necessary replacement management bits ("LRU(Least Recently Used) bits" - variable $\#$ bits)

Total bits in cache = $\#\, \mathrm{slots}\times(8\times 2^{\mathrm{Offset}}+\mathrm{Tag}+1+?)$ bits

![image.png](https://fastly.jsdelivr.net/gh/f1a3h/imgs/PicGo/202402092250179.png)

### Direct-Mapped Caches

Direct-mapped cache: each memory address is associated with one possible block within the cache

![[Screenshot 2024-02-16 at 00.33.39.png]]

Within the cache:

- Index: which row/block
	- $\# \mathrm{blocks}/ \mathrm{cache}=\dfrac{\mathrm{bytes/ cache}}{\mathrm{bytes/ block}}$
- Offset: which byte within the block
- Tag: distinguish between all the memory addresses that map to the same location
	- $\mathrm{tag \, length=addr \, length - offset - index}$

### N-Way Set Associative Caches

pros:

- avoids a lot of conflict misses
- hardware cost isn't that bad: only need N comparators

1-way set assoc $\leftarrow$ directly mapped

M-way set assoc $\leftarrow$ fully assoc

### Writes, Block Sizes, Misses

- Effective
	- Hit rate (HR)
	- Miss rate (MR)
- Fast
	- Hit time (HT)
	- Miss penalty (MP)

Handle cache hits:

- Read hits (`I$` & `D$`)
	- Fasted possible scenario
- Write hits (`D$`)
	1. Write-Through Policy: always write to cache and memory (through cache)
		- Write buffer
	2. Write-Back Policy: write only to cache, update memory when block is removed
		- Dirty bit

Handle cache misses:

- Read misses (`$I` & `$D`)
	- Stall, fetch from memory, put in cache
- Write misses (`$D`)
	- Write Allocate Policy: bring the block into the cache after a write miss
	- No Write Allocate Polity: only change main memory
	- Write allocate $\leftrightarrow$ write back, every slot needs an extra `dirty` bit
	- No write allocate $\leftrightarrow$ write-through

Block size tradeoff:

- Benefits of larger block size
	- Spatial Locality
	- Very applicable with Stored-Program Concept
	- Works well for sequential array accesses
- Drawbacks of larger block size
	- Larger miss penalty
	- If block size is too big relative to cache size, then there are too few blocks $\rightarrow$ Higher miss rate

![[Screenshot 2024-02-16 at 20.27.04.png]]

Types of cache misses:

1. Compulsory Misses
2. Conflict Misses (Direct-Mapped Cache only)
	- Solution 1: Make the cache size bigger
	- Solution 2: Multiple distinct blocks can fit in the same cache Index
3. Capacity Misses (primary for Fully Associative Cache)

How to categorize misses:

![[Screenshot 2024-02-16 at 20.41.07.png]]

#### Block Replacement Policy

- LRU (Least Recently Used)
- FIFO
- Random

#### Average Memory Access Time

AMAT = Hit Time $+$ Miss Penalty $\times$ Miss Rate

> [!summary]+
> 
>  Why not "Hit Time $\times$ Hit Rate $+$ Miss Time $\times$ Miss Rate"?
>  
>  AMAT = Hit Time $\times$ (1 $-$ Miss Rate) + (Miss Penalty $-$ Hit Time) $\times$ Miss Rate

> [!summary]+
> 
>  Big Idea: If something is expensive but we want to do it repeatedly, do it once and cache the result
>  
>  Cache Design Choices:
>  - size of cache: speed vs. capacity
>  - block size
>  - write policy
>  - associativity choice of N
>  - block replacement policy
>  - 2nd level cache
>  - 3rd level cache

## OS

### Basics

What does OS do?

- OS is the first thing that runs when computer starts
- Finds and controls all devices in the machine in a general way
- Starts services
- Loads, runs and manages programs

What does the core of OS do?

- Provides *isolation* between running processes
- Provides *interaction* with the outside world

What does OS need from hardware?

- Memory translation
- Protection and privilege
	- Split the processor into at least two modes: "User" and "Supervisor"
	- Lesser privilege cannot change its memory mapping
- Traps & Interrupts
	- A way of going into supervisor mode on demand

What happens at boot?

1. BIOS[^1]: Find a storage device and loads the first sector
2. Bootloader: Load the OS kernel from disk into a location in memory and jump into it
3. Init: Launch an application that waits for input in loop (e.g., Terminal/Desktop/...)
4. OS Boot: Initialize services, drivers, etc.

[^1]: BIOS: Basic Input Output System

### OS Functions

Refer to the lecture [slides](https://inst.eecs.berkeley.edu/~cs61c/fa20/pdfs/lectures/lec28.pdf).

### Virtual Memory

![[Screenshot 2024-02-26 at 21.59.28.png|Hierarchy]]

![[Screenshot 2024-02-26 at 22.01.01.png]]

- Processes uses virtual addresses
- Memory uses physical addresses

Address space = set of addresses for all available memory locations.

Responsibilities of memory manager:

1. Map virtual to physical addresses
2. Protection: isolate memory between processes
3. Swap memory to disk: give illusions of larger memory by storing some contents on disk

Physical memory (DRAM) is broken into pages:

![[Screenshot 2024-02-26 at 22.20.39.png]]

Paged memory translation:

- OS keeps track of active processes
- Memory manager extracts page number from virtual address
- Looks up page address in page table
- Computes physical memory address from sum of
	- page address and
	- offset (from virtual address)

> [!note]+ 
> Physical addresses may have more or fewer bits than virtual addresses.

Since a page table is too large for a cache, so we have to store it in memory (DRAM, acts like caches for disk). To minimize performance penalty, we can:

- Transfer blocks (not words) between DRAM and caches
- Use a cache for frequently accessed page table entries

Refer to [slides](https://inst.eecs.berkeley.edu/~cs61c/fa20/pdfs/lectures/lec30.pdf).

### I/O

Interface options:

1. Special I/O instructions
2. Memory mapped I/O
	- Use normal load/store instructions

I/O polling:

- Device registers:
	- Control register
	- Data register
- Processor reads from control register in loop
- then loads from (input) / writes to (output) data register
- Procedure called "polling"

Polling wastes processor resources, there is an alternative called "interrupts":

- No I/O activity: nothing to do
- Lots of I/O: expensive - thrashing cashes, VM, saving/restoring state

When there is low data rate, we use interrputs. While there's high data rate, we start with interrupts, then switch to Direct Memory Access (DMA).

The DMA allows I/O devices to directly read/write main memory, introducing a new hardware called *DMA engine* to let CPU execute other instructions.

- Incoming data
	- Receive interrupt from device
	- CPU takes interrupt, initiates transfer
	- Device/DMA engine handle the transfer
		- CPU is free to execute other things
	- Upon completion, device/DMA engine interrupts the CPU again
- Outging data
	- CPU decides to initiate transfer, confirms that external device is ready
	- CPU begins transfer
	- Device/DMA engine handle the transfer
	- Device/DMA engine interrupt the CPU again to signal completion

To plug in the DMA engine in the memory hierarchy, there are two extremes:

- Between `L1$` and CPU
	- Pro: free coherency
	- Con: trash the CPU's working set with transferred data
- Between Last-level cache and main memory
	- Pro: don't mess with cache
	- Con: need to explicitly manage coherency