# Disk Scheduling Algorithms

Interactive visualizations of disk I/O scheduling algorithms that optimize the order of disk access requests.

## Overview

Disk scheduling determines the order in which disk I/O requests are serviced. The scheduling strategy affects disk throughput, average response time, and fairness. This module visualizes six different disk scheduling algorithms using interactive visualizations with Plotly graphs.

## Disk I/O Fundamentals

### Disk Structure

```
Disk Platter (rotating surface)
├── Tracks (concentric circles)
├── Sectors (divisions within tracks)
├── Blocks (smallest addressable units)
└── Cylinders (tracks at same position on multiple platters)
```

### Disk Access Time

**Total Access Time** = Seek Time + Rotational Delay + Transfer Time

**Seek Time** (dominant, ~5-15ms):
- Time for read/write head to move to track
- Non-linear with distance
- Varies: 0 to max travel time

**Rotational Delay** (~3ms average):
- Time waiting for sector to rotate under head
- Depends on disk RPM and initial position

**Transfer Time** (negligible, < 1ms):
- Time to read/write actual data

### Disk Request Queue

```
Request Queue:
- Multiple pending I/O requests
- Each specifies disk block number (0 to max cylinder)
- Requests arrive dynamically
- Scheduler determines order
```

## Scheduling Algorithms

### 1. First Come First Serve (FCFS)

**Concept**:
```
Service disk requests in the order they arrive
```

**Algorithm**:
```
1. Maintain queue of requests
2. Service next request in queue
3. After completing, move to next
```

**Characteristics**:
- Simplest scheduling algorithm
- Fair to all requests
- Predictable behavior
- No optimization

**Advantages**:
✅ Simple to implement
✅ Fair to all requests
✅ No overhead
✅ Predictable

**Disadvantages**:
❌ No optimization
❌ Can cause excessive head movement
❌ Poor average response time
❌ Ignores spatial locality

**Time Complexity**: O(n)
**Seek Pattern**: Unpredictable, can be very large

**Example**:
```
Current Head Position: 50
Queue: [10, 184, 39, 122, 199]

Service Order: 10, 184, 39, 122, 199
Head Movement: |50-10| + |10-184| + |184-39| + |39-122| + |122-199|
             = 40 + 174 + 145 + 83 + 77
             = 519 (very poor!)
```

### 2. Shortest Seek Time First (SSTF)

**Concept**:
```
Service request with MINIMUM seek time from current position
```

**Algorithm**:
```
1. Current position = head location
2. Find request closest to current position
3. Service it
4. Update current position
5. Repeat until all served
```

**Characteristics**:
- Greedy optimization
- Minimizes immediate seek time
- Generally good average time
- Can cause starvation

**Advantages**:
✅ Much better than FCFS
✅ Significant seek time reduction
✅ Good average response time
✅ Simple to implement

**Disadvantages**:
❌ Starvation possible
❌ Requests far from head may wait indefinitely
❌ Not optimal globally
❌ Favor middle cylinder

**Time Complexity**: O(n²) with careful implementation O(n log n)
**Seek Pattern**: Better locality

**Example**:
```
Current Head Position: 50
Queue: [10, 184, 39, 122, 199]

Service Order:
1. Closest to 50: 39 (distance: 11)
2. Closest to 39: 10 (distance: 29)
3. Closest to 10: 122 (distance: 112)
4. Closest to 122: 184 (distance: 62)
5. Closest to 184: 199 (distance: 15)

Head Movement: 11 + 29 + 112 + 62 + 15 = 229 (much better than FCFS!)
```

### 3. SCAN (Elevator) Algorithm

**Concept**:
```
Head moves in ONE DIRECTION serving all requests until end,
then reverses and moves in other direction
```

**Algorithm**:
```
1. Head moves in one direction (say, toward 0)
2. Service all requests in that direction
3. When reaching end (0 or max), reverse direction
4. Service all requests in other direction
5. Repeat
```

**Characteristics**:
- Like elevator in building
- Uniform service pattern
- Prevents starvation
- Good average time

