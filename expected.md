# Expected Questions and CLO Mapping

Sources:
- [Questions.pdf](<Questions.pdf>)
- [WhatsApp Image 2026-05-16 at 8.49.49 PM.jpeg](<WhatsApp Image 2026-05-16 at 8.49.49 PM.jpeg>)
- [WhatsApp Image 2026-05-16 at 8.49.50 PM.jpeg](<WhatsApp Image 2026-05-16 at 8.49.50 PM.jpeg>) checked separately; it is an Analog note, not PVL238 VLSI Architectures.

Numbering:
- `Q1` to `Q25` are copied from `Questions.pdf` first.
- `H1` onward are from the handwritten VLSI Architecture note.
- `A1` onward are from the separate Analog handwritten note and are not mapped to PVL238 CLOs.

## CLO Key

### CLO 1 - Processor Architecture and Organization

Describe the basics of different processors based on their architecture and organization.

### CLO 2 - Pipelining and RISC Performance

Apply the concepts of pipelining to improvise the performance of RISC architecture.

### CLO 3 - Memory Organization and Management

Understand various memory organization and management techniques.

### CLO 4 - ILP and Advanced Architectures

Acquire the knowledge of instruction-level parallelism and various advanced architectures.

## Questions From Questions.pdf

### Q1

**CLO:** [CLO 3](#clo-3---memory-organization-and-management), [CLO 4](#clo-4---ilp-and-advanced-architectures)

Explain hierarchical memory organization in computer systems. Discuss inclusion, coherence, and locality properties.

### Q2

**CLO:** [CLO 3](#clo-3---memory-organization-and-management)

Discuss memory management techniques used in virtual memory systems. Explain paging and segmentation with examples.

### Q3

**CLO:** [CLO 3](#clo-3---memory-organization-and-management), [CLO 4](#clo-4---ilp-and-advanced-architectures)

Explain coherence problems in shared memory systems. Discuss cache coherence protocols in multiprocessor systems.

### Q4

**CLO:** [CLO 4](#clo-4---ilp-and-advanced-architectures)

Explain the concept of Instruction-Level Parallelism (ILP). Discuss various techniques used to increase ILP.

### Q5

**CLO:** [CLO 2](#clo-2---pipelining-and-risc-performance), [CLO 4](#clo-4---ilp-and-advanced-architectures)

Compare superscalar, super-pipelined, and VLIW processor architectures with suitable block diagrams.

### Q6

**CLO:** [CLO 4](#clo-4---ilp-and-advanced-architectures)

Explain Flynn's taxonomy of parallel architectures with suitable examples.

### Q7

**CLO:** [CLO 4](#clo-4---ilp-and-advanced-architectures)

Discuss centralized shared-memory multiprocessor architecture. Explain synchronization techniques used in shared-memory systems.

### Q8

**CLO:** [CLO 3](#clo-3---memory-organization-and-management), [CLO 4](#clo-4---ilp-and-advanced-architectures)

Explain distributed shared-memory architecture in detail. Discuss its advantages, limitations, and applications.

### Q9

**CLO:** [CLO 4](#clo-4---ilp-and-advanced-architectures)

Discuss different interconnection networks used in multiprocessor architectures. Compare bus, crossbar, multistage, and hypercube networks.

### Q10

**CLO:** [CLO 1](#clo-1---processor-architecture-and-organization), [CLO 2](#clo-2---pipelining-and-risc-performance)

Explain the design of a multicycle RISC processor and discuss how pipelining improves processor performance. Also explain different hazards and their handling techniques.

### Q11

**CLO:** [CLO 3](#clo-3---memory-organization-and-management), [CLO 4](#clo-4---ilp-and-advanced-architectures)

Explain the role of memory hierarchy in improving system performance. Discuss cache coherence, virtual memory, and replacement policies.

### Q12

**CLO:** [CLO 2](#clo-2---pipelining-and-risc-performance)

Discuss the impact of pipeline depth on processor performance. Explain the associated challenges and optimization methods.

### Q13

**CLO:** [CLO 1](#clo-1---processor-architecture-and-organization), [CLO 2](#clo-2---pipelining-and-risc-performance), [CLO 3](#clo-3---memory-organization-and-management), [CLO 4](#clo-4---ilp-and-advanced-architectures)

Design considerations for high-performance processors include pipelining, cache hierarchy, ILP, and multiprocessing. Discuss these aspects in detail with suitable examples.

### Q14

**CLO:** [CLO 1](#clo-1---processor-architecture-and-organization), [CLO 3](#clo-3---memory-organization-and-management)

Explain processor organization and architectural overview of modern computer systems. Discuss the role of ALU, control unit, registers, buses, and memory hierarchy.

### Q15

**CLO:** [CLO 2](#clo-2---pipelining-and-risc-performance)

Explain data hazards in pipelined processors with suitable timing diagrams. Discuss forwarding, stalling, and pipeline interlocking techniques.

### Q16

**CLO:** [CLO 4](#clo-4---ilp-and-advanced-architectures)

Find ILP in Given Instruction Sequence. Consider the following instructions:

```text
I1: R1 = R2 + R3
I2: R4 = R1 + R5
I3: R6 = R7 + R8
I4: R9 = R6 + R10
I5: R11 = R12 + R13
I6: R14 = R1 + R6
```

Find:
- a) Identify all data dependencies.
- b) Draw the dependency graph.
- c) Find the maximum instruction-level parallelism.
- d) Arrange the instructions into minimum execution cycles assuming unlimited functional units.

### Q17

**CLO:** [CLO 4](#clo-4---ilp-and-advanced-architectures)

Given:

```text
I1: A = B + C
I2: D = E + F
I3: G = A + D
I4: H = G + I
I5: J = K + L
I6: M = J + H
```

Assume each instruction takes one cycle.

Find:
- a) Which instructions can execute in parallel?
- b) Minimum number of cycles required.
- c) Average ILP.

### Q18

**CLO:** [CLO 4](#clo-4---ilp-and-advanced-architectures)

Consider:

```text
I1: ADD R1, R2, R3
I2: SUB R4, R1, R5
I3: MUL R6, R4, R7
I4: ADD R8, R2, R3
I5: SUB R9, R8, R6
```

Identify:
- a) RAW hazards
- b) WAR hazards
- c) WAW hazards
- d) Instructions that can be reordered safely.

### Q19

**CLO:** [CLO 4](#clo-4---ilp-and-advanced-architectures)

Given the following instruction sequence:

```text
I1: R1 = R2 + R3
I2: R2 = R4 + R5
I3: R6 = R1 + R7
I4: R8 = R2 + R9
I5: R10 = R11 + R12
I6: R13 = R10 + R14
```

Find:
- a) True dependencies
- b) Anti-dependencies
- c) Output dependencies
- d) Maximum parallel groups.

### Q20

**CLO:** [CLO 2](#clo-2---pipelining-and-risc-performance), [CLO 4](#clo-4---ilp-and-advanced-architectures)

Assume:
- ADD latency = 1 cycle
- MUL latency = 3 cycles
- LOAD latency = 2 cycles

Instruction sequence:

```text
I1: LOAD R1, 0(R2)
I2: ADD R3, R1, R4
I3: MUL R5, R3, R6
I4: ADD R7, R8, R9
I5: MUL R10, R7, R11
I6: ADD R12, R5, R10
```

Find:
- a) Data dependency graph
- b) Number of stalls without scheduling
- c) Optimized instruction schedule
- d) Total execution cycles after scheduling.

### Q21

**CLO:** [CLO 2](#clo-2---pipelining-and-risc-performance)

A pipeline has 5 stages with delays:

```text
S1 = 4 ns
S2 = 6 ns
S3 = 8 ns
S4 = 5 ns
S5 = 7 ns
```

Pipeline register delay = 1 ns.

Calculate:
- a) Pipeline cycle time
- b) Time to execute 50 instructions
- c) Non-pipelined time for 50 instructions
- d) Speedup.

