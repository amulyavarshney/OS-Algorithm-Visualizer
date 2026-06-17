# Memory Management - Fixed Partition Allocation

This module visualizes memory allocation strategies used in fixed partition memory management schemes.

## Overview

Memory management is crucial for efficient OS operation. This module demonstrates different allocation strategies for partitioning main memory into fixed-size chunks. Each strategy has different trade-offs between memory utilization and allocation speed.

## Memory Management Concepts

### Memory Hierarchy

```
CPU Registers (fastest, smallest)
    ↓
Level 1 Cache
    ↓
Level 2 Cache
    ↓
Main Memory (RAM)
    ↓
Secondary Storage (slowest, largest)
```

### Memory Organization

**Fixed Partition Allocation**:
- Memory divided into fixed-size partitions
- Each partition holds one process
- Simple to implement
- Poor memory utilization

**Partition Allocation Methods**:
- First Fit
- Best Fit
- Worst Fit
- Next Fit

## Allocation Strategies

### 1. First Fit

**Algorithm**:
```
For each new process:
  Scan partitions from beginning
  Allocate to FIRST partition that has sufficient space
  Return
```

**Characteristics**:
- Simplest and fastest algorithm
- Scans from beginning each time (or from last allocation)
- May leave large usable spaces unused

**Advantages**:
✅ Fast execution
✅ Minimal overhead
✅ Simple implementation
✅ Fair distribution

**Disadvantages**:
❌ Can waste memory
❌ Fragmentation near start of memory
❌ Leaves gaps

**Time Complexity**: O(n) where n = number of partitions

**Example**:
```
Memory Layout (total 1000):
[Partition1: 100]
[Free: 200]
[Partition2: 150]
[Free: 150]
[Partition3: 100]
[Free: 300]

Request: 180
Result: Allocates to first Free (200), wastes 20
```

### 2. Best Fit

**Algorithm**:
```
For each new process:
  Scan ALL partitions
  Find smallest partition with sufficient space
  Allocate to BEST (smallest sufficient) partition
  Return
```

**Characteristics**:
- Searches entire free list
- Allocates to smallest sufficient partition
- Minimizes wasted space in allocated partition
- May leave fragments

**Advantages**:
✅ Better memory utilization
✅ Minimizes wasted space per allocation
✅ Reduces large gaps
✅ Good for known partition sizes

**Disadvantages**:
❌ Slower than First Fit (must scan all)
❌ Can create many tiny unusable fragments
❌ Higher overhead
❌ May leave small gaps scattered

**Time Complexity**: O(n)

**Example**:
```
Memory Layout (total 1000):
[Partition1: 100]
[Free: 200]
[Partition2: 150]
[Free: 150]
[Partition3: 100]
[Free: 300]

Request: 180
Result: Allocates to Free (200), wastes 20
```

### 3. Worst Fit

**Algorithm**:
```
For each new process:
  Scan ALL partitions
  Find largest free partition
  Allocate to WORST (largest free) partition
  Return
```

**Characteristics**:
- Searches entire free list
- Allocates to largest available space
- Leaves largest fragments for future processes
- Tries to keep large gaps available

**Advantages**:
✅ Leaves largest possible remainder
✅ May accommodate future large processes
✅ Reduces very small fragments
✅ Predictable remainder size

**Disadvantages**:
❌ Slowest algorithm (scan all)
❌ Can still create fragmentation
❌ Not as good as Best Fit usually
❌ High overhead

**Time Complexity**: O(n)

**Example**:
```
Memory Layout (total 1000):
[Partition1: 100]
[Free: 200]
[Partition2: 150]
[Free: 150]
[Partition3: 100]
[Free: 300]

Request: 180
Result: Allocates to largest Free (300), wastes 120
```

### 4. Next Fit

**Algorithm**:
```
For each new process:
  Start scan from LAST allocation position
  Find next partition with sufficient space
  Allocate to next found
  Remember position for next time
  Return
```

**Characteristics**:
- Modified First Fit variant
- Remembers last allocation position
- Wraps around from end to beginning
- Distributes allocations more evenly

**Advantages**:
✅ Fast like First Fit
✅ Better distribution than pure First Fit
✅ Minimal overhead
✅ Reduces concentration at beginning

**Disadvantages**:
❌ Can still waste memory
❌ Implementation complexity increases
❌ Requires tracking state
❌ Still creates fragmentation

**Time Complexity**: O(n) average

**Example**:
```
Memory Layout (total 1000):
Position markers: [Start → ... → Last_allocation → ... → Wrap]

Request 1: Allocate from position 0
Request 2: Continue scan from last position
Request 3: Wrap around if needed
```

## Fragmentation Issues

### Internal Fragmentation

Memory allocated but not used within a partition.

**Example**:
```
Partition size: 100
Process needs: 80
Wasted: 20 (internal fragmentation)
```

**Causes**:
- Fixed partition sizes
- Process size smaller than partition
- Rounding up allocations

