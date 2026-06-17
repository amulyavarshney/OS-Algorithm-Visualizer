# Page Replacement Algorithms

Interactive visualizations of virtual memory page replacement algorithms used when physical memory is full and a new page must be loaded.

## Overview

Page replacement is a critical component of virtual memory systems. When physical memory is full and a page fault occurs, the OS must decide which page to evict to make room for the new page. Different algorithms have different characteristics affecting system performance.

## Memory Management Basics

### Virtual Memory

**Concept**:
- Programs use virtual addresses
- OS maps virtual to physical memory
- More virtual memory than physical possible
- Disk extends effective memory

**Advantages**:
✅ Programs can use more memory than available
✅ Multiple processes run simultaneously
✅ Simple memory layout for programs
✅ Memory protection

**Disadvantage**:
❌ Page misses (faults) slow access significantly

### Page and Frame

**Page**: Unit of virtual memory (typically 4KB)
**Frame**: Unit of physical memory (same size)
**Page Fault**: Reference to page not in physical memory

## Page Replacement Algorithms

### 1. First In First Out (FIFO)

**Concept**:
```
Replace the page that has been in memory the LONGEST
```

**Algorithm**:
```
1. Maintain queue of pages in memory order
2. When page fault occurs:
   - Remove page at front of queue
   - Add new page to rear
3. Continue
```

**Characteristics**:
- Simplest algorithm to implement
- Uses FIFO queue data structure
- Fair to all pages
- No information about page usage

**Advantages**:
✅ Simple and fast
✅ Minimal overhead
✅ Fair to all pages
✅ Easy to implement

**Disadvantages**:
❌ Not aware of page usage patterns
❌ Removes frequently used pages
❌ Belady's anomaly: More frames can mean more faults
❌ Poor performance typically

**Time Complexity**: O(1)
**Space Complexity**: O(n)

**Example**:
```
Physical Memory: 3 frames
Page Reference: 1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5

Execution:
Page  Queue Before   Queue After   Fault?
1     []            [1]           Yes
2     [1]           [1,2]         Yes
3     [1,2]         [1,2,3]       Yes
4     [1,2,3]       [2,3,4]       Yes (remove 1)
1     [2,3,4]       [3,4,1]       Yes (remove 2)
2     [3,4,1]       [4,1,2]       Yes (remove 3)
5     [4,1,2]       [1,2,5]       Yes (remove 4)
1     [1,2,5]       [1,2,5]       No
2     [1,2,5]       [1,2,5]       No
3     [1,2,5]       [2,5,3]       Yes (remove 1)
4     [2,5,3]       [5,3,4]       Yes (remove 2)
5     [5,3,4]       [5,3,4]       No

Total Faults: 9/12
```

### 2. Least Recently Used (LRU)

**Concept**:
```
Replace the page that has NOT BEEN USED for the LONGEST time
```

**Algorithm**:
```
1. Maintain recency information for each page
2. On page access: Update its recency timestamp
3. On page fault:
   - Identify page with oldest timestamp
   - Remove it
   - Load new page with current timestamp
```

**Implementation**:
- Use timestamp with each page
- Use queue/linked list (reorder on access)
- Use reference bits with aging

**Characteristics**:
- Based on temporal locality (recent = likely needed soon)
- More complex than FIFO
- Better performance typically
- Requires tracking page usage

**Advantages**:
✅ Respects temporal locality
✅ Better average performance
✅ No Belady's anomaly
✅ Matches optimal theory better

**Disadvantages**:
❌ More complex implementation
❌ Tracking overhead (timestamp/bits)
❌ Updates on every memory access
❌ Hardware support needed for speed

**Time Complexity**: O(1) to O(n) depending on implementation
**Space Complexity**: O(n)

**Example**:
```
Physical Memory: 3 frames
Page Reference: 1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5

Execution (tracking timestamps):
Page  Memory         Timestamps       Action
1     [1]           [1:0]            Fault
2     [1,2]         [1:0,2:1]        Fault
3     [1,2,3]       [1:0,2:1,3:2]    Fault
4     [1,2,4]       [1:0,2:1,4:3]    Fault (remove 3, least recent)
1     [1,2,4]       [1:3,2:1,4:3]    No fault (refresh 1)
2     [1,2,4]       [1:3,2:4,4:3]    No fault (refresh 2)
5     [1,2,5]       [1:3,2:4,5:5]    Fault (remove 4, least recent)
1     [1,2,5]       [1:6,2:4,5:5]    No fault (refresh 1)
2     [1,2,5]       [1:6,2:7,5:5]    No fault (refresh 2)
3     [3,2,1]       [3:8,2:7,1:6]    Fault (remove 5)
4     [3,2,4]       [3:8,2:7,4:9]    Fault (remove 1)
5     [3,5,4]       [3:8,5:10,4:9]   Fault (remove 2)

Total Faults: 8/12 (better than FIFO)
```

