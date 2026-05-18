# Expected Questions - Solutions

Workspace: `C:/Users/kapil/OneDrive/Desktop/Architecture`

This file will be built question by question from [expected.md](<expected.md>).

## Sources Used For Q1

- [Computer Architecture, Sixth Edition: A Quantitative Approach](<Computer Architecture, Sixth Edition_ A Quantitative Approach ( PDFDrive ).pdf>) - Chapter 2: Memory Hierarchy Design; Appendix B: Review of Memory Hierarchy; Chapter 5: cache coherence background.
- [Memory.pdf](<MaterialToRefer/Memory.pdf>) - memory hierarchy, SRAM/DRAM, cache, locality, inclusion, coherence, hit/miss, mapping, replacement, virtual memory.
- [VLSI ARC.pptx](<MaterialToRefer/VLSI ARC.pptx>) - basic architecture components: registers, cache, main memory, processor organization.
- Class board image shared in chat - memory hierarchy pyramid from registers down to cache, RAM, flash/SSD, hard disk, and tapes.
- Screenshot exported for exam diagram: [Memory.pdf page 1 memory hierarchy](<images/solutions/q1/memory_pdf_page_01_memory_hierarchy.png>).
- Class board screenshot for exam diagram: [class board memory hierarchy](<images/solutions/q1/class_board_memory_hierarchy.png>).

---

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

## Q2. Discuss Memory Management Techniques Used In Virtual Memory Systems. Explain Paging And Segmentation With Examples.

**CLO:** CLO 3 - Memory Organization and Management.

### Figures To Use

![Address translation in a paging system](<images/solutions/q2/virtual_memory_ppt_export/Slide24.PNG>)

This is the main diagram to draw. It shows that a virtual address is split into `page number + offset`; the page number goes to the page table, the page table gives the frame number, and `frame number + same offset` becomes the physical address. This diagram directly supports paging and address translation.

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

## Q3. Explain Coherence Problems In Shared Memory Systems. Discuss Cache Coherence Protocols In Multiprocessor Systems.

**CLO:** CLO 3 - Memory Organization and Management; CLO 4 - Advanced Architectures.

### Figures To Use

![Example cache coherence problem](<images/solutions/q3/pp2_ppt_export/Slide24.PNG>)

This is the main problem diagram. It shows processors with private caches connected to shared memory. One processor writes a new value, but other caches may still contain the old value. Draw this first because it explains why coherence is needed.

![Snoopy cache-coherence protocols](<images/solutions/q3/pp2_ppt_export/Slide30.PNG>)

This is the main protocol diagram. It shows that every cache controller watches, or snoops, the shared bus. If a bus transaction affects a block present in a cache, that cache invalidates, updates, or supplies the value.

![Two classes of cache coherence protocols](<images/solutions/q3/pp2_ppt_export/Slide29.PNG>)

This slide is useful for structure. It reminds you that the two main protocol families are snooping and directory-based protocols.

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

| Protocol | Idea | Best use |
|---|---|---|
| Snooping | Every cache watches the bus | Small UMA/SMP systems |
| Directory-based | Directory stores sharer/owner information | Large NUMA/DSM systems |
| Write-invalidate | Writer invalidates other copies | Most common, lower bandwidth |
| Write-update | Writer broadcasts new value | Useful when many processors read after every write |

Thus, coherence problems occur because private caches may store stale copies of shared data. Cache coherence protocols solve this by ensuring that writes are propagated and stale copies are invalidated or updated. Snooping is simpler, while directory-based coherence is more scalable.

### Draw In Exam

Draw the cache coherence problem diagram first: three processors with private caches and one shared memory. Show old value `X = 5` in some caches and new value `X = 7` after one processor writes. Then draw a bus-based snooping diagram and write: "cache controllers snoop bus and invalidate/update copies."