**Solutions**:
- Variable partitions (dynamic allocation)
- Compaction/defragmentation
- Better partitioning strategy

### External Fragmentation

Free memory split into multiple small chunks, none sufficient for new process.

**Example**:
```
[Used: 100] [Free: 30] [Used: 100] [Free: 40] [Used: 100] [Free: 30]

Total free: 100
But largest contiguous: 40 (insufficient for 50-byte process)
```

**Causes**:
- Process deallocation pattern
- Allocation strategy
- Variable process sizes

**Solutions**:
- Defragmentation/compaction
- Virtual memory with paging
- Different allocation strategy

## Comparison

| Strategy | Speed | Memory Use | Fragmentation | Overhead |
|----------|-------|-----------|----------------|----------|
| First Fit | Fastest | Fair | Moderate | Low |
| Best Fit | Slower | Good | Can be bad | Medium |
| Worst Fit | Slowest | Fair | Moderate | High |
| Next Fit | Fast | Good | Low | Low-Medium |

## Module Structure

```
MFT/
├── README.md          # This file
├── index.html         # Main visualization interface
├── mft.js             # Algorithm implementations
├── style.css          # Styling
└── [related files]
```

## How to Use

### Basic Workflow

1. **Open index.html** in browser
2. **Enter Total Memory Size** (e.g., 1000)
3. **Enter Number of Blocks** (partitions)
4. **Select Allocation Strategy**:
   - Best Fit
   - First Fit
   - Worst Fit
   - Next Fit
5. **Input process sizes** for allocation
6. **Click Execute/Run** to visualize
7. **Observe**:
   - Memory layout diagram
   - Allocation results
   - Fragmentation statistics
   - Comparison metrics

### Parameters

**Total Memory Size**: Total available memory (100-10000)
**Number of Blocks**: Partition count (3-10)
**Block Size**: Fixed size for each partition
**Process Sizes**: Variable sizes to allocate

## Key Metrics

### Memory Utilization

```
Utilization = (Used Memory) / (Total Memory) × 100
```

**Good**: > 80%
**Fair**: 60-80%
**Poor**: < 60%

### Fragmentation Ratio

```
Fragmentation = (Wasted Memory) / (Total Memory) × 100
```

**Good**: < 20%
**Fair**: 20-40%
**Poor**: > 40%

### Average Remaining Space

Average free space left after allocation.

## Real-World Scenarios

### Scenario 1: Batch Processing
```
Process 1: 250 MB
Process 2: 200 MB
Process 3: 150 MB
Total Memory: 1024 MB

Best Fit likely optimal for throughput
```

### Scenario 2: Interactive Systems
```
Variable process sizes
Frequent allocation/deallocation
Next Fit or First Fit preferred
```

### Scenario 3: Embedded Systems
```
Limited memory (< 1 GB)
Known process sizes
Best Fit with careful planning
```

## Limitations of Fixed Partitions

⚠️ **Inflexible**: Cannot adjust partition sizes
⚠️ **Wasted Space**: Unused portion within partitions (internal fragmentation)
⚠️ **Internal Fragmentation**: Always present
⚠️ **Limited Processes**: Number of partitions limits concurrent processes
⚠️ **Poor Utilization**: Typical 25-50% utilization

## Modern Alternatives

### Variable Partitions (Dynamic Allocation)
- Create partitions as needed
- Better memory utilization
- More complex to implement
- Higher overhead

### Paging
- Divide into small fixed pages
- Virtual memory support
- Reduces fragmentation
- More complex bookkeeping

### Segmentation
- Variable-sized logical segments
- Better matches program structure
- More flexible
- Complex management

## Algorithm Selection

**Choose First Fit when**:
- Speed is critical
- Simple implementation needed
- Overhead concerns

**Choose Best Fit when**:
- Memory utilization important
- Sufficient processing power
- Known distribution of process sizes

**Choose Worst Fit when**:
- Want to keep large gaps
- Few specific large processes expected
- Can afford overhead

**Choose Next Fit when**:
- Balance speed and utilization
- Uniform distribution desired
- Moderate overhead acceptable

## Historical Context

**Early Operating Systems**:
- Fixed partition schemes primary method
- Simple but inefficient
- DOS memory management

**Modern Systems**:
- Virtual memory with paging primary
- Fixed partitions mainly educational
- Some embedded systems still use

## Educational Value

This module teaches:
1. Memory allocation strategies
2. Fragmentation concepts
3. Algorithm trade-offs
4. Performance analysis
5. Resource management principles

## References

- Silberschatz, Galvin, Gagne - "Operating System Concepts"
- Tanenbaum, A.S. - "Modern Operating Systems"
- Historical OS documentation

## Practice Problems

1. Trace First Fit allocation for given sequence
2. Calculate fragmentation ratios
3. Compare algorithms on same dataset
4. Design worst-case scenario
5. Optimize for specific workload

---

**Module Created**: 2026  
**Last Updated**: 2026