**Advantages**:
✅ No starvation
✅ Better than SSTF
✅ Bounded waiting time
✅ Good average response
✅ Fair to all directions

**Disadvantages**:
❌ Not exactly optimal
❌ More complex than SSTF
❌ Slightly longer than SSTF average
❌ Uneven request distribution

**Time Complexity**: O(n)
**Seek Pattern**: Directed movement

**Example**:
```
Current Head Position: 50
Queue: [10, 184, 39, 122, 199]
Direction: Toward 0

Service Order:
1. Move toward 0: 39 (at 50, move to 39)
2. Continue toward 0: 10 (from 39 to 10)
3. Reached 0, reverse direction toward max
4. Move toward max: 122
5. Continue toward max: 184
6. Continue toward max: 199

Head Movement: |50-39| + |39-10| + |10-122| + |122-184| + |184-199|
             = 11 + 29 + 112 + 62 + 15 = 229
```

### 4. Circular SCAN (C-SCAN) Algorithm

**Concept**:
```
Head moves in ONE DIRECTION only, serving all requests,
then JUMPS BACK to opposite end WITHOUT servicing,
repeats
```

**Algorithm**:
```
1. Head moves in one direction (say, 0 to max)
2. Service all requests in that direction
3. Jump to opposite end (0)
4. Move in same direction again
5. Repeat
```

**Characteristics**:
- Circular, one-directional sweep
- More uniform response times
- Better for uniform load
- Prevents clumping

**Advantages**:
✅ More uniform service times
✅ Better for uniform load
✅ Prevents queue buildup at one end
✅ Fair response time distribution

**Disadvantages**:
❌ Wasted seek on return
❌ Slightly longer total time
❌ More complex implementation
❌ Jump back is inefficient

**Time Complexity**: O(n)
**Seek Pattern**: Unidirectional sweep

**Example**:
```
Queue: [10, 184, 39, 122, 199]
Direction: 0 to max (unidirectional)

Service Order:
1. 39 (toward max)
2. 122 (continuing)
3. 184 (continuing)
4. 199 (reaching far end)
5. Jump back to 0
6. Next cycle starts from 0

Prevents end starvation (10 served in next cycle)
```

### 5. LOOK Algorithm

**Concept**:
```
Modified SCAN: Head moves in direction only WHILE requests exist,
then reverses (doesn't necessarily reach end)
```

**Algorithm**:
```
1. Head moves in direction
2. Service requests in that direction
3. When no more requests in direction, reverse
4. Service requests in other direction
5. Repeat
```

**Characteristics**:
- Smarter SCAN variant
- Stops before absolute end
- Reduces unnecessary seek
- Similar service pattern

**Advantages**:
✅ Reduces wasted seek
✅ Better than SCAN
✅ Similar fairness
✅ Practical and efficient

**Disadvantages**:
❌ Still more complex than SSTF
❌ Slightly less optimal than SCAN in some cases
❌ Implementation complexity

**Time Complexity**: O(n)
**Seek Pattern**: Directed with early reversal

**Example**:
```
Head Position: 50
Queue: [10, 184, 39, 122, 199]

Service Order (moving toward max):
1. 122 (closest ahead)
2. 184
3. 199 (no more ahead, reverse)
4. Move toward min:
5. 39
6. 10

Head Movement: 11 + 62 + 15 + 112 + 29 = 229
(Better than SCAN: doesn't go to actual max before reversing)
```

### 6. C-LOOK Algorithm

**Concept**:
```
Modified C-SCAN: Head moves in direction while requests exist,
then JUMPS to opposite end and repeats (doesn't reach absolute end)
```

**Algorithm**:
```
1. Move in one direction servicing requests
2. When no more requests ahead, jump to end of requests in other direction
3. Move in other direction
4. When no more requests, jump to start
5. Repeat
```

**Characteristics**:
- Most practical algorithm
- Combines LOOK and C-SCAN benefits
- Minimal wasted seek
- Good fairness

**Advantages**:
✅ Best practical performance
✅ Minimal wasted movement
✅ Good fairness
✅ Used in real systems