### 3. Least Frequently Used (LFU)

**Concept**:
```
Replace the page with the LOWEST frequency of use
```

**Algorithm**:
```
1. Maintain frequency counter for each page
2. On page access: Increment its frequency
3. On page fault:
   - Identify page with minimum frequency
   - Remove it
   - Load new page with frequency = 1
```

**Characteristics**:
- Based on frequency of access
- Tracks how often pages used
- Slightly different from LRU
- Works well for cyclic patterns

**Advantages**:
✅ Recognizes frequently used pages
✅ Good for repetitive patterns
✅ Fewer adjustments than LRU
✅ Less sensitive to recent changes

**Disadvantages**:
❌ Requires frequency counting
❌ New pages start with frequency 1
❌ Aging needed to forget old frequencies
❌ May retain rarely used old pages

**Time Complexity**: O(n)
**Space Complexity**: O(n)

### 4. Most Recently Used (MRU)

**Concept**:
```
Replace the page that has been MOST RECENTLY used
```

**Algorithm**:
```
1. Track recency of each page
2. On page fault:
   - Remove most recently used page
   - Load new page
```

**Characteristics**:
- Counter-intuitive approach
- Works well in specific scenarios
- Less common than LRU
- Good for sequential patterns

**When MRU is Better**:
- Sequential access patterns
- Working set changes regularly
- Next reference likely to different pages

**Example Scenario**:
```
Access pattern: 1,2,3,4,5,6,1,2,3,4,5,6...

LRU: Removes recently used = good fit here
MRU: Removes oldest in-use = better for sequential

If accessing 1 through 6 repeatedly:
- LRU keeps 6 (just used)
- MRU removes 6, keeps 1-5 for next cycle
```

### 5. Second Chance (Clock) Algorithm

**Concept**:
```
Give pages a SECOND CHANCE by checking reference bit
```

**Algorithm**:
```
1. Maintain reference bit for each page
2. Arrange pages in circular queue
3. On page fault:
   - Check reference bit of current page
   - If 0: Replace it
   - If 1: Set to 0, move to next (give second chance)
   - Repeat until find victim (bit=0)
```

**Characteristics**:
- FIFO variant with improvement
- Uses single-bit reference flag
- Clock/circular buffer structure
- Practical algorithm

**Advantages**:
✅ Simple modification of FIFO
✅ Considers page usage
✅ Single-bit tracking (cheap)
✅ Better than pure FIFO
✅ Used in some real systems

**Disadvantages**:
❌ Coarse granularity (1 bit)
❌ Multiple sweeps possible
❌ Still not as good as LRU
❌ Needs reference bit updates

**Time Complexity**: O(n) worst case
**Space Complexity**: O(n)

**Example**:
```
Physical Memory: 3 frames
Page   Ref Bit
1      1
2      0
3      1

New page fault:
- Check page 1: ref=1, set to 0, advance
- Check page 2: ref=0, REMOVE this page
- Load new page

Queue becomes: [3:1, new:0, 1:0]
```

### 6. Optimal Page Replacement (Belady's Algorithm)

**Concept**:
```
Replace the page that will NOT be used for the LONGEST time in future
```

**Algorithm**:
```
1. When page fault occurs:
   - Look ahead in reference string
   - Find page not needed longest
   - Replace it
2. Continue
```

**Characteristics**:
- Theoretical optimal algorithm
- Requires future knowledge (impossible in practice)
- Used as benchmark for other algorithms
- Demonstrates best possible performance

**Advantages**:
✅ Minimizes page faults theoretically
✅ Provides performance upper bound
✅ Useful for algorithm comparison
✅ Shows optimal behavior

**Disadvantages**:
❌ Impossible to implement (needs future)
❌ Only for off-line analysis
❌ Not practical
❌ Real systems use approximations

**Use Case**: Benchmarking other algorithms