### Q22

**CLO:** [CLO 2](#clo-2---pipelining-and-risc-performance)

A pipelined processor has ideal CPI = 1. The instruction mix is:
- 30% load instructions
- 20% branch instructions
- 50% ALU instructions

Assume:
- 40% of loads cause 1 stall cycle
- 50% of branches cause 2 stall cycles

Find:
- a) Average stall cycles per instruction
- b) Effective CPI
- c) Speedup over non-pipelined processor with CPI = 5.

### Q23

**CLO:** [CLO 2](#clo-2---pipelining-and-risc-performance), [CLO 3](#clo-3---memory-organization-and-management)

A 5-stage pipeline uses a single memory for both instruction fetch and data access. Load/store instructions are 40% of total instructions. Each load/store causes one structural stall.

For 1,000 instructions, find:
- a) Number of structural stalls
- b) Total cycles
- c) Effective CPI.

### Q24

**CLO:** [CLO 1](#clo-1---processor-architecture-and-organization)

Identify Addressing Modes. Identify the addressing mode used in each instruction.

```text
a) MOV R1, R2
b) MOV R1, #25
c) MOV R1, 2000
d) MOV R1, (R2)
e) MOV R1, 20(R2)
f) MOV R1, @(R2)+
g) JMP LOOP
h) ADD R1, R2, R3
```

### Q25

**CLO:** [CLO 1](#clo-1---processor-architecture-and-organization), [CLO 3](#clo-3---memory-organization-and-management)

Calculate Effective Address. Assume:

```text
R1 = 1000
R2 = 2000
Memory[1000] = 5000
Memory[2000] = 7000
Memory[2020] = 9000
PC = 4000
```

Find the effective address and operand value for:

```text
a) MOV R3, (R1)
b) MOV R3, 20(R2)
c) MOV R3, #50
d) MOV R3, @R1
e) JMP 40(PC)
```

## Handwritten VLSI Architecture Notes

Source: [WhatsApp Image 2026-05-16 at 8.49.49 PM.jpeg](<WhatsApp Image 2026-05-16 at 8.49.49 PM.jpeg>)

### H1

**CLO:** [CLO 4](#clo-4---ilp-and-advanced-architectures)

Instruction-level architecture.

### H2

**CLO:** [CLO 1](#clo-1---processor-architecture-and-organization), [CLO 2](#clo-2---pipelining-and-risc-performance), [CLO 3](#clo-3---memory-organization-and-management)

Study scope written as: "Till memory -> book."

### H3

**CLO:** General exam pattern across CLOs

Expected pattern: around `6+` questions, with a few theory questions and a few conceptual/numerical questions.

### H4

**CLO:** [CLO 1](#clo-1---processor-architecture-and-organization)

Calculate CPI.

### H5

**CLO:** [CLO 1](#clo-1---processor-architecture-and-organization), [CLO 3](#clo-3---memory-organization-and-management)

Memory address calculation or memory address fields.

### H6

**CLO:** [CLO 4](#clo-4---ilp-and-advanced-architectures)

Calculate how to parallelize if we have some instructions.

### H7

**CLO:** [CLO 4](#clo-4---ilp-and-advanced-architectures)

If we have 10 lines of code, explain how we will parallelize them.

### H8

**CLO:** [CLO 2](#clo-2---pipelining-and-risc-performance)

Pipeline -> branching.

### H9

**CLO:** [CLO 3](#clo-3---memory-organization-and-management)

Virtual page and offset.

### H10

**CLO:** [CLO 3](#clo-3---memory-organization-and-management)

Virtual memory -> paging and segmentation.

### H11

**CLO:** [CLO 2](#clo-2---pipelining-and-risc-performance), [CLO 3](#clo-3---memory-organization-and-management)

Handwriting appears as "DMA" or an unclear related item, followed by "hardware based" and "compiler based." If this is about hazard handling, it belongs mainly to CLO 2; if it is literally DMA, it belongs mainly to CLO 3.

### H12

**CLO:** [CLO 3](#clo-3---memory-organization-and-management)

Cache hit/miss and how to minimize misses.

### H13

**CLO:** Unclear from source

There is a standalone unclear question mark/item after cache hit/miss in the handwritten note. The content is not readable enough to map safely.

### H14

**CLO:** [CLO 3](#clo-3---memory-organization-and-management), [CLO 4](#clo-4---ilp-and-advanced-architectures)

What is snooping?

### H15

**CLO:** [CLO 4](#clo-4---ilp-and-advanced-architectures)

What is network structure?

### H16

**CLO:** [CLO 4](#clo-4---ilp-and-advanced-architectures)

How are memory and processor connected? The note marks this as from PPT.

### H17

**CLO:** General exam pattern across CLOs

Pre-MST: written as "Q -> 1 or 1.5."

### H18

**CLO:** Unclear from source

Right-margin partial text is visible but not readable enough to map safely; it may relate to pipelining or operation/programming.

## Cross-Source Overlap To Prioritize

These topics appear in both `Questions.pdf` and the handwritten VLSI Architecture note:

1. ILP and code/instruction parallelization: [Q4](#q4), [Q16](#q16), [Q17](#q17), [Q18](#q18), [Q19](#q19), [Q20](#q20), [H1](#h1), [H6](#h6), [H7](#h7)
2. Pipelining, branching, hazards, stalls, and speedup: [Q10](#q10), [Q12](#q12), [Q15](#q15), [Q20](#q20), [Q21](#q21), [Q22](#q22), [Q23](#q23), [H8](#h8), [H11](#h11)
3. Virtual memory, virtual page, offset, paging, and segmentation: [Q2](#q2), [Q11](#q11), [H9](#h9), [H10](#h10)
4. Memory hierarchy, cache hit/miss, miss minimization, replacement, and coherence: [Q1](#q1), [Q3](#q3), [Q11](#q11), [H12](#h12), [H14](#h14)
5. CPI, addressing modes, effective address, and memory address calculations: [Q22](#q22), [Q24](#q24), [Q25](#q25), [H4](#h4), [H5](#h5)
6. Multiprocessors, snooping, shared memory, and interconnection/network structure: [Q3](#q3), [Q6](#q6), [Q7](#q7), [Q8](#q8), [Q9](#q9), [H14](#h14), [H15](#h15), [H16](#h16)

## Separate Handwritten Analog Note

Source: [WhatsApp Image 2026-05-16 at 8.49.50 PM.jpeg](<WhatsApp Image 2026-05-16 at 8.49.50 PM.jpeg>)

This page is headed "Analog" and does not match the PVL238 VLSI Architectures CLOs above. It is included here only so the handwritten file is not ignored.

### A1

**PVL238 CLO:** Not mapped

Common mode rejection ratio.

### A2

**PVL238 CLO:** Not mapped

Frequency response, including number of poles and zeros.

### A3

**PVL238 CLO:** Not mapped

Band gap reference (BGR): use and how designing is done.

### A4

**PVL238 CLO:** Not mapped

Miller theorem: how much capacitance appears at input and output.

### A5

**PVL238 CLO:** Not mapped

Noise and PSD; layout design rules; shot noise, thermal noise, and flicker noise.

### A6

**PVL238 CLO:** Not mapped

Differential pair and 2-stage op-amp design; gain margin and phase margin when values are given.

### A7

**PVL238 CLO:** Not mapped

Discuss the 2-stage op-amp design and techniques to increase gain.

### A8

**PVL238 CLO:** Not mapped

Complete design of OTA and numerical.
