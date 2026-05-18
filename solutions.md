# Expected Questions - Solutions

Workspace: `C:/Users/kapil/OneDrive/Desktop/Architecture`

This file will be built question by question from [expected.md](<expected.md>).

## Index

- [Q1. Hierarchical memory organization, inclusion, coherence, locality](#q1)
- [Q2. Virtual memory, paging, segmentation](#q2)
- [Q3. Shared-memory coherence problems and cache coherence protocols](#q3)
- [Q4. Instruction-Level Parallelism (ILP) and techniques to increase ILP](#q4)
- [Q5. Superscalar, super-pipelined, and VLIW architectures](#q5)
- [Q6. Flynn's taxonomy of parallel architectures](#q6)
- [Q7. Centralized shared-memory multiprocessor and synchronization](#q7)
- [Q8. Distributed shared-memory architecture](#q8)
- [Q9. Interconnection networks: bus, crossbar, multistage, hypercube](#q9)
- [Q10. Multicycle RISC processor, pipelining, and hazards](#q10)
- [Q11. Memory hierarchy, coherence, virtual memory, replacement](#q11)
- [Q12. Pipeline depth impact and optimization](#q12)
- [Q13. High-performance processor design considerations](#q13)
- [Q14. Processor organization and modern system overview](#q14)
- [Q15. Data hazards, forwarding, stalling, and interlocking](#q15)

## Sources Used For Q1

- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - Chapter 2: Memory Hierarchy Design; Appendix B: Review of Memory Hierarchy; Chapter 5: cache coherence background.
- [Memory.pdf](<MaterialToRefer/Memory.pdf>) - memory hierarchy, SRAM/DRAM, cache, locality, inclusion, coherence, hit/miss, mapping, replacement, virtual memory.
- [VLSI ARC.pptx](<MaterialToRefer/VLSI ARC.pptx>) - basic architecture components: registers, cache, main memory, processor organization.
- Class board image shared in chat - memory hierarchy pyramid from registers down to cache, RAM, flash/SSD, hard disk, and tapes.
- Screenshot exported for exam diagram: [Memory.pdf page 1 memory hierarchy](<images/solutions/q1/memory_pdf_page_01_memory_hierarchy.png>).
- Class board screenshot for exam diagram: [class board memory hierarchy](<images/solutions/q1/class_board_memory_hierarchy.png>).

---

<a id="q1"></a>

## Q1. Explain Hierarchical Memory Organization In Computer Systems. Discuss Inclusion, Coherence, And Locality Properties.

**CLO:** CLO 3 - Memory Organization and Management; CLO 4 - Advanced Architectures support.

### PPT/PDF Figure Check

Checked local material:
- `Memory.pdf`: relevant figure found on page 1. It shows the memory hierarchy pyramid with CPU registers, cache memory, main memory, magnetic disk, optical disk, and magnetic tape.
- `VLSI ARC.pptx`: relevant supporting slides found for computer architecture components, registers, cache, main memory, and processor organization. No better memory hierarchy figure was present there.
- Class board screenshot: relevant figure found and saved.

Use these for the diagram:

![Memory hierarchy figure from Memory.pdf](<images/solutions/q1/memory_pdf_page_01_memory_hierarchy.png>)

![Class board memory hierarchy figure](<images/solutions/q1/class_board_memory_hierarchy.png>)

### What To Draw In Exam

Draw the pyramid before writing the theory. It is the most useful diagram for this question.

Your exam diagram should show:
- Top: `Registers`
- Then: `Cache Memory (SRAM)` or `L1/L2/L3 Cache`
- Then: `Main Memory / RAM (DRAM)`
- Then: `SSD / Flash / Magnetic Disk`
- Bottom: `Magnetic Tape / Backup Storage`
- Side arrow upward: `Cost per bit and speed increase`
- Side arrow downward: `Capacity and access time increase`

Simple exam diagram:

```text
                 Speed and cost per bit increase
                              ^
                              |
                         +-----------+
                         | Registers |
                         +-----------+
                         | L1 Cache  |
                         +-----------+
                         | L2 Cache  |
                         +-----------+
                         | L3 Cache  |
                         +-----------+
                         | Main RAM  |
                         +-----------+
                         | SSD/HDD   |
                         +-----------+
                         | Tape      |
                         +-----------+
                              |
                              v
                   Capacity and access time increase
```

Write the answer after this diagram. Mention SRAM beside cache and DRAM beside main memory because teachers usually expect that.

### 1. Exam-Style Answer

Hierarchical memory organization is the arrangement of storage devices in multiple levels according to speed, cost, capacity, and distance from the CPU. The CPU needs very fast access to instructions and data, but very fast memory is expensive and small. Therefore, computer systems use a memory hierarchy: small and fast memories are placed near the processor, while large and slow memories are placed farther away.

The hierarchy normally starts with CPU registers, then cache memory, main memory, secondary storage such as SSD/HDD, and finally tertiary storage such as magnetic tape. The goal is to make the system appear to have a large, cheap memory while still giving performance close to a small, fast memory.

This organization works because programs show locality of reference: they repeatedly use recently accessed data and often access nearby addresses. The system keeps active blocks of data in faster levels. When data is found in the upper level, it is called a hit; when it is not found, it is called a miss, and the block is fetched from a lower level.

Inclusion ensures that contents of an upper memory level are also present in the next lower level, for example L1 contents are also present in L2 or L3 in an inclusive cache hierarchy. Coherence ensures that all copies of the same data item remain consistent when the data is modified. Locality is the program behavior that makes the hierarchy effective.

### 2. Memory Hierarchy Diagram

```text
                 Fastest, smallest, costliest per bit
                              |
                              v
                         +-----------+
                         | Registers |
                         +-----------+
                         | L1 Cache  |
                         +-----------+
                         | L2 Cache  |
                         +-----------+
                         | L3 Cache  |
                         +-----------+
                         | Main RAM  |
                         +-----------+
                         | SSD/Flash |
                         +-----------+
                         | HDD/Disk  |
                         +-----------+
                         | Tape/Arch |
                         +-----------+
                              ^
                              |
                 Slowest, largest, cheapest per bit
```

From top to bottom:
- Speed decreases.
- Capacity increases.
- Cost per bit decreases.
- Access time increases.
- Distance from CPU increases.

### 3. Levels Of Memory Hierarchy

The exact numbers change from system to system, but the exam idea is the order of magnitude: every lower level is much larger and much slower than the level above it.

| Level | Example | Technology | Typical Capacity | Typical Access Time / Latency | Approx. CPU Cycles | Main Purpose |
|---|---|---|---:|---:|---:|---|
| CPU registers | General-purpose registers, PC, status registers | Flip-flops | Bytes to a few KB | About 0.2 to 1 ns | 1 cycle or less | Hold current operands and addresses |
| L1 cache | Instruction cache, data cache | SRAM | 16 KB to 128 KB | About 0.5 to 1 ns | 1 to 4 cycles | Fastest cache for immediate CPU access |
| L2 cache | Per-core or shared cache | SRAM | 256 KB to a few MB | About 2 to 5 ns | 5 to 15 cycles | Backup for L1 misses |
| L3 cache | Shared last-level cache | SRAM | Few MB to tens of MB | About 10 to 30 ns | 30 to 100 cycles | Shared cache before going to RAM |
| Main memory | RAM | DRAM | GB range | About 50 to 100 ns | 100 to 300 cycles | Store running programs and data |
| SSD / flash storage | NVMe SSD, SATA SSD, USB flash | NAND flash | GB to TB | About 10 us to 1 ms | Thousands to millions of cycles | Persistent storage, faster than disk |
| Magnetic disk | Hard disk drive | Magnetic disk | GB to TB | About 5 to 15 ms | Millions of cycles | Large persistent storage |
| Magnetic tape | Tape drive, archive tape | Magnetic tape | TB to PB | Seconds to minutes for random access | Extremely high | Backup and archival storage |

Registers and cache are often called internal memory because they are close to or inside the CPU chip. Main memory is primary memory. SSD/HDD are secondary storage. Tape is tertiary storage.

### 4. Why Memory Hierarchy Is Needed

The processor is much faster than main memory. If the CPU had to wait for DRAM or disk for every instruction and data access, performance would drop heavily. At the same time, building all memory using very fast SRAM or registers is not practical because it is expensive, consumes more chip area, and has limited capacity.

So designers combine different memory technologies:
- Registers are extremely fast but very small.
- SRAM cache is fast but costly.
- DRAM main memory is larger and cheaper but slower.
- SSD/HDD is much larger and persistent but far slower.

This gives the illusion of a large and fast memory system.

### 5. Basic Working: Hit And Miss

Data moves between adjacent levels in fixed-size units:
- Between cache and RAM, the unit is usually a cache block or cache line.
- Between RAM and disk/SSD in virtual memory, the unit is usually a page.

When the processor requests data:

1. CPU first checks registers or cache.
2. If data is found, it is a hit.
3. If data is not found, it is a miss.
4. On a miss, the lower level supplies the required block.
5. The block is copied upward because it may be used again soon.

The standard performance idea is:

```text
Average Memory Access Time = Hit Time + Miss Rate x Miss Penalty
```

Where:
- Hit time is the time to access the upper level.
- Miss rate is the fraction of accesses not found in the upper level.
- Miss penalty is the extra time to fetch from the lower level.

For example, if cache hit time is low and miss rate is also low, the CPU sees memory as almost as fast as cache most of the time.

### 6. Locality Property

Locality is the most important reason memory hierarchy works. It means that programs do not access memory randomly all the time. They tend to reuse a small working set of instructions and data.

#### 6.1 Temporal Locality

Temporal locality means that if a memory location is accessed now, it is likely to be accessed again soon.

Examples:
- Loop instructions execute again and again.
- A variable such as `sum` is updated repeatedly.
- Stack variables are accessed repeatedly during function execution.

Because of temporal locality, keeping recently used data in cache improves performance.

#### 6.2 Spatial Locality

Spatial locality means that if a memory location is accessed, nearby memory locations are likely to be accessed soon.

Examples:
- Sequential instruction execution.
- Array traversal such as `A[0], A[1], A[2], ...`.
- Reading structure fields stored close together in memory.

Because of spatial locality, caches fetch a block of nearby bytes, not just one byte or word.

#### 6.3 Sequential Locality

Sequential locality is a special case of spatial locality. Instructions are normally fetched from consecutive addresses unless a branch or jump occurs. This is why instruction caches are effective.

### 7. Inclusion Property

Inclusion means that data present in an upper level of the hierarchy is also present in a lower level.

Example:

```text
L1 cache contents ⊆ L2 cache contents ⊆ L3 cache contents ⊆ Main memory contents
```

If a block is in L1, an inclusive hierarchy also keeps a copy of that block in L2 or L3. This is also called the subset property.

#### Benefits Of Inclusion

Inclusion makes it easier to:
- Track which blocks may be present in upper caches.
- Maintain cache coherence in multicore processors.
- Invalidate upper-level copies by checking the lower-level inclusive cache.
- Simplify snooping or directory-based coherence mechanisms.

For example, in a multicore system, if the shared L3 cache is inclusive of private L1 and L2 caches, the system can use L3 information to know whether a core may have a copy of a block.

#### Cost Of Inclusion

The disadvantage is duplication. Some cache space is used to store copies already present in higher levels. Also, when a block is evicted from a lower inclusive level, the system may need to invalidate the same block from upper levels.

Modern processors may use:
- Inclusive cache: upper-level blocks must also exist in lower level.
- Exclusive cache: a block is kept in only one cache level at a time.
- Non-inclusive/non-exclusive cache: no strict rule.

For exam answers, define inclusion clearly and mention that it helps coherence but may waste cache capacity.

### 8. Coherence Property

Coherence means that all copies of the same memory block must show a consistent value. This is important because the same data can exist in multiple places:
- L1 cache
- L2 cache
- L3 cache
- Main memory
- Caches of other processor cores

Without coherence, one part of the system may read an old value while another part has already written a new value.

Example:

```text
Initial value: X = 10

Core 1 reads X into its cache.
Core 2 reads X into its cache.
Core 1 writes X = 20.
Core 2 still has old X = 10 unless coherence action is taken.
```

This is the cache coherence problem.

#### Coherence In A Single Processor Hierarchy

In a single processor system, coherence mainly means that copies between cache and main memory must be managed correctly.

Two common write policies:

| Policy | Meaning | Coherence Effect |
|---|---|---|
| Write-through | Every cache write also updates lower memory immediately | Main memory stays updated, but traffic is higher |
| Write-back | Cache write updates only cache first; memory is updated when block is evicted | Faster, but needs dirty bit and careful control |

#### Coherence In Multiprocessors

In multicore or multiprocessor systems, each core may have private caches. Coherence protocols keep the copies synchronized.

Common methods:
- Snooping protocol: caches observe bus/interconnect transactions and invalidate or update their copies.
- Directory-based protocol: a directory records which caches have a copy of each block.
- Write-invalidate protocol: when one core writes a block, other cached copies are invalidated.
- Write-update protocol: when one core writes, updated value is sent to other copies.

For this question, the key point is: coherence ensures that reads return the correct latest value according to the memory model.

### 9. Relationship Between Inclusion, Coherence, And Locality

These three properties support different parts of memory hierarchy design:

| Property | Main Question It Answers | Role |
|---|---|---|
| Locality | Which data should be kept near the CPU? | Predicts future accesses from recent/nearby accesses |
| Inclusion | Where else is a cached block stored? | Defines subset relation between hierarchy levels |
| Coherence | Are all copies of a block consistent? | Prevents stale/incorrect values |

Locality improves performance. Inclusion simplifies management. Coherence preserves correctness.

### 10. Example Answer Using The Board Diagram

The board diagram shows a pyramid. At the top are registers, then cache memory, then main memory/RAM, then USB or flash memory, then magnetic disk/hard disk, and finally magnetic tape or tape drives. The top levels store fewer bytes but are very fast. The lower levels store GB, TB, or PB of data but are slower.

Cache memory is usually SRAM, so it is faster and more expensive. Main memory is usually DRAM, so it is larger but slower. SSD/flash and magnetic disk are non-volatile secondary storage. Tape is used for very large backup or archival storage.

This pyramid structure is useful because the CPU mostly needs a small active portion of the program at any time. Due to temporal locality, recently used items are kept in cache. Due to spatial locality, nearby items are fetched together as a cache block. If the block is present in cache, access is fast. If it is absent, the block is fetched from main memory, causing a miss penalty.

### 11. Final 10-Mark Exam Answer To Write

After drawing the memory hierarchy pyramid, write the answer like this:

Hierarchical memory organization is a method of organizing the storage system of a computer into multiple levels according to speed, capacity, cost per bit, and access time. The main idea is that the processor should get most of its required instructions and data from a small fast memory, while the complete program and data are stored in larger lower-level memories. Thus, the hierarchy gives the user the effect of a large memory system while giving the processor an average access time close to the faster levels.

A typical memory hierarchy contains CPU registers at the top, followed by cache memory, main memory, secondary storage, and tertiary storage. Registers are the fastest and smallest storage units and are directly used by the CPU. Cache memory is built using SRAM and stores recently or frequently used blocks of instructions and data. Main memory is built using DRAM and stores currently executing programs. SSDs, hard disks, and flash memories provide large non-volatile secondary storage, while magnetic tapes are used for backup and archival storage.

The need for memory hierarchy arises because there is a large speed gap between the CPU and main memory. Modern processors can execute operations very quickly, but DRAM access takes many CPU cycles, and disk access is much slower. If every instruction and operand were fetched directly from main memory or secondary storage, the CPU would spend much of its time waiting. Therefore, frequently needed data is copied into faster levels such as cache and registers.

The movement of data in a memory hierarchy happens in blocks. When the CPU requests an item, the upper level is checked first. If the item is present, it is called a hit. If the item is absent, it is called a miss, and the required block is fetched from the next lower level. The performance of the memory system can be expressed using average memory access time:

```text
AMAT = Hit Time + Miss Rate x Miss Penalty
```

A good hierarchy tries to reduce hit time, miss rate, and miss penalty. This is why cache design, block size, associativity, replacement policy, and write policy are important.

The effectiveness of memory hierarchy depends mainly on locality of reference. Locality means that programs do not access all memory locations uniformly. Temporal locality means that if a memory location is accessed now, it is likely to be accessed again in the near future. For example, loop instructions and variables used repeatedly show temporal locality. Spatial locality means that if one memory location is accessed, nearby locations are likely to be accessed soon. For example, array traversal and sequential instruction execution show spatial locality. Due to locality, keeping recently used and nearby blocks in cache greatly improves performance.

Inclusion is another important property of a memory hierarchy. Inclusion means that the contents of an upper-level memory are also present in the next lower level. For example, in an inclusive cache hierarchy, every block present in L1 cache is also present in L2 or L3 cache. This is also called the subset property. Inclusion helps in tracking data and maintaining cache coherence, especially in multicore processors where a lower-level shared cache can know which blocks may be present in private upper-level caches. However, inclusion can waste some cache capacity because the same block is stored at more than one level.

Coherence means that all copies of the same memory block must remain consistent. This is necessary because the same block may exist in L1 cache, L2 cache, L3 cache, main memory, or in the private caches of different processor cores. If one processor changes a value and another processor still reads an old cached value, the program may produce wrong results. Therefore, coherence mechanisms are used to maintain correctness. In a single processor, write-through and write-back policies maintain consistency between cache and memory. In multiprocessor systems, snooping and directory-based cache coherence protocols are used to invalidate or update stale copies.

Thus, memory hierarchy improves performance by combining small fast memories with large slow memories. Locality makes it efficient, inclusion helps organize and track data across levels, and coherence ensures correctness when multiple copies of data exist.

### 12. Key Points To Remember

- Memory hierarchy is arranged by speed, capacity, and cost.
- Registers are fastest and smallest.
- Cache uses SRAM; main memory uses DRAM.
- SSD/HDD/tape provide large non-volatile storage.
- Locality is the reason caches work.
- Temporal locality means reuse of recently accessed items.
- Spatial locality means use of nearby addresses.
- Inclusion means upper-level cache contents are also found in lower levels.
- Coherence means all copies of the same data are consistent.
- AMAT formula: `Hit Time + Miss Rate x Miss Penalty`.

### 13. Short 5-Mark Version

Memory hierarchy is a layered organization of storage devices. The fastest and smallest memories, such as registers and cache, are placed near the CPU, while slower and larger memories, such as RAM, SSD, HDD, and tape, are placed lower in the hierarchy. This gives the system the effect of a large memory with fast average access time.

It works because of locality. Temporal locality means recently used data is likely to be used again. Spatial locality means data near recently accessed addresses is likely to be used soon. Therefore, cache stores recently used blocks from main memory.

Inclusion means data in an upper level is also available in a lower level, such as L1 being a subset of L2 or L3. Coherence means that if multiple copies of a data block exist, all copies must be kept consistent after reads and writes. These properties make the memory hierarchy both fast and correct.

### 14. Common Diagram To Draw In Exam

```text
          Registers
             |
          L1 Cache
             |
          L2 Cache
             |
          L3 Cache
             |
        Main Memory
             |
       SSD / Flash
             |
        Hard Disk
             |
       Magnetic Tape

Top: fast, small, costly
Bottom: slow, large, cheap
```

---

## Sources Used For Q2

- [Virtual memory.ppt](<MaterialToRefer/Virtual memory.ppt>) - demand paging, address translation, page table, TLB, page replacement, paging, segmentation.
- [Memory.pdf](<MaterialToRefer/Memory.pdf>) - virtual memory organization, MMU, address translation, segmentation, TLB.
- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - Appendix B virtual memory, page tables, TLB, paging versus segmentation.

---

<a id="q2"></a>

## Q2. Discuss Memory Management Techniques Used In Virtual Memory Systems. Explain Paging And Segmentation With Examples.

**CLO:** CLO 3 - Memory Organization and Management.

### Figures To Use

![Address translation in a paging system](<images/solutions/q2/virtual_memory_ppt_export/Slide24.PNG>)

This is the main diagram to draw. It shows that a virtual address is split into `page number + offset`; the page number goes to the page table, the page table gives the frame number, and `frame number + same offset` becomes the physical address. This diagram directly supports paging and address translation.

![Book page-table mapping figure](<images/solutions/q2/book_figures/virtual_to_physical_page_table_book_fig_b23.png>)

Use this as the cleanest book version of the paging diagram. It is easy to redraw: virtual page number goes through the page table, while the page offset is copied unchanged.

![Use of Translation Lookaside Buffer](<images/solutions/q2/virtual_memory_ppt_export/Slide29.PNG>)

Use this if you want extra marks. It shows the real flow: first check TLB, then page table, and if the page is absent, a page fault loads the page from secondary memory. This explains why virtual memory is practical despite page tables being in memory.

![Paging versus segmentation comparison](<images/solutions/q2/book_figures/paging_vs_segmentation_book_fig_b21_b22.png>)

Use this to remember the difference: paging divides memory into fixed-size pages, while segmentation divides a program into logical variable-size parts such as code, data, stack, and heap.

### Answer To Write

Virtual memory is a memory management technique in which the logical address space seen by a program is separated from the physical main memory available in the computer. The program behaves as if it has a large continuous memory, but in reality only the required parts of the program are kept in RAM. The remaining parts are kept in secondary storage and brought into main memory only when needed. This is useful because physical memory is limited, while programs can be much larger than RAM.

The CPU generates a virtual address during execution. This virtual address is translated into a physical address by the Memory Management Unit (MMU). The operating system maintains mapping information using page tables or segment tables. It also handles page faults, page replacement, protection, and sharing. The important techniques used in virtual memory are demand paging, paging, segmentation, page tables, TLB, page replacement, and protection bits.

In demand paging, a page is loaded into main memory only when it is required. If a process refers to a page that is not currently in RAM, a page fault occurs. The operating system then finds the required page on disk, loads it into a free frame, updates the page table, and restarts the instruction. If there is no free frame, a page replacement algorithm such as FIFO, LRU, Clock, or Optimal is used to remove one old page. This is why virtual memory saves RAM: all pages of a process are not loaded at once.

Paging divides virtual memory into fixed-size blocks called pages and physical memory into equal-size blocks called frames. The virtual address is divided into page number and offset. The page number is used to index the page table, which gives the frame number. The offset remains the same because the position inside the page and frame is unchanged.

```text
Virtual address  = Page number + Offset
Physical address = Frame number + Offset
```

A page table entry generally contains the frame number, valid bit, dirty bit, reference bit, and protection bits. If the valid bit is 0, the page is not present in RAM and a page fault occurs. Since page tables are stored in memory, address translation can become slow. To reduce this overhead, a Translation Lookaside Buffer (TLB) is used. A TLB is a small fast cache that stores recently used page table entries. On a TLB hit, the frame number is obtained quickly. On a TLB miss, the page table is checked.

Example of paging: suppose the virtual address is `3045`, page size is `2 KB = 2048 bytes`, and page 1 maps to frame 5.

```text
Page number = floor(3045 / 2048) = 1
Offset      = 3045 - 2048 = 997
Physical address = 5 x 2048 + 997 = 11237
```

So virtual address `3045` maps to physical address `11237`.

Segmentation divides a program into logical variable-size units instead of fixed-size pages. A segment may represent code, data, stack, heap, or a procedure. A logical address in segmentation has a segment number and an offset. The segment number selects an entry in the segment table. Each segment table entry contains a base address and a limit. The offset is first checked against the limit. If it is valid, the physical address is calculated as `base + offset`; otherwise, a segmentation fault occurs.

Example of segmentation: if segment 1 has base address `8000` and limit `2000`, then logical address `<1, 1200>` is valid because `1200 < 2000`. The physical address is:

```text
Physical address = 8000 + 1200 = 9200
```

But if segment 0 has limit `1000`, then `<0, 1200>` is invalid because the offset exceeds the segment limit.

Paging and segmentation solve related problems in different ways. Paging is easier for the operating system because all pages and frames are the same size, and any page can be placed in any free frame. Its main drawback is internal fragmentation. Segmentation is closer to the programmer's view because it separates code, data, stack, and heap naturally. Its main drawback is external fragmentation because segments are variable in size.

| Point | Paging | Segmentation |
|---|---|---|
| Division | Fixed-size pages | Variable-size logical segments |
| Address | Page number + offset | Segment number + offset |
| Visibility | Usually invisible to programmer | May be visible to programmer |
| Fragmentation | Internal fragmentation | External fragmentation |
| Replacement | Easier, same-size pages | Harder, variable-size segments |

Thus, virtual memory improves memory utilization, allows execution of large programs, supports protection and sharing, and helps multiprogramming. Paging is more common because it is simple and efficient, while segmentation is useful for representing the logical structure of a program. Many systems combine both ideas using paged segmentation.

### Draw In Exam

Draw the paging address translation diagram first. If time remains, draw a small segmentation table showing `segment number -> base + offset` and write one line comparing paging and segmentation.
## Sources Used For Q3

- [pp2.ppt](<MaterialToRefer/pp2.ppt>) - cache coherence problem, snoopy protocol, write-invalidate, write-back protocol state machines.
- [ParallelArchitecture_PP.ppt](<MaterialToRefer/ParallelArchitecture_PP.ppt>) - shared memory caches, cache coherence problem, invalidation-based coherence.
- [ParallelArchitecture.pdf](<MaterialToRefer/ParallelArchitecture.pdf>) - shared-memory multiprocessors, UMA/NUMA, cc-NUMA and coherence requirement.
- [DSM.ppt](<MaterialToRefer/DSM.ppt>) - memory coherence, consistency, write-invalidate/write-update.
- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - Chapter 5 cache coherence, snooping, directory-based coherence.

---

<a id="q3"></a>

## Q3. Explain Coherence Problems In Shared Memory Systems. Discuss Cache Coherence Protocols In Multiprocessor Systems.

**CLO:** CLO 3 - Memory Organization and Management; CLO 4 - Advanced Architectures.

### Figures To Use

![Example cache coherence problem](<images/solutions/q3/pp2_ppt_export/Slide24.PNG>)

This is the main problem diagram. It shows processors with private caches connected to shared memory. One processor writes a new value, but other caches may still contain the old value. Draw this first because it explains why coherence is needed.

![Snoopy cache-coherence protocols](<images/solutions/q3/pp2_ppt_export/Slide30.PNG>)

This is the main protocol diagram. It shows that every cache controller watches, or snoops, the shared bus. If a bus transaction affects a block present in a cache, that cache invalidates, updates, or supplies the value.

![Two classes of cache coherence protocols](<images/solutions/q3/pp2_ppt_export/Slide29.PNG>)

This slide is useful for structure. It reminds you that the two main protocol families are snooping and directory-based protocols.

![Directory-based cache coherence state diagram](<images/solutions/q3/book_figures/book_directory_state_transition_fig_5_20.png>)

Use this if you explain directory-based coherence in depth. It shows block states like Invalid, Shared, and Modified, and the messages used for read misses, write misses, invalidations, and write-backs.

![Write-through invalidate protocol state diagram](<images/solutions/q3/pp2_ppt_export/Slide38.PNG>)

Use this only if the question asks for protocol operation/state diagram. It shows the simple valid/invalid idea: a write by one processor invalidates other cached copies.

### Answer To Write

In a shared-memory multiprocessor system, all processors share the same main memory address space. Communication between processors happens by reading and writing shared variables in memory. To reduce memory access time, each processor usually has its own private cache. This improves performance, but it also creates the cache coherence problem.

The problem occurs because the same memory block can exist in many places at the same time: in main memory, in processor P1's cache, in processor P2's cache, and so on. If one processor changes its cached copy, the other processors may continue to read old copies from their own caches. This means different processors can see different values for the same memory location.

Example: assume memory location `X = 5`. Processor P1 reads `X`, so P1's cache stores `X = 5`. Processor P2 also reads `X`, so P2's cache also stores `X = 5`. Now processor P3 writes `X = 7`. If P1 and P2 are not informed, they may still read `X = 5`. This is stale data, and it is incorrect for shared-memory programming.

A coherent memory system must satisfy three basic rules. First, if a processor writes a value and then reads the same location, it should get the value it wrote. Second, a write by one processor must eventually become visible to other processors. Third, all processors must see writes to the same memory location in the same order. The third point is called write serialization. Coherence is mainly about correctness for one memory location, while consistency deals with ordering of accesses to different memory locations.

Cache coherence protocols are used to maintain correct values among multiple cached copies. The two main types are snooping protocols and directory-based protocols.

In a snooping protocol, all caches are connected through a shared bus or broadcast medium. Every cache controller monitors all bus transactions. If a processor reads or writes a block, the transaction appears on the bus. Other caches check whether they have that block. If they do, they take the required action: invalidate the block, update it, supply the latest value, or change the block state. Snooping is simple and works well for small shared-memory multiprocessors, but it does not scale well because every transaction is broadcast to all caches.

The most common snooping method is the write-invalidate protocol. Before a processor writes to a shared cache block, it obtains exclusive access to that block. Other cached copies are marked invalid. After that, no other processor can read the old value. If another processor later reads that block, it gets a cache miss and fetches the updated value.

```text
Initial: P1 cache has X = 5, P2 cache has X = 5
P1 writes X = 7
P1 sends invalidate on bus
P2 marks its copy invalid
If P2 reads X again, it fetches updated X = 7
```

Write-invalidate is preferred because it uses less bus bandwidth when there are many writes. The alternative is write-update, also called write-broadcast. In write-update, when one processor writes a shared block, the new value is sent to all caches that hold that block. This keeps all copies updated, but it creates high bus traffic because every write must be broadcast.

Many protocols maintain a state for each cache block. In MSI, the states are Modified, Shared, and Invalid. Modified means this cache has the only updated copy and memory may be stale. Shared means one or more caches may have the block and memory is up to date. Invalid means the cache copy cannot be used. MESI adds an Exclusive state, where one cache has the only clean copy. This reduces bus traffic because a processor can write an exclusive block without first invalidating other caches.

For larger multiprocessor systems, directory-based protocols are used. Here, a directory keeps track of which caches have each memory block and whether the block is shared or modified. On a read miss, the directory supplies the block or contacts the owner. On a write, it sends invalidation messages only to caches that actually have that block. This is more scalable than snooping because it avoids broadcasting every request to every processor.

The directory method gives better scalability, but it adds directory storage and more protocol messages. That is the main tradeoff: snooping is simpler and fast for small bus-based systems, while directory coherence is better for larger NUMA or distributed shared-memory systems.

| Protocol | Idea | Best use |
|---|---|---|
| Snooping | Every cache watches the bus | Small UMA/SMP systems |
| Directory-based | Directory stores sharer/owner information | Large NUMA/DSM systems |
| Write-invalidate | Writer invalidates other copies | Most common, lower bandwidth |
| Write-update | Writer broadcasts new value | Useful when many processors read after every write |

Thus, coherence problems occur because private caches may store stale copies of shared data. Cache coherence protocols solve this by ensuring that writes are propagated and stale copies are invalidated or updated. Snooping is simpler, while directory-based coherence is more scalable.

### Draw In Exam

Draw the cache coherence problem diagram first: three processors with private caches and one shared memory. Show old value `X = 5` in some caches and new value `X = 7` after one processor writes. Then draw a bus-based snooping diagram and write: "cache controllers snoop bus and invalidate/update copies."

## Sources Used For Q4

- [pp1.ppt](<MaterialToRefer/pp1.ppt>) - ILP definition, loop-level parallelism, loop unrolling, branch prediction, dynamic scheduling, Tomasulo algorithm.
- [Pipelining.pdf](<MaterialToRefer/Pipelining.pdf>) - pipeline stalls, hazards, CPI effect, superscalar and superpipelining basics.
- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - Chapter 3, instruction-level parallelism and its exploitation.

---

<a id="q4"></a>

## Q4. Explain The Concept Of Instruction-Level Parallelism (ILP). Discuss Various Techniques Used To Increase ILP.

**CLO:** CLO 4 - ILP and Advanced Architectures.

### Figures To Use

![ILP definition and two approaches](<images/solutions/q4/pp1_ppt_export/Slide3.PNG>)

This slide gives the clean structure of the answer: ILP means overlapping instructions, and it can be exploited by hardware dynamically or by software/compiler statically.

![Loop-level parallelism](<images/solutions/q4/pp1_ppt_export/Slide5.PNG>)

Use this for the "why loops matter" part. Most useful ILP comes from loops because different loop iterations may be independent and can be overlapped after unrolling or scheduling.

![Two-bit dynamic branch predictor](<images/solutions/q4/pp1_ppt_export/Slide27.PNG>)

This is a good small diagram to draw if branch prediction is asked. It shows that prediction changes only after repeated wrong outcomes, so loops are predicted better than with a one-bit predictor.

![Tomasulo organization](<images/solutions/q4/pp1_ppt_export/Slide45.PNG>)

Use this as the main hardware ILP diagram. It shows reservation stations, functional units, load/store buffers, registers, and the common data bus used for dynamic scheduling.

### Answer To Write

Instruction-Level Parallelism (ILP) is the ability of a processor to overlap or execute more than one instruction from the same program at the same time. In a purely sequential processor, one instruction completes before the next one starts. In a pipelined or superscalar processor, different instructions can be in different stages, or even execute on different functional units, during the same clock cycle. The aim of ILP is to reduce CPI and increase instruction throughput.

ILP exists because many instructions in a program are independent. For example:

```text
I1: R1 = R2 + R3
I2: R4 = R5 + R6
I3: R7 = R1 + R8
```

`I1` and `I2` are independent, so they can execute in parallel. But `I3` depends on the result of `I1`, so it must wait. Therefore, ILP is limited mainly by dependences. True data dependences cause RAW hazards, name dependences cause WAR and WAW hazards, and branches cause control hazards. The processor or compiler tries to find independent instructions and keep only the necessary order.

The first technique to increase ILP is pipelining. In pipelining, instruction execution is divided into stages such as instruction fetch, decode, execute, memory access, and write back. Different instructions occupy different stages at the same time. Pipelining does not reduce the latency of one instruction much, but it increases throughput because ideally one instruction completes every cycle.

The second technique is compiler scheduling. The compiler reorders independent instructions so that useful work is done during cycles that would otherwise become stalls. For example, after a load instruction, the compiler can place an independent arithmetic instruction before the loaded value is used. This hides load-use delay without changing the final result.

Loop unrolling is another important compiler technique. In loops, the same instruction pattern repeats many times. By unrolling the loop, the compiler reduces branch overhead and exposes independent instructions from different iterations. For example, instead of doing one array element per loop iteration, the compiler may do four elements per iteration. Then loads, additions, and stores from different iterations can be scheduled together. Register renaming is usually needed here so that different iterations do not unnecessarily reuse the same registers.

Branch prediction and speculation also increase ILP. Branches break the instruction stream because the processor may not know the next correct instruction address. A branch predictor guesses the branch direction and target so that fetching and execution can continue. If the prediction is correct, many cycles are saved. If it is wrong, the speculative instructions are cancelled. Dynamic predictors, such as two-bit predictors and branch history tables, improve performance because program branches usually repeat patterns.

Hardware techniques increase ILP dynamically at run time. A superscalar processor has multiple functional units and can issue more than one instruction per clock cycle. For this to work, the hardware must check dependences and issue only instructions that can safely run together. VLIW also executes multiple operations per cycle, but the compiler packs the independent operations into one wide instruction, so more responsibility is placed on software.

Dynamic scheduling is a stronger hardware technique. It allows instructions after a stalled instruction to proceed if their operands are ready. This gives out-of-order execution while preserving correct data flow. Tomasulo's algorithm is a classic dynamic scheduling method. It uses reservation stations, register renaming, and a common data bus. Reservation stations hold waiting instructions near functional units. Register renaming removes false WAR and WAW dependences. The common data bus broadcasts completed results to all waiting instructions, so dependent instructions can start as soon as their operands are ready.

The main techniques for increasing ILP can be summarized as follows:

| Technique | How it increases ILP |
|---|---|
| Pipelining | Overlaps stages of different instructions |
| Compiler scheduling | Reorders independent instructions to reduce stalls |
| Loop unrolling | Exposes independent work across loop iterations |
| Register renaming | Removes false WAR and WAW dependences |
| Branch prediction | Keeps the pipeline full across branches |
| Speculation | Executes predicted-path instructions early |
| Superscalar issue | Issues multiple independent instructions per cycle |
| Dynamic scheduling | Executes ready instructions out of order |
| VLIW/static multiple issue | Compiler packs independent operations into wide instructions |

Thus, ILP improves processor performance by finding independent instructions and executing them in an overlapped way. Its benefit is limited by true data dependences, branch mispredictions, memory dependences, structural hazards, finite registers, and hardware complexity. A good ILP design combines compiler support, prediction, multiple issue, and dynamic scheduling.

### Draw In Exam

Draw the Tomasulo organization diagram if the answer needs hardware detail: instruction queue, reservation stations, functional units, registers, load/store buffers, and common data bus. If the question is more general, draw a simple pipeline/multiple-issue diagram and a small two-bit branch predictor state diagram.

## Sources Used For Q5

- [Pipelining.pdf](<MaterialToRefer/Pipelining.pdf>) - superpipelining and superscalar summary.
- [pp1.ppt](<MaterialToRefer/pp1.ppt>) - ILP background, dynamic scheduling, Tomasulo organization.
- [pp2.ppt](<MaterialToRefer/pp2.ppt>) - limits of multiple-issue ILP and note on Itanium/EPIC/VLIW.
- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - Chapter 3 multiple issue; Appendix H VLIW/EPIC.

---

<a id="q5"></a>

## Q5. Compare Superscalar, Super-Pipelined, And VLIW Processor Architectures With Suitable Block Diagrams.

**CLO:** CLO 2 - Pipelining and RISC Performance; CLO 4 - ILP and Advanced Architectures.

### Figures To Use

![Exam block diagrams for superscalar, super-pipelined, and VLIW](<images/solutions/q5/q5_exam_block_diagrams.png>)

This is the best figure to draw in the exam. It keeps the three architectures visually separate: superscalar widens issue, super-pipelined deepens stages, and VLIW uses compiler-packed wide instructions.

![Local note on superpipelining and superscalar](<images/solutions/q5/book_figures/superpipelining_superscalar_notes.png>)

This local note gives the short definitions: superpipelining increases pipeline stages and clock frequency, while superscalar issues multiple instructions per clock.

![Multiple-issue approaches from textbook](<images/solutions/q5/book_figures/multiple_issue_approaches_book_fig_3_19.png>)

This table is useful for comparison. It shows that superscalar processors use dynamic issue structure and hardware hazard detection, while VLIW mainly relies on compiler scheduling.

![Multiple-issue superscalar organization](<images/solutions/q5/book_figures/multiple_issue_superscalar_book_fig_3_21.png>)

Use this only if you want a richer superscalar block diagram. It shows instruction queue, reservation stations, multiple functional units, reorder buffer, registers, and common data bus.

![VLIW/EPIC instruction slots](<images/solutions/q5/book_figures/vliw_epic_slots_book_fig_h_7.png>)

This shows the VLIW/EPIC idea of instruction slots. One wide instruction bundle contains several operation fields, and each field is meant for a specific functional unit type.

### Answer To Write

Superscalar, super-pipelined, and VLIW processors are techniques used to increase processor performance by exploiting instruction-level parallelism. All three try to complete more instructions per unit time, but they do it in different ways. Superscalar increases the number of instructions issued per clock, super-pipelining increases the number of pipeline stages to raise clock frequency, and VLIW shifts most scheduling work to the compiler.

In a superscalar processor, the hardware fetches and decodes multiple instructions and issues more than one independent instruction in the same clock cycle. It has multiple functional units such as ALUs, load/store units, and floating-point units. The processor checks dependences in hardware and sends only safe instructions to execute together. Modern superscalar processors may also use out-of-order execution, register renaming, branch prediction, speculation, reservation stations, and a reorder buffer.

```text
Instruction stream -> Fetch/Decode -> Issue + hazard check
                                      |      |      |
                                     ALU   Load    FP
                                      \      |     /
                                      Writeback/Commit
```

The advantage of superscalar design is that the same normal instruction stream can run faster without needing the programmer to explicitly specify parallel operations. It is flexible and handles dynamic events such as cache misses better. The disadvantage is hardware complexity: issue logic, dependency checking, register renaming, and speculation become expensive as issue width increases.

In a super-pipelined processor, the normal pipeline is divided into more and smaller stages. Since each stage does less work, the clock cycle can be reduced and the clock frequency can be increased. For example, instead of one `IF` stage and one `EX` stage, the processor may split them into `IF1`, `IF2`, `EX1`, and `EX2`.

```text
IF1 -> IF2 -> ID1 -> ID2 -> EX1 -> EX2 -> MEM -> WB
```

Super-pipelining mainly improves throughput by allowing a higher clock rate. It does not mean that many instructions are issued in the same cycle; usually it is still a narrow issue pipeline unless it is also superscalar. Its main disadvantage is that deeper pipelines increase branch misprediction penalty and make hazard handling more difficult.

In a VLIW processor, VLIW means Very Long Instruction Word. The compiler finds independent operations and packs them into one long instruction word. Each field of the long instruction controls a different functional unit. For example, one VLIW instruction may contain one integer operation, one memory operation, one floating-point operation, and one branch operation.

```text
Compiler scheduler -> [ ALU op | MEM op | FP op | BR op ]
                         |        |        |       |
                        ALU      Load      FP    Branch
```

The main idea is that the compiler performs scheduling statically before the program runs. Therefore, VLIW hardware can be simpler than a complex out-of-order superscalar processor because it does not need as much dynamic dependency-checking logic. The disadvantages are larger code size, unused instruction slots when enough parallelism is not available, dependence on compiler quality, and difficulty handling unpredictable events like cache misses and branches.

| Feature | Superscalar | Super-pipelined | VLIW |
|---|---|---|---|
| Main idea | Issue multiple instructions per clock | Split pipeline into more stages | Pack multiple operations in one wide instruction |
| Parallelism found by | Hardware, sometimes with compiler help | Pipeline timing/hardware | Compiler |
| Issue width | More than one instruction per cycle | Usually one, unless combined with superscalar | Fixed number of operation slots |
| Hazard handling | Hardware detects and resolves hazards | Pipeline interlocks/forwarding/stalls | Mostly compiler scheduling |
| Hardware complexity | High | Medium to high as depth increases | Lower issue logic, but needs compiler support |
| Code size | Normal instruction stream | Normal instruction stream | Larger because of wide instructions and empty slots |
| Branch effect | Managed by prediction/speculation | More branch penalty due to depth | Compiler scheduling/predication/speculation support |
| Example idea | Intel Core/i7 style dynamic superscalar | Pentium 4 style deep pipeline | Itanium/EPIC, TI C6x DSP |

The important comparison is: superscalar is a wide processor controlled mainly by hardware, super-pipelined is a deep processor controlled by faster stage timing, and VLIW is a wide processor controlled mainly by the compiler. Superscalar improves performance by issuing several independent instructions dynamically. Super-pipelining improves performance by increasing clock frequency. VLIW improves performance by using compiler-generated long instruction words that explicitly specify parallel operations.

Thus, superscalar is more flexible but hardware-heavy, VLIW is simpler in hardware but compiler-dependent, and super-pipelining improves clock speed but suffers more from branch and hazard penalties. In practice, high-performance processors often combine ideas, such as deep pipelining plus superscalar issue plus branch prediction.

### Draw In Exam

Draw three small diagrams side by side. For superscalar, show one fetch/decode block feeding an issue unit and three functional units. For super-pipelined, show one long pipeline split into many stages like `IF1, IF2, ID1, ID2, EX1, EX2, MEM, WB`. For VLIW, show a compiler producing one wide instruction with slots connected to fixed functional units.

## Sources Used For Q6

- [ParallelArchitecture_PP.ppt](<MaterialToRefer/ParallelArchitecture_PP.ppt>) - Flynn classification slides and examples.
- [ParallelArchitecture.pdf](<MaterialToRefer/ParallelArchitecture.pdf>) - instruction/data streams and SISD/SIMD/MISD/MIMD explanation.
- [pp2.ppt](<MaterialToRefer/pp2.ppt>) - Flynn taxonomy linked to SIMD data-level parallelism and MIMD thread-level parallelism.
- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - Chapter 1/4 context for SISD, SIMD, and MIMD.

---

<a id="q6"></a>

## Q6. Explain Flynn's Taxonomy Of Parallel Architectures With Suitable Examples.

**CLO:** CLO 4 - ILP and Advanced Architectures.

### Figures To Use

![Flynn taxonomy matrix](<images/solutions/q6/q6_flynn_taxonomy_matrix.png>)

This is the best exam diagram. It classifies machines using two axes: instruction stream and data stream. Draw this first, then explain each quadrant.

![Flynn classification slide 1](<images/solutions/q6/parallel_architecture_pp/Slide2.PNG>)

This PPT slide gives SISD and SIMD examples. Use it to remember that SIMD includes vector processors and processor arrays.

![Flynn classification slide 2](<images/solutions/q6/parallel_architecture_pp/Slide3.PNG>)

This slide gives MISD and MIMD. MIMD is the most common category for multiprocessors, clusters, and grids.

![Flynn taxonomy and parallelism types](<images/solutions/q6/pp2_ppt_export/Slide12.PNG>)

Use this source slide to add a strong final point: SIMD mainly exploits data-level parallelism, while MIMD mainly exploits thread-level parallelism.

### Answer To Write

Flynn's taxonomy is a classification of computer architectures based on the number of instruction streams and data streams processed by a system. An instruction stream is the sequence of instructions executed by a processing unit. A data stream is the sequence of data items operated on by those instructions. Each stream may be single or multiple, giving four classes: SISD, SIMD, MISD, and MIMD.

SISD means Single Instruction Single Data. It is the conventional sequential model. One processor executes one instruction stream on one data stream. A simple single-core scalar processor is the usual example. Even if a modern core internally uses pipelining, the programmer still sees one sequential instruction stream, so a single scalar core is generally treated as SISD.

SIMD means Single Instruction Multiple Data. One control unit broadcasts the same instruction to many processing elements, and each element applies it to different data. This is useful when the same operation is repeated over arrays, pixels, vectors, or matrix elements. Examples are vector processors, GPU SIMD/SIMT execution units, multimedia extensions such as SSE/AVX/NEON, and processor arrays. Applications include image processing, scientific vector computation, machine learning kernels, and matrix multiplication.

MISD means Multiple Instruction Single Data. Multiple instruction streams operate on the same data stream. It is rare in commercial systems because most real workloads do not naturally require different programs to operate on exactly the same data stream at the same time. It is usually discussed in fault-tolerant or redundant pipeline systems, where the same input may be processed by different units for reliability.

MIMD means Multiple Instruction Multiple Data. Multiple processors execute different instruction streams on different data sets. This is the most general and widely used form of parallel architecture. Examples include multicore processors, shared-memory multiprocessors, distributed-memory clusters, and warehouse-scale systems. MIMD supports both independent programs and one parallel program divided into cooperating threads or processes.

In exam language, SIMD is best connected with data-level parallelism because one operation is applied to many data elements. MIMD is best connected with thread-level or task-level parallelism because many instruction streams can run independently or cooperate on a larger problem.

| Class | Instruction stream | Data stream | Example | Main use |
|---|---|---|---|---|
| SISD | Single | Single | Single scalar CPU core | Sequential programs |
| SIMD | Single | Multiple | GPU, vector processor, AVX | Data-parallel work |
| MISD | Multiple | Single | Fault-tolerant/redundant pipelines | Rare/specialized |
| MIMD | Multiple | Multiple | Multicore, SMP, cluster | General parallel processing |

The main benefit of Flynn's taxonomy is that it gives a simple way to classify parallel machines. Its limitation is that it is coarse: it does not fully describe memory organization, interconnection network, cache coherence, synchronization, or programming model. For example, both a shared-memory multicore and a distributed cluster are MIMD, but they behave very differently.

### Draw In Exam

Draw a 2x2 matrix. Columns: single instruction and multiple instructions. Rows: single data and multiple data. Fill the cells as SISD, MISD, SIMD, and MIMD, and write one example in each cell.

## Sources Used For Q7

- [pp2.ppt](<MaterialToRefer/pp2.ppt>) - centralized vs distributed memory, SMPs, shared-memory communication model, cache coherence in SMPs.
- [ParallelArchitecture_PP.ppt](<MaterialToRefer/ParallelArchitecture_PP.ppt>) - shared memory vs message passing and UMA/NUMA classification.
- [ParallelArchitecture.pdf](<MaterialToRefer/ParallelArchitecture.pdf>) - shared-memory multiprocessor architecture, UMA, NUMA, access control, synchronization.
- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - shared-memory multiprocessors and synchronization basics.

---

<a id="q7"></a>

## Q7. Discuss Centralized Shared-Memory Multiprocessor Architecture. Explain Synchronization Techniques Used In Shared-Memory Systems.

**CLO:** CLO 4 - ILP and Advanced Architectures.

### Figures To Use

![Centralized shared memory and synchronization](<images/solutions/q7/q7_centralized_shared_memory_sync.png>)

This is the best exam diagram. It shows multiple processors with private caches connected through a shared bus/switch to centralized memory banks, plus a simple lock flow.

![Centralized versus distributed memory](<images/solutions/q7/pp2_ppt_export/Slide14.PNG>)

This PPT figure helps you explain the difference: centralized memory has one shared memory system; distributed memory places memory with nodes.

![Centralized memory multiprocessor](<images/solutions/q7/pp2_ppt_export/Slide15.PNG>)

Use this source slide for SMP points: symmetric relationship to memory, good for a small to moderate number of processors, and limited scalability.

![Communication and memory models](<images/solutions/q7/pp2_ppt_export/Slide17.PNG>)

This slide helps connect centralized memory with UMA/shared-address-space programming.

![SMP cache coherence issue](<images/solutions/q7/pp2_ppt_export/Slide23.PNG>)

Use this figure to explain why private caches improve latency but create the cache-coherence problem for shared data.

![Spin lock coherence traffic](<images/solutions/q7/book_figures/book_spin_lock_coherence_page_0448.png>)

Use this book figure only if the examiner asks deeply about synchronization. It shows why many processors spinning on one lock create bus/directory traffic and invalidations.

### Answer To Write

A centralized shared-memory multiprocessor is a parallel computer in which two or more processors share one physical main memory and one global address space. Each processor can access any memory location using normal load and store instructions. Because all processors have equal access to the same memory system, this organization is also called a symmetric multiprocessor (SMP), and when memory access time is approximately the same for all processors it is called UMA, or Uniform Memory Access.

The architecture normally contains several processors, private caches for each processor, a shared interconnection medium such as a bus or switch, memory banks, and I/O devices. The private caches reduce average memory access time and reduce traffic to main memory. The shared memory gives an easy programming model because communication occurs through shared variables instead of explicit send and receive messages.

The main advantage is programmability. Threads can communicate by reading and writing shared data structures. The operating system can schedule threads on any processor, and all processors see the same address space. Another advantage is low communication overhead for small systems, because shared data can be cached locally and updated through coherence protocols.

The main limitation is scalability. As the number of processors increases, the shared bus, memory banks, and coherence traffic become bottlenecks. Many processors may try to access memory at the same time. If the system has private caches, it also needs cache coherence so that processors do not read stale values of shared variables. Therefore, centralized shared-memory systems are good for small to medium multiprocessors, but large systems usually use distributed memory or cc-NUMA designs.

Synchronization is required because multiple processors may access the same shared data at the same time. If two processors update a shared variable without coordination, a race condition can occur. A race condition means the final result depends on the timing/order of execution, not only on the program logic.

The basic synchronization technique is a lock or mutex. Before entering a critical section, a processor must acquire the lock. Only the processor that gets the lock can execute the critical section. After finishing, it releases the lock. A simple lock uses an atomic read-modify-write instruction such as test-and-set, compare-and-swap, or load-linked/store-conditional. Atomic means the read and write occur as one indivisible operation, so two processors cannot both acquire the lock at the same time.

```text
while (test_and_set(lock) == 1) {
    wait;
}
critical section;
lock = 0;
```

Spinlocks are useful when the waiting time is very short, because the processor repeatedly checks the lock. Mutexes are better for longer waits because the operating system can block the thread and allow another thread to run. Semaphores generalize locks by maintaining a count and are useful for controlling access to multiple identical resources. Barriers are used when all threads must reach a point before any one continues. Memory fences are used to enforce ordering of memory operations, especially in processors that allow reordering.

A lock can become a performance bottleneck in a coherent shared-memory system. If many processors repeatedly perform atomic writes on the same lock, the lock block keeps moving between caches and produces invalidation traffic. A better spin-lock style is to spin mostly using ordinary reads and perform the atomic operation only when the lock appears free; exponential backoff can also reduce simultaneous retries.

Synchronization must be designed carefully. Too little synchronization causes incorrect results. Too much synchronization reduces parallelism and increases contention. Lock variables can also cause heavy cache coherence traffic because many processors repeatedly read and write the same cache block. Good shared-memory programs minimize critical sections, avoid false sharing, and use scalable synchronization primitives.

### Draw In Exam

Draw processors with private caches connected to one shared bus/switch and common memory banks. Then draw a small lock flow: `test-and-set -> critical section -> unlock`. Write one line: "Synchronization protects shared data from races."

## Sources Used For Q8

- [DSM.ppt](<MaterialToRefer/DSM.ppt>) - DSM definition, architecture, mapping manager, advantages, algorithms, coherence, consistency, granularity, IVY case study.
- [ParallelArchitecture_PP.ppt](<MaterialToRefer/ParallelArchitecture_PP.ppt>) - NUMA and distributed shared-memory classification.
- [ParallelArchitecture.pdf](<MaterialToRefer/ParallelArchitecture.pdf>) - distributed memory, NUMA, message-passing contrast, scalability.
- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - distributed shared memory and directory coherence background.

---

<a id="q8"></a>

## Q8. Explain Distributed Shared-Memory Architecture In Detail. Discuss Its Advantages, Limitations, And Applications.

**CLO:** CLO 3 - Memory Organization and Management; CLO 4 - ILP and Advanced Architectures.

### Figures To Use

![Distributed shared memory architecture](<images/solutions/q8/q8_dsm_architecture.png>)

This is the best exam diagram. It shows several nodes, each with CPU, local memory, and mapping manager, connected by a communication network while presenting one shared address space.

![DSM definition from PPT](<images/solutions/q8/dsm_ppt_export/Slide2.PNG>)

This source slide gives the core definition: DSM implements a shared-memory model on distributed systems that do not have physical shared memory.

![DSM mapping manager diagram](<images/solutions/q8/dsm_ppt_export/Slide3.PNG>)

Use this when explaining address translation. Each node has a mapping manager that maps shared addresses to local or remote physical memory.

![DSM advantages](<images/solutions/q8/dsm_ppt_export/Slide4.PNG>)

This slide is useful for the advantages part: implicit sharing, easier pointer-based structures, locality, low-cost hardware, and large total memory.

![DSM coherence protocols](<images/solutions/q8/dsm_ppt_export/Slide11.PNG>)

Use this for coherence: write-invalidate and write-update are the two important approaches.

![DSM read-replication algorithm](<images/solutions/q8/dsm_ppt_export/Slide7.PNG>)

This figure is useful for the read-heavy case. It shows that DSM may keep several read-only copies, but only one writer should own the current writable copy.

![DSM memory coherence and consistency](<images/solutions/q8/dsm_ppt_export/Slide9.PNG>)

Use this source slide to distinguish coherence from consistency. Coherence is about the correct value of one location; consistency is about the visible order of operations.

![DSM granularity design issue](<images/solutions/q8/dsm_ppt_export/Slide12.PNG>)

This is a good extra exam figure for limitations. It explains why page/object size matters: large pages reduce transfer overhead but can increase contention and false sharing.

### Answer To Write

Distributed Shared Memory (DSM) is an architecture in which physically separate memories in different nodes are made to appear as one shared address space. Each node has its own processor, local memory, and communication interface. There is no single physical memory module shared by all processors, but the software/hardware DSM layer gives the programmer a shared-memory abstraction.

The main idea is that a process can access a shared address as if it were local. If the required page or object is already in local memory, the access is fast. If it is not local, a page fault or object fault occurs. The DSM mapping manager locates the owner of the page/object, transfers it across the network, maps it into the requesting node, and updates ownership or sharing information. Thus, DSM hides explicit message passing from the programmer.

DSM may be implemented at page level, object level, or language/runtime level. In page-based DSM, the unit of sharing is usually a virtual memory page. This works well with existing memory management hardware. In object-based DSM, the unit is a shared object, which can reduce false sharing but requires more runtime support.

On a read miss, the DSM system locates a node that has the latest copy and sends a read-only copy to the requester. On a write miss, the requester must obtain exclusive access. Other copies may be invalidated or updated. This is similar to cache coherence, but the unit may be a page or object and the communication happens over a network.

DSM systems need coherence and consistency rules. Coherence ensures that reads get the correct value for a memory location. Consistency defines when writes become visible to other processors. Sequential consistency is simple but expensive. Weak consistency and release consistency improve performance by enforcing strict ordering mainly at synchronization points such as lock acquire and release.

Granularity is a major DSM design choice. If the shared unit is large, such as a page, DSM can use virtual-memory hardware and transfer fewer metadata entries, but unrelated variables may travel together and cause false sharing. If the shared unit is small, such as an object or word, false sharing is reduced, but the system needs more tracking information and may create more messages.

The advantages of DSM are important. It provides an easier programming model than message passing because programmers can use shared variables and pointer-based data structures. It can use inexpensive off-the-shelf nodes and scale memory capacity by combining local memories. It exploits locality by moving data close to the node that uses it. Programs written for shared-memory systems may need fewer changes than if they were rewritten for explicit message passing.

The limitations are also serious. Remote access is much slower than local memory access. If pages move frequently between nodes, page thrashing can occur. False sharing occurs when two processors use different variables that happen to be on the same shared page; the whole page keeps moving or invalidating even though the variables are logically independent. DSM also requires complex coherence, directory, and consistency management. Performance depends heavily on locality, granularity, and synchronization behavior.

Applications of DSM include distributed scientific computing, clusters that want a shared-memory programming model, parallel simulations, large data-processing tasks with shared structures, and research systems such as IVY. It is useful when programmers want shared-memory convenience but the machine is physically distributed.

| Aspect | DSM explanation |
|---|---|
| Physical memory | Distributed among nodes |
| Programmer view | One shared address space |
| Access mechanism | Local access or remote page/object fault |
| Coherence | Write-invalidate or write-update |
| Main strength | Easier programming than message passing |
| Main weakness | Remote latency, false sharing, thrashing |

### Draw In Exam

Draw three nodes connected by a network. Inside each node draw CPU, local memory, and mapping manager. Below that draw a shared virtual address space divided into pages. Show a page moving/replicating from one node to another.

## Sources Used For Q9

- [ParallelArchitecture_PP.ppt](<MaterialToRefer/ParallelArchitecture_PP.ppt>) - bus, crossbar, omega/multistage, mesh, torus, hypercube, topology metrics.
- [pp3.pdf](<MaterialToRefer/pp3.pdf>) - interconnection network basics, bus/crossbar/hypercube/multistage properties.
- [ParallelArchitecture.pdf](<MaterialToRefer/ParallelArchitecture.pdf>) - interconnection network role in parallel machines.
- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - interconnection network background.

---

<a id="q9"></a>

## Q9. Discuss Different Interconnection Networks Used In Multiprocessor Architectures. Compare Bus, Crossbar, Multistage, And Hypercube Networks.

**CLO:** CLO 4 - ILP and Advanced Architectures.

### Figures To Use

![Interconnection network comparison](<images/solutions/q9/q9_interconnection_networks.png>)

This is the best exam diagram. It gives small redrawable block diagrams for bus, crossbar, multistage/Omega, and hypercube.

![Network topologies from PPT](<images/solutions/q9/parallel_architecture_pp/Slide17.PNG>)

This slide lists the main topologies and where they are used.

![Crossbar switch](<images/solutions/q9/parallel_architecture_pp/Slide18.PNG>)

Use this source figure for crossbar. It shows a grid of switching elements.

![Omega network](<images/solutions/q9/parallel_architecture_pp/Slide19.PNG>)

Use this source figure for multistage networks. Omega reduces switch complexity but can be blocking.

![Mesh, torus, and hypercube](<images/solutions/q9/parallel_architecture_pp/Slide21.PNG>)

This PPT slide gives the hypercube idea and related topologies.

![Interconnection topology metrics](<images/solutions/q9/parallel_architecture_pp/Slide24.PNG>)

Use this source slide for comparison words: diameter, connectivity, and bisection width. These terms make the answer look more complete.

![Bus network properties](<images/solutions/q9/pp3_pdf/pp3_page_0012.png>)

Use this for bus advantages and limitations: simple, cost-effective for small systems, easy snooping, but poor scalability and high contention.

![Multistage logarithmic network](<images/solutions/q9/pp3_pdf/pp3_page_0021.png>)

Use this for the multistage part. It shows the `O(N log N)` cost, `O(log N)` latency idea, and blocking conflict.

### Answer To Write

An interconnection network is the communication structure that connects processors, memories, cache banks, and I/O modules in a multiprocessor system. Its design strongly affects latency, bandwidth, scalability, cost, and reliability. In shared-memory systems it connects processors to memory and helps support coherence. In distributed-memory systems it carries messages between nodes.

Important network metrics are latency, bandwidth, diameter, bisection bandwidth, blocking behavior, and cost. Latency is the time for a message to travel from source to destination. Bandwidth is the amount of data transferred per unit time. Diameter is the maximum shortest path between any two nodes. Bisection bandwidth measures communication capacity between two halves of the system. A non-blocking network can connect many source-destination pairs simultaneously without conflict; a blocking network may force some communications to wait.

For comparison, also mention connectivity. High connectivity gives alternate paths and better fault tolerance, but it increases wiring and router cost. This is why no single network is best for every multiprocessor size.

A bus is the simplest interconnection network. All processors and memory modules share the same communication medium. It is easy to implement and naturally supports broadcast, so snooping cache coherence is simple. Its main disadvantage is limited scalability. Only one transaction can use the bus at a time, so contention grows quickly as processors increase. Electrical loading also limits bus frequency.

A crossbar network connects every input to every output through a grid of switches. It can support multiple simultaneous non-conflicting transfers and has low latency. It is effectively non-blocking for many processor-memory connections. The disadvantage is cost: for `N` processors and `N` memory modules, the number of crosspoints is about `N^2`. Arbitration and wiring also become difficult for large systems.

A multistage interconnection network uses several stages of small switches, often 2x2 switches, between inputs and outputs. Omega, Butterfly, Banyan, and Benes networks are common examples. The cost is usually `O(N log N)`, much lower than a crossbar, and latency is about `O(log N)` stages. The limitation is that many multistage networks are blocking: two communication requests may need the same internal link or switch, even if their final destinations are different.

A hypercube network connects `2^d` nodes as vertices of a `d`-dimensional cube. Each node connects to `d = log2(N)` neighbors, and the diameter is also `log2(N)`. This gives good connectivity and relatively low latency for a distributed-memory machine. The disadvantages are layout complexity and increasing node degree as the system grows.

| Network | Cost | Latency | Scalability | Main advantage | Main limitation |
|---|---|---|---|---|---|
| Bus | Low | Low for small systems | Poor | Simple, cheap, broadcast | High contention |
| Crossbar | `O(N^2)` | Low | Limited by cost | High bandwidth, non-blocking | Expensive wiring/switches |
| Multistage/Omega | `O(N log N)` | `O(log N)` | Good | Lower cost than crossbar | Blocking/contention |
| Hypercube | `O(N log N)` links | `O(log N)` | Good for message passing | Low diameter, many paths | Hard physical layout |

Thus, buses are suitable for small SMPs, crossbars are good when high bandwidth is needed for a moderate number of nodes, multistage networks offer a cost-performance compromise, and hypercubes are useful in distributed-memory systems where logarithmic diameter and structured routing are important.

### Draw In Exam

Draw four small diagrams: one shared bus line, one crossbar grid, one three-stage 2x2 Omega network, and one cube/hypercube. Then write the comparison table with cost, scalability, and blocking behavior.

## Sources Used For Q10

- [VLSI ARC.pptx](<MaterialToRefer/VLSI ARC.pptx>) - single-cycle vs multicycle, multicycle steps, RISC datapath, pipeline introduction.
- [Pipelining.pdf](<MaterialToRefer/Pipelining.pdf>) - 5-stage pipeline, speedup, hazards, forwarding, stalls.
- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - RISC-V datapath and pipelining background.

---

<a id="q10"></a>

## Q10. Explain The Design Of A Multicycle RISC Processor And Discuss How Pipelining Improves Processor Performance. Also Explain Different Hazards And Their Handling Techniques.

**CLO:** CLO 1 - Processor Architecture and Organization; CLO 2 - Pipelining and RISC Performance.

### Figures To Use

![Multicycle RISC processor and pipeline](<images/solutions/q10/q10_multicycle_risc_pipeline.png>)

This is the best exam diagram. It shows the multicycle datapath blocks and a 5-stage pipeline timing diagram.

![Single-cycle versus multicycle](<images/solutions/q10/vlsi_arc_ppt_export/Slide15.PNG>)

This source slide explains why multicycle is more efficient than single-cycle: it divides execution into stages.

![Multicycle implementation steps](<images/solutions/q10/vlsi_arc_ppt_export/Slide18.PNG>)

Use this slide to remember the five steps: instruction fetch, decode/register fetch, execute/address calculation, memory access, and write back.

![Pipeline introduction](<images/solutions/q10/vlsi_arc_ppt_export/Slide20.PNG>)

This slide connects multicycle design with pipelining and hazards.

![5-stage pipeline and hazards note](<images/solutions/q10/pipelining_pdf/pipelining_page_0001.png>)

Use this source note for the expected 5-stage pipeline names and hazard terms: `IF`, `ID`, `EX`, `MEM`, and `WB`.

### Answer To Write

A multicycle RISC processor executes each instruction in a sequence of smaller steps instead of completing the whole instruction in one long clock cycle. The datapath contains the program counter (PC), instruction memory, instruction register (IR), register file, ALU, data memory, memory data register (MDR), ALU output register, multiplexers, and a control unit. The control unit generates signals in each cycle to select operands, read/write registers, access memory, update the PC, and choose ALU operations.

The usual multicycle stages are instruction fetch, instruction decode/register fetch, execution or address calculation, memory access, and write back. During instruction fetch, the processor reads the instruction from memory and increments the PC. During decode, it reads source registers and identifies the instruction type. During execution, the ALU performs an arithmetic operation, calculates an effective address, or compares operands for a branch. During memory access, load/store instructions access data memory. During write back, results are written into the register file.

The advantage of multicycle design is that different instructions can take different numbers of cycles. For example, an arithmetic instruction may not need the memory access stage, while a load instruction needs memory and write back. Hardware can be reused across cycles; the same ALU can increment PC, calculate addresses, and perform arithmetic. The clock cycle is shorter than in a single-cycle design because each cycle performs only one small step.

Pipelining improves performance by overlapping these stages for different instructions. In a 5-stage RISC pipeline, the stages are usually `IF`, `ID`, `EX`, `MEM`, and `WB`. While one instruction is in execute, another can be in decode, another in fetch, and so on. Pipelining does not greatly reduce the latency of one instruction, but it increases throughput. Ideally, after the pipeline fills, one instruction completes every clock cycle, so ideal CPI approaches 1.

For `N` instructions and `k` pipeline stages, non-pipelined time is roughly `N x k x T`, while pipelined time is roughly `(k + N - 1) x T`, assuming balanced stages and no stalls. Therefore, ideal speedup approaches `k` for large `N`, but real speedup is lower because of hazards and pipeline overhead.

The first hazard type is structural hazard. It occurs when two stages need the same hardware resource in the same cycle, such as one memory used for both instruction fetch and data access. It is handled by duplicating resources, using separate instruction/data caches, multiporting, or stalling.

The second type is data hazard. It occurs when an instruction depends on the result of an earlier instruction. The most common is RAW, or read-after-write. It is handled by forwarding/bypassing, stalling, compiler scheduling, or pipeline interlocking. A load-use hazard may still need one stall because loaded data is available only after the memory stage.

The third type is control hazard. It occurs due to branches and jumps because the next PC is not known immediately. It is handled by branch prediction, delayed branching, early branch resolution, flushing wrong-path instructions, and speculation.

Thus, a multicycle RISC processor improves clock efficiency by dividing instructions into smaller control steps, while pipelining improves throughput by overlapping those steps across many instructions.

The key contrast is this: multicycle execution reuses datapath hardware over time for one instruction, while pipelining keeps several instructions in different stages at the same time. So multicycle mainly reduces clock waste and hardware duplication; pipelining mainly improves instruction throughput.

### Draw In Exam

Draw the multicycle datapath blocks: PC, instruction memory, IR, register file, ALU, data memory, MDR, and control unit. Then draw a 5-stage timing diagram for five instructions and mark hazards as bubbles/stalls.

## Sources Used For Q11

- [Memory.pdf](<MaterialToRefer/Memory.pdf>) - memory hierarchy and locality.
- [Virtual memory.ppt](<MaterialToRefer/Virtual memory.ppt>) - TLB, virtual memory, demand paging, replacement policies.
- [pp2.ppt](<MaterialToRefer/pp2.ppt>) - cache coherence protocols in shared-memory systems.
- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - memory hierarchy, cache performance, virtual memory, and coherence.

---

<a id="q11"></a>

## Q11. Explain The Role Of Memory Hierarchy In Improving System Performance. Discuss Cache Coherence, Virtual Memory, And Replacement Policies.

**CLO:** CLO 3 - Memory Organization and Management; CLO 4 - ILP and Advanced Architectures.

### Figures To Use

![Memory hierarchy summary](<images/solutions/q11/q11_memory_hierarchy_summary.png>)

This is the best exam diagram. It combines the hierarchy with the three subtopics asked in the question: coherence, virtual memory, and replacement.

![Memory hierarchy source figure](<images/solutions/q11/memory_pdf/memory_page_0001.png>)

Use this source figure for the basic hierarchy: registers, cache, main memory, disk, and tape.

![TLB from virtual memory PPT](<images/solutions/q11/virtual_memory_ppt_export/Slide27.PNG>)

This is useful for virtual memory performance. It shows that a TLB avoids doing a full page-table lookup for every memory reference.

![Replacement algorithms from PPT](<images/solutions/q11/virtual_memory_ppt_export/Slide44.PNG>)

Use this for replacement policy names: Optimal, LRU, FIFO, and Clock.

### Answer To Write

Memory hierarchy improves system performance by arranging storage into levels of different speed, size, and cost. The CPU needs data very quickly, but the fastest memories are expensive and small. Therefore, systems use small fast memories near the processor and large slow memories farther away. The common hierarchy is registers, L1/L2/L3 cache, main memory, secondary storage, and archival storage.

The hierarchy works because programs show locality of reference. Temporal locality means recently used data is likely to be used again. Spatial locality means nearby addresses are likely to be used soon. Caches exploit locality by storing recently used blocks near the CPU. Virtual memory exploits locality by keeping active pages in main memory and inactive pages on disk.

The performance of a cache hierarchy is often described using AMAT:

```text
Average Memory Access Time = Hit time + Miss rate x Miss penalty
```

A hit means the requested block is found in the upper level. A miss means the block must be fetched from a lower level, causing delay. Good hierarchy design reduces miss rate, hit time, and miss penalty.

Cache coherence is required in shared-memory multiprocessors where each processor has a private cache. The same memory block may exist in several caches. If one processor writes a new value, other processors may still hold an old copy. A coherence protocol ensures that all processors observe correct values. Common methods are write-invalidate and write-update. In write-invalidate, the writing processor invalidates other cached copies before writing. In write-update, the new value is broadcast to other copies. Snooping protocols are common in small bus-based SMP systems, while directory-based protocols scale better for larger systems.

Virtual memory gives each process a large private logical address space. The memory management unit translates virtual addresses into physical addresses using page tables. Demand paging loads pages only when needed. If a referenced page is not in main memory, a page fault occurs and the operating system brings the page from disk. The TLB stores recently used page table entries so that address translation is fast.

Replacement policies decide which cache block or memory page should be removed when a new block/page must be brought in and there is no free space. FIFO removes the oldest entry. LRU removes the least recently used entry and usually matches locality well. Random replacement is simple and sometimes effective. Clock is an efficient approximation to LRU used in virtual memory systems. Optimal replacement would remove the item used farthest in the future, but it cannot be implemented because the future is unknown.

| Topic | Role in performance |
|---|---|
| Cache hierarchy | Reduces average access time by keeping active blocks near CPU |
| Cache coherence | Maintains correctness of cached shared data |
| Virtual memory | Gives large protected address spaces and supports demand paging |
| TLB | Speeds up virtual-to-physical translation |
| Replacement policy | Reduces miss/page fault cost by choosing good victims |

Thus, memory hierarchy improves performance by giving the illusion of a large, fast, low-cost memory. Its effectiveness depends on locality, cache organization, coherence correctness, virtual memory translation, and good replacement decisions.

### Draw In Exam

Draw the memory hierarchy pyramid and write AMAT beside it. Then add three side notes: coherence keeps shared cached copies correct, virtual memory maps virtual pages to frames, and replacement chooses the victim block/page on a miss.

## Sources Used For Q12

- [Pipelining.pdf](<MaterialToRefer/Pipelining.pdf>) - pipeline depth tradeoff and superpipelining note.
- [VLSI ARC.pptx](<MaterialToRefer/VLSI ARC.pptx>) - pipeline introduction and hazards.
- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - pipelining limits, branch penalties, and deeper pipeline tradeoffs.

---

<a id="q12"></a>

## Q12. Discuss The Impact Of Pipeline Depth On Processor Performance. Explain The Associated Challenges And Optimization Methods.

**CLO:** CLO 2 - Pipelining and RISC Performance.

### Figures To Use

![Pipeline depth tradeoff](<images/solutions/q12/q12_pipeline_depth_tradeoff.png>)

This is the best exam diagram. It shows that performance improves with depth at first, then falls when branch penalties, latch overhead, and stalls dominate.

![Pipeline depth note](<images/solutions/q12/pipelining_pdf/pipelining_page_0002.png>)

This local note gives the expected point: deeper pipelines can improve speed but increase misprediction penalties.

### Answer To Write

Pipeline depth means the number of stages into which instruction execution is divided. A shallow pipeline has fewer stages, and each stage performs more work. A deep pipeline splits the work into many smaller stages. The goal of increasing pipeline depth is to reduce the clock cycle time because each stage has less logic delay.

Ideally, if an instruction path is split into `k` balanced stages, the clock period becomes smaller and the processor frequency increases. After the pipeline is full, one instruction may complete every cycle, so throughput improves. This is the main idea behind superpipelining.

For a simple numerical explanation, assume 100 instructions and a 5-stage pipeline with 10 ns per stage. Without pipelining, time is about `100 x 5 x 10 = 5000 ns`. With pipelining, time is about `(100 + 5 - 1) x 10 = 1040 ns`, so ideal speedup is about `4.8`. This proves why pipelining improves throughput, but actual speedup is lower when stalls and branch flushes occur.

However, performance does not keep improving forever as depth increases. Each pipeline stage needs pipeline registers/latches, clocking, setup time, and control overhead. When stages become too small, latch overhead becomes a significant part of the cycle time. Also, it becomes harder to balance stages exactly, so the slowest stage still determines the clock period.

Deeper pipelines increase branch penalty. If a branch is resolved many stages after fetch, then many younger instructions may already be in the pipeline. If the prediction is wrong, those instructions must be flushed. Therefore, the cost of one branch misprediction increases with pipeline depth. This makes accurate branch prediction more important.

Deeper pipelines also increase data hazard complexity. A result may take more stages before it is available to a dependent instruction. More forwarding paths may be needed, and some dependences may require extra stalls. Pipeline control logic becomes more complex because it must handle more pipeline registers, exceptions, interrupts, flushes, and stalls.

Other challenges include higher power consumption due to more pipeline registers and clock distribution, more verification complexity, and greater sensitivity to cache misses. A deep pipeline may have high peak frequency but poor real performance if branch mispredictions and stalls occur often.

Optimization methods include balanced stage partitioning, forwarding/bypassing, pipeline interlocking, compiler instruction scheduling, early branch resolution, branch prediction, branch target buffers, speculation, out-of-order execution, and good cache design. The best pipeline depth is a balance between clock frequency and the penalties caused by hazards.

| Pipeline depth effect | Benefit | Cost |
|---|---|---|
| More stages | Higher clock frequency | More latch overhead |
| More in-flight instructions | Higher throughput | More complex control |
| Later branch resolution | More speculation possible | Higher misprediction penalty |
| Longer producer-consumer distance | More overlap | More forwarding/stalls |

Thus, increasing pipeline depth improves performance only up to an optimum point. Beyond that point, stalls, branch penalties, power, and control overhead can reduce actual performance.

### Draw In Exam

Draw a graph with pipeline depth on the x-axis and performance on the y-axis. Show performance rising first, reaching a best region, then falling. On the side draw a deep pipeline and mark flushed stages after a branch misprediction.

## Sources Used For Q13

- [VLSI ARC.pptx](<MaterialToRefer/VLSI ARC.pptx>) - performance metrics, quantitative design, RISC, datapath, pipeline, memory hierarchy.
- [Pipelining.pdf](<MaterialToRefer/Pipelining.pdf>) - pipelining and hazards.
- [Memory.pdf](<MaterialToRefer/Memory.pdf>) - memory hierarchy.
- [pp1.ppt](<MaterialToRefer/pp1.ppt>) - ILP techniques.
- [pp2.ppt](<MaterialToRefer/pp2.ppt>) - multiprocessing, shared memory, coherence.
- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - quantitative processor design.

---

<a id="q13"></a>

## Q13. Design Considerations For High-Performance Processors Include Pipelining, Cache Hierarchy, ILP, And Multiprocessing. Discuss These Aspects In Detail With Suitable Examples.

**CLO:** CLO 1, CLO 2, CLO 3, and CLO 4.

### Figures To Use

![High-performance processor design map](<images/solutions/q13/q13_high_performance_design_map.png>)

This is the best exam diagram. It places pipelining, cache hierarchy, ILP, and multiprocessing around the CPU performance equation.

![Performance metrics slide](<images/solutions/q13/vlsi_arc_ppt_export/Slide4.PNG>)

This source slide gives the CPU performance equation and metrics such as execution time, throughput, latency, and clock frequency.

![Quantitative design slide](<images/solutions/q13/vlsi_arc_ppt_export/Slide5.PNG>)

Use this to justify why high-performance processor design must be quantitative, not only descriptive.

### Answer To Write

High-performance processor design is based on reducing execution time. The basic equation is:

```text
CPU time = Instruction Count x CPI x Clock Cycle Time
```

Therefore, a processor can be improved by reducing instruction count, reducing CPI, or reducing clock cycle time. Pipelining, cache hierarchy, instruction-level parallelism, and multiprocessing attack different parts of this equation.

Pipelining divides instruction execution into stages such as fetch, decode, execute, memory, and write back. It improves throughput because multiple instructions are overlapped. After the pipeline fills, ideally one instruction completes every clock cycle. This reduces effective CPI. The design challenge is handling structural, data, and control hazards. Forwarding, stalls, branch prediction, and compiler scheduling are used to reduce pipeline losses.

Cache hierarchy is required because processor speed is much higher than main memory speed. L1 cache gives very fast access, L2 and L3 provide larger capacity, and DRAM provides main memory. The memory hierarchy reduces average memory access time by exploiting locality. Important design choices include cache size, block size, associativity, write policy, replacement policy, prefetching, TLB design, and coherence. A fast processor without a good memory system will stall frequently.

Instruction-Level Parallelism (ILP) improves performance by executing or overlapping independent instructions from the same program. Techniques include superscalar issue, out-of-order execution, branch prediction, speculation, register renaming, dynamic scheduling, Tomasulo-style reservation stations, VLIW, loop unrolling, and compiler scheduling. ILP reduces CPI below what simple pipelining can achieve. Its limits are true dependences, branch mispredictions, memory dependences, finite issue width, power, and hardware complexity.

Multiprocessing improves performance by using multiple cores or processors. It exploits thread-level parallelism rather than only instruction-level parallelism. Examples include multicore CPUs, SMP systems, NUMA systems, clusters, and GPUs. Multiprocessing improves throughput and can reduce execution time for parallel programs. However, it introduces challenges such as synchronization, cache coherence, load balancing, interconnection latency, shared data contention, and Amdahl's law.

The key design issue is balance. A very deep pipeline is not useful if branch mispredictions are frequent. A wide superscalar processor is not useful if the instruction stream has little ILP. Many cores are not useful if the program is mostly serial. A large cache is not useful if hit time becomes too slow. High-performance design must balance core speed, memory behavior, power, area, and workload characteristics.

| Aspect | Improves | Main techniques | Main challenge |
|---|---|---|---|
| Pipelining | Throughput/CPI | 5-stage/deeper pipelines, forwarding | Hazards and branch penalty |
| Cache hierarchy | Memory access time | L1/L2/L3, TLB, prefetching | Misses, coherence, replacement |
| ILP | CPI | Superscalar, OOO, prediction, renaming | Dependences and complexity |
| Multiprocessing | Throughput/parallel speedup | Multicore, SMP, NUMA, clusters | Synchronization and scalability |

Thus, high-performance processors are designed by combining fast pipelines, efficient caches, ILP mechanisms, and multiprocessing. The best design is workload-dependent and must be evaluated using execution time, CPI, clock rate, power, and cost.

### Draw In Exam

Draw the CPU performance equation in the center. Around it draw four blocks: pipelining, cache hierarchy, ILP, and multiprocessing. For each block, write one benefit and one challenge.

## Sources Used For Q14

- [VLSI ARC.pptx](<MaterialToRefer/VLSI ARC.pptx>) - processor organization, datapath, control unit, ALU, registers, buses, memory hierarchy.
- [Memory.pdf](<MaterialToRefer/Memory.pdf>) - memory hierarchy.
- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - processor organization and memory-system context.

---

<a id="q14"></a>

## Q14. Explain Processor Organization And Architectural Overview Of Modern Computer Systems. Discuss The Role Of ALU, Control Unit, Registers, Buses, And Memory Hierarchy.

**CLO:** CLO 1 - Processor Architecture and Organization; CLO 3 - Memory Organization and Management.

### Figures To Use

![Processor organization overview](<images/solutions/q14/q14_processor_organization.png>)

This is the best exam diagram. It shows the CPU components and their connection to cache, main memory, I/O, and system interconnect.

![Basic computer architecture slide](<images/solutions/q14/vlsi_arc_ppt_export/Slide2.PNG>)

This source slide lists the key components: ALU, control unit, registers, cache, and main memory.

![Processor organization slide](<images/solutions/q14/vlsi_arc_ppt_export/Slide12.PNG>)

Use this slide for the roles of datapath, control unit, registers, and buses.

![Datapath components slide](<images/solutions/q14/vlsi_arc_ppt_export/Slide13.PNG>)

This slide helps you explain PC, register file, ALU, and multiplexers.

![Control unit design slide](<images/solutions/q14/vlsi_arc_ppt_export/Slide14.PNG>)

Use this for the control-unit paragraph. It gives the direct comparison: hardwired control is faster, microprogrammed control is more flexible.

### Answer To Write

Processor organization describes how the internal hardware units of a CPU are connected and controlled to execute instructions. Modern computer architecture includes the CPU core, registers, ALU, control unit, caches, main memory, I/O system, buses/interconnects, and storage. The processor executes instructions by fetching them from memory, decoding them, reading operands, performing operations, accessing memory if required, and writing results.

The ALU, or Arithmetic Logic Unit, performs arithmetic and logical operations. Arithmetic operations include addition, subtraction, increment, decrement, and sometimes multiplication/division through separate units. Logical operations include AND, OR, XOR, NOT, shifts, and comparisons. The ALU is the main execution block for integer instructions and address calculation.

The control unit coordinates the operation of the processor. It decodes the instruction opcode and generates control signals such as register write, memory read, memory write, ALU operation, ALU source selection, branch control, and PC update. In a hardwired control unit, signals are generated by combinational/sequential logic. In a microprogrammed control unit, signals are generated from control memory. Hardwired control is faster, while microprogrammed control is more flexible.

Registers are small, very fast storage locations inside the CPU. General-purpose registers hold operands and intermediate results. Special registers include the program counter (PC), instruction register (IR), status/flag register, stack pointer, and memory address/data registers. Registers reduce memory traffic because the ALU can operate directly on register operands.

Buses and interconnects move information between components. A data bus carries data values. An address bus carries memory or I/O addresses. A control bus carries read/write, interrupt, clock, and control signals. Inside modern CPUs, buses may be replaced by more complex interconnects, crossbars, rings, or networks-on-chip.

Memory hierarchy is essential because CPU registers and caches are much faster than main memory. The hierarchy normally includes registers, L1/L2/L3 caches, main memory, secondary storage, and sometimes tertiary storage. Caches hold recently or frequently used blocks to reduce average memory access time. Main memory stores running programs and data. Secondary storage stores persistent data.

In a modern system, one CPU may contain multiple cores. Each core may have private L1 caches and shared L2/L3 caches. The memory management unit translates virtual addresses to physical addresses. The memory controller connects to DRAM. I/O controllers connect devices such as disks, network cards, and displays. All these parts are connected through a system interconnect.

The instruction execution cycle can be summarized as:

```text
Fetch instruction -> Decode -> Read registers -> Execute in ALU
-> Access memory if needed -> Write result -> Update PC
```

Thus, the ALU performs computation, the control unit directs operations, registers provide fast operands, buses/interconnects move information, and memory hierarchy supplies data/instructions efficiently. Together these components determine performance, cost, and power.

### Draw In Exam

Draw a CPU box containing control unit, register file, ALU, PC/IR, and internal buses. Connect the CPU to cache, main memory, I/O/storage, and system bus. Then explain each component in one paragraph.

## Sources Used For Q15

- [Pipelining.pdf](<MaterialToRefer/Pipelining.pdf>) - 5-stage pipeline, hazards, forwarding, stalling, interlocking.
- [VLSI ARC.pptx](<MaterialToRefer/VLSI ARC.pptx>) - pipeline hazards and forwarding/stall mechanisms.
- [pp1.ppt](<MaterialToRefer/pp1.ppt>) - data dependences, RAW/WAR/WAW, hazards.
- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - pipeline hazards and forwarding.

---

<a id="q15"></a>

## Q15. Explain Data Hazards In Pipelined Processors With Suitable Timing Diagrams. Discuss Forwarding, Stalling, And Pipeline Interlocking Techniques.

**CLO:** CLO 2 - Pipelining and RISC Performance.

### Figures To Use

![Data hazard timing diagrams](<images/solutions/q15/q15_data_hazard_timing.png>)

This is the best exam figure. It shows RAW without forwarding, RAW with forwarding, and load-use hazard with one stall.

![Pipelining source note](<images/solutions/q15/pipelining_pdf/pipelining_page_0001.png>)

This source page covers the 5-stage pipeline and mentions hazards, forwarding, stalls, and interlocking.

![Book forwarding example](<images/solutions/q15/book_figures/book_forwarding_paths_page_0789.png>)

Use this book figure if drawing forwarding arrows. It shows that ALU results can be sent from pipeline registers directly to the ALU input of a later instruction.

![Book load-use hazard example](<images/solutions/q15/book_figures/book_load_use_hazard_page_0790.png>)

Use this figure for the important exception: a load-use dependence still needs a stall because the load data is available only after memory access.

![Book pipeline interlock explanation](<images/solutions/q15/book_figures/book_pipeline_interlock_page_0791.png>)

Use this only for deep explanation. It connects the inserted bubble with hardware interlocking.

### Answer To Write

A data hazard occurs in a pipeline when an instruction depends on the result of another instruction that has not yet completed. Because pipelining overlaps instructions, a later instruction may reach its execute stage before an earlier instruction has written its result. If not handled, the later instruction may use an old or incorrect value.

The three common data dependences are RAW, WAR, and WAW. RAW means Read After Write. It is a true dependence: instruction `I2` needs a value produced by `I1`. WAR means Write After Read. It is an anti-dependence caused by name reuse. WAW means Write After Write. It is an output dependence where two instructions write the same destination. In a simple 5-stage in-order RISC pipeline, RAW is the main data hazard. WAR and WAW usually appear in pipelines that allow out-of-order execution or writes in different stages.

| Dependence | Meaning | Why it matters |
|---|---|---|
| RAW | Later instruction reads a value before earlier instruction writes it | True dependence; most important in 5-stage pipelines |
| WAR | Later instruction writes a location before earlier instruction reads it | Name dependence; mainly in out-of-order execution |
| WAW | Later instruction writes before earlier instruction writes | Name/output dependence; mainly in out-of-order execution |

Example of RAW hazard:

```text
I1: ADD R1, R2, R3
I2: SUB R4, R1, R5
```

`I2` needs `R1`, but `R1` is produced by `I1`. In a 5-stage pipeline, `I1` produces the ALU result at the end of EX and writes it in WB. Without any hazard handling, `I2` may read old `R1` during ID.

Forwarding, also called bypassing, solves many RAW hazards by sending a result directly from a later pipeline stage to an earlier consumer stage, without waiting for write back. For an ALU-to-ALU dependence, the result from the EX/MEM or MEM/WB pipeline register can be forwarded to the ALU input of the next instruction. This avoids a stall.

However, forwarding cannot remove every stall. The classic case is a load-use hazard:

```text
I1: LW  R1, 0(R2)
I2: ADD R3, R1, R4
```

For a load, the data is available only after the MEM stage. The dependent `ADD` needs the value at the beginning of its EX stage. Therefore, even with forwarding, one bubble is usually inserted so that the loaded value can be forwarded after memory access.

Stalling means delaying one or more pipeline stages until the hazard is removed. A stall inserts a bubble, or no-operation, into the pipeline. During a stall, earlier stages such as IF and ID may be frozen, while later stages continue. Stalling is simple and correct, but it reduces performance because no useful instruction completes from the bubble.

Pipeline interlocking is the hardware mechanism that detects hazards and automatically inserts stalls. The hazard detection unit compares source registers of the instruction in ID with destination registers of instructions ahead in the pipeline. If forwarding can solve the problem, the forwarding unit selects the correct bypass input. If forwarding cannot solve it, the interlock freezes PC and IF/ID registers and inserts a bubble into EX.

Compiler scheduling can also help by placing independent instructions between a producer and consumer. For example, after a load instruction, the compiler may schedule an unrelated arithmetic instruction before the dependent instruction. This reduces stalls without changing program meaning.

| Technique | Meaning | Use |
|---|---|---|
| Forwarding | Send result directly to dependent stage | Avoid ALU-ALU RAW stalls |
| Stalling | Insert bubble until data is ready | Correctness when data unavailable |
| Interlocking | Hardware detects hazard and stalls automatically | Automatic pipeline safety |
| Scheduling | Compiler reorders independent instructions | Reduces number of stalls |

Thus, data hazards are caused by operand dependences between overlapping instructions. Forwarding gives performance, stalling gives correctness, and interlocking automates hazard detection and stall insertion.

### Draw In Exam

Draw three timing diagrams: RAW without forwarding with two stalls, RAW with forwarding and no stall, and load-use with one stall. Mark stages as `IF ID EX MEM WB`, show the forwarding arrow, and label the bubble inserted by interlocking.