**Example**:
```
Page Reference: 1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5
(We know full sequence in advance)

Physical Memory: 3 frames

Page  Memory      Future Reference    Action
1     [1]        [2,3,4,1,2,5,...]   Fault
2     [1,2]      [3,4,1,2,5,...]     Fault
3     [1,2,3]    [4,1,2,5,...]       Fault
4     [4,2,3]    [1,2,5,1,2,3,4,5]   Fault (remove 1, needed furthest: index 4)
1     [4,2,1]    [2,5,1,2,3,4,5]     Fault (remove 3, not in future!)
      Wait, 3 not in remaining! Remove page with furthest next use: 4 at index 4

Optimal: 6 faults (theoretical minimum for this sequence)
```

## Comparison Table

| Algorithm | FIFO | LRU | LFU | MRU | 2nd Chance | Optimal |
|-----------|------|-----|-----|-----|------------|---------|
| Simplicity | ★★★★★ | ★★★ | ★★★ | ★★★★ | ★★★★ | ★☆☆ |
| Performance | ★★☆☆☆ | ★★★★ | ★★★ | ★★☆☆ | ★★★ | ★★★★★ |
| Hardware Support | ✓ | ✓ | ✗ | ✗ | ✓ | ✗ |
| Practical Use | Rare | Common | Some | Rare | Some | No |
| Belady's Anomaly | Yes | No | No | No | Possible | No |

## Module Structure

```
page replacement/
├── README.md          # This file
├── lruindex.html      # Main visualization
├── wiki.html          # Educational reference
├── compare.js         # Comparison tool
├── lru.js             # LRU implementation
├── [algorithm implementations]
├── [reference images]
└── css/               # Styling
```

## How to Use

### Basic Workflow

1. **Open lruindex.html** in browser
2. **Enter page reference sequence** (comma or space separated)
3. **Enter number of frames** (physical memory slots)
4. **Select algorithm(s)** to visualize:
   - FIFO
   - LRU
   - LFU
   - MRU
   - Second Chance
   - Optimal
5. **Click Visualize/Run**
6. **Observe**:
   - Memory state timeline
   - Page faults occurrence
   - Statistics

### Comparison Mode

1. **Input same reference sequence**
2. **Select multiple algorithms**
3. **View side-by-side comparison**
4. **Analyze fault counts**
5. **Understand differences**

### Reference Sequence Format

Examples:
- `1 2 3 4 1 2 5 1 2 3 4 5`
- `1,2,3,4,1,2,5,1,2,3,4,5`
- `7 0 1 2 0 3 0 4 2 3 0 3 2 1 2 0 1 7 0 1`

## Key Metrics

### Page Fault Rate

```
Fault Rate = (Total Faults) / (Total References) × 100
```

**Good**: < 5%
**Fair**: 5-10%
**Poor**: > 10%

### Page Replacement Count

Number of times page evicted = number of faults (after initial loads)

## Real-World Implementation

### Operating Systems

**Linux**:
- Uses LRU approximation with working set
- Reference bits with aging
- Active/inactive lists

**Windows**:
- Working set model
- Trim operations
- Modified page list

**macOS**:
- Anonymous pages (LRU-like)
- File-backed pages
- Unified buffer cache

### Hardware Support

Modern CPUs provide:
- Reference bits (accessed flag)
- Dirty bits (modified flag)
- TLB (Translation Lookaside Buffer)
- These reduce overhead significantly

## Belady's Anomaly

Some algorithms experience more faults with more frames!

**Example** (FIFO):
```
With 3 frames: 9 faults
With 4 frames: 10 faults (Belady's anomaly!)

Reference: 1,2,3,4,1,2,5,1,2,3,4,5
```

**Why LRU avoids this**: Stack property - if page in stack with n frames, it's in stack with n+1 frames.

## Advanced Topics

- **Working Set Model**: Concepts for optimal page replacement
- **Page Buffering**: Pre-fetching strategies
- **Demand Paging**: Loading on demand vs pre-loading
- **Thrashing**: Too many page faults, system becomes I/O bound

## Practice Problems

1. Trace FIFO for given sequence
2. Calculate LRU for reference string
3. Compare algorithms on same input
4. Find sequences where MRU beats LRU
5. Analyze performance trade-offs

## References

- Silberschatz, Galvin, Gagne - "Operating System Concepts"
- Denning, P.J. - Working set model papers
- Wikipedia articles on paging

---

**Module Created**: 2026  
**Last Updated**: 2026