**Disadvantages**:
❌ Most complex to implement
❌ Still sequential servicing
❌ Not optimal for all cases

**Time Complexity**: O(n)
**Seek Pattern**: Optimized circular

## Comparison Table

| Algorithm | Movement | Seek Time | Fairness | Complexity | Real Use |
|-----------|----------|-----------|----------|-----------|----------|
| FCFS | Chaotic | Poor | High | Very Low | Rarely |
| SSTF | Local | Good | Low | Low | Some |
| SCAN | Linear | Good | High | Medium | Common |
| C-SCAN | Circular | Fair | Highest | High | Common |
| LOOK | Linear | Better | High | Medium | Common |
| C-LOOK | Circular | Best | Highest | High | Common |

## Module Structure

```
Disk/
├── README.md          # This file
├── disk.html          # Main visualization
├── disk.js            # Algorithm implementations
├── lba.html          # LBA discussion
├── styles.css        # Styling
└── [reference images]
```

## Visualization Features

### Interactive Interface

1. **Algorithm Selection**: Dropdown to choose algorithm
2. **Current Position Input**: Set initial head position
3. **Request Queue Input**: Enter disk block requests
4. **Visual Display**:
   - Cylinder/track axis
   - Request positions
   - Head movement animation/diagram
   - Seek time calculation

### Plotly Graphs

- **Seek Distance**: Bar chart of distances
- **Movement Timeline**: Path visualization
- **Queue Status**: Request ordering
- **Performance Metrics**: Comparison stats

## How to Use

### Basic Workflow

1. **Open disk.html** in browser
2. **Select Algorithm** from dropdown
3. **Enter Disk Cylinders** (typical: 0-199 or 0-999)
4. **Enter Current Head Position** (e.g., 50)
5. **Input Request Queue** (comma/space separated):
   - Example: `10, 184, 39, 122, 199`
6. **Click Visualize**
7. **Observe**:
   - Head movement path
   - Total seek distance
   - Request service order
   - Performance metrics

### Comparison Mode

1. **Input same parameters**
2. **Run different algorithms**
3. **Compare seek times**
4. **Analyze effectiveness**

### Parameters

**Disk Cylinders**: 0 to N (typical 200 or 1000)
**Head Position**: Current cylinder
**Request Queue**: List of cylinder requests

## Key Metrics

### Total Seek Distance

```
Seek Distance = Σ |current_position - request|
```

Lower is better.

### Average Seek Time

```
Average = Total Seek Distance / Number of Requests
```

### Request Sequence

Order in which requests served.

## Real-World Applications

### Historical Context
- **Early OSes**: Primarily FCFS (no optimization)
- **1970s-80s**: SSTF widely used
- **Modern SSDs**: Algorithms less relevant (no mechanical delay)
- **HDD Still Used**: SCAN/LOOK variants in modern disks

### Current Systems

**Linux**:
- CFQ (Completely Fair Queuing)
- Deadline scheduler
- noop (no scheduling for SSDs)

**Windows**:
- Disk I/O prioritization
- Workload-based scheduling

**macOS**:
- Similar to modern systems
- SSD optimization

## Important Notes

### SSD Impact
Modern SSDs have:
- No mechanical seek
- Random access = sequential access (time-wise)
- Different scheduling considerations
- Priority often: fairness, isolation

### Rotational Media Still Relevant
- Large storage often rotational
- Archival systems use HDDs
- Backup systems use HDDs
- Scheduling still matters for throughput

## Practice Problems

1. Trace FCFS and SSTF for same queue
2. Calculate total seek for C-SCAN
3. Compare algorithms on uniform vs. clustered requests
4. Design worst-case for each algorithm
5. Analyze fairness with skewed load

## Educational Value

This module teaches:
1. Device I/O management
2. Scheduling algorithm types
3. Optimization trade-offs
4. Performance analysis
5. Practical system design

## References

- Silberschatz, Galvin, Gagne - "Operating System Concepts"
- Tanenbaum, A.S. - "Modern Operating Systems"
- Linux Kernel I/O Scheduler docs
- Storage system papers

---

**Module Created**: 2026  
**Last Updated**: 2026
