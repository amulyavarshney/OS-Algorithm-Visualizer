# Process Scheduling Algorithms

This module provides interactive visualizations of various CPU scheduling algorithms used by operating systems to determine the order in which processes execute on the CPU.

## Overview

Process scheduling is fundamental to multitasking operating systems. The scheduler must allocate CPU time among competing processes efficiently. This module allows you to:
- Input process details (arrival times, burst times, priorities)
- Visualize how different algorithms schedule processes
- Compare scheduling metrics (turnaround time, waiting time, response time)
- Understand algorithm trade-offs

## Algorithms Included

### 1. First Come First Serve (FCFS)
**File**: `src/fcfs.html`

**Characteristics**:
- Non-preemptive algorithm
- Simplest scheduling algorithm
- Processes execute in the order they arrive
- Fair but may suffer from convoy effect

**Use Cases**:
- Batch processing systems
- Educational demonstrations
- Systems where fairness is important over efficiency

**Metrics**:
- Average Waiting Time
- Average Turnaround Time

### 2. Shortest Job First (SJF)
**File**: `src/sjf.html`

**Characteristics**:
- Non-preemptive algorithm
- Selects process with shortest burst time
- Minimizes average waiting time
- Cannot be implemented without knowing burst times in advance

**Use Cases**:
- Systems where job duration is known
- Minimizing average waiting time scenarios

**Advantages**:
- Optimal for average waiting time
- Low overhead

**Disadvantages**:
- Starvation of longer processes
- Requires prior knowledge of burst times

### 3. Round Robin (RR)
**File**: `src/rr.html`

**Characteristics**:
- Preemptive algorithm
- Each process gets a time quantum (time slice)
- If process doesn't complete, it goes to back of queue
- Fair allocation of CPU time

**Parameters**:
- Time Quantum (typically 10-100ms)

**Use Cases**:
- General-purpose time-sharing systems
- Interactive systems
- Most modern operating systems

**Trade-offs**:
- Fair but higher context switching overhead
- Performance depends on time quantum selection

### 4. Priority Scheduling
**File**: `src/priority.html`

**Characteristics**:
- Preemptive or non-preemptive variant
- Executes process with highest priority first
- Can use static or dynamic priorities
- Prone to starvation without aging

**Variants**:
- Preemptive: High-priority process can interrupt lower-priority ones
- Non-preemptive: Completes once started

**Use Cases**:
- Real-time systems
- Systems with mixed workloads
- Embedded systems with critical tasks

**Challenges**:
- Starvation: Low-priority processes may never execute
- Solution: Aging - gradually increase priority over time

### 5. Shortest Remaining Time First (SRTF)
**File**: `src/srtf.html`

**Characteristics**:
- Preemptive version of SJF
- At each decision point, CPU goes to process with shortest remaining time
- Minimizes average waiting time among preemptive algorithms
- Higher overhead due to frequent context switching

**Advantages**:
- Optimal for average waiting time (preemptive)
- Responds to new arrivals

**Disadvantages**:
- Requires remaining time estimates
- High context switching overhead
- Starvation risk for long processes

### 6. Multilevel Queue Scheduling
**File**: `src/multilevelqueue.html`

**Characteristics**:
- Divides processes into multiple queues by priority class
- Each queue can use different scheduling algorithm
- Typically includes: foreground (interactive) and background (batch) queues
- Fixed or floating priorities between queues

**Use Cases**:
- Mixed workload systems
- Systems with different process types
- Time-sharing systems with batch jobs

**Structure**:
```
Priority Level | Queue Type        | Algorithm
High           | System Processes  | Priority
               | Interactive       | RR
               | Batch            | FCFS
```

## Module Structure

```
Process Scheduling/
├── README.md                    # This file
├── src/                        # Individual algorithm pages
│   ├── fcfs.html              # FCFS visualization
│   ├── sjf.html               # SJF visualization
│   ├── rr.html                # Round Robin visualization
│   ├── priority.html          # Priority Scheduling visualization
│   ├── srtf.html              # SRTF visualization
│   └── multilevelqueue.html   # Multilevel Queue visualization
├── js/                        # JavaScript implementations
│   └── [algorithm-specific implementations]
└── css/                       # Styling
```

## How to Use

### Basic Workflow

1. **Open Algorithm Page**: Select algorithm from main menu or directly access `src/[algorithm].html`

2. **Input Parameters**:
   - **Process Details**: Burst time, arrival time, priority (if applicable)
   - **Algorithm Parameters**: Time quantum (for RR), priorities (for priority scheduling)

3. **Run Simulation**: Click "Execute" or "Run" button

4. **Analyze Results**:
   - **Gantt Chart**: Visual representation of process execution timeline
   - **Metrics**: Turnaround time, waiting time, response time
   - **Statistics**: Averages and comparisons

### Example: FCFS

```
Processes:
P1: Arrival=0, Burst=8
P2: Arrival=1, Burst=4
P3: Arrival=2, Burst=2

Execution Order: P1 → P2 → P3
Turnaround Times: P1=8, P2=11, P3=12
Average Waiting Time = (0 + 7 + 10) / 3 = 5.67
```

### Example: Round Robin (Quantum=2)

```
Same processes with RR:
P1 → P2 → P3 → P1 → P2 → P3 → P1 ...
More interactive response, higher context switching
```

## Key Concepts

### Turnaround Time
- Time from submission to completion
- Formula: Completion Time - Arrival Time

### Waiting Time
- Time process spends in ready queue
- Formula: Turnaround Time - Burst Time

### Response Time
- Time from submission to first execution
- Important for interactive systems

### Convoy Effect
- In FCFS, long process delays all subsequent processes
- Demonstrates why FCFS is not optimal for interactive systems

## Scheduling Criteria Comparison

| Algorithm | FCFS | SJF | RR | Priority | SRTF |
|-----------|------|-----|----|---------|----|
| CPU Utilization | Fair | High | Fair | Depends | High |
| Throughput | Low | High | Medium | Depends | High |
| Turnaround Time | Poor | Best | Fair | Depends | Best |
| Waiting Time | Poor | Best | Fair | Depends | Best |
| Response Time | Poor | Poor | Best | Depends | Best |
| Starvation | No | Yes | No | Yes | Yes |
| Context Switches | Low | None | High | Depends | High |

## Interactive Features

- **Custom Input**: Enter your own process details
- **Visual Gantt Chart**: See execution timeline
- **Metrics Display**: Automatic calculation of scheduling metrics
- **Algorithm Comparison**: Run multiple algorithms on same input
- **Step-by-Step Execution**: Watch algorithm progression

## Educational Value

This module helps you understand:
1. How OS schedulers work
2. Trade-offs between scheduling objectives
3. Why different algorithms suit different scenarios
4. Importance of preemption vs non-preemption
5. Practical scheduling implementation

## Common Questions

**Q: Why is FCFS not used in interactive systems?**
A: FCFS causes long processes to block all others (convoy effect), making system unresponsive.

**Q: How do I choose time quantum for Round Robin?**
A: Too small = excessive context switching; too large = approaches FCFS. Usually 10-100ms works well.

**Q: Can SJF be used in real systems?**
A: Not directly, as burst times aren't known. Estimated times or historical data can be used.

**Q: What prevents starvation in Priority Scheduling?**
A: Aging - gradually increase priority of waiting processes over time.

## Real-World Applications

- **Windows Scheduler**: Uses multilevel feedback queues
- **Linux CFS**: Completely Fair Scheduler for fair distribution
- **Real-Time Systems**: Priority-based scheduling for deterministic behavior
- **Mobile OS**: Various adaptive scheduling based on process type

## References

- Silberschatz, Galvin, Gagne - "Operating System Concepts"
- Modern Operating Systems textbooks
- Linux Kernel Scheduler documentation

---

**Module Created**: 2026  
**Last Updated**: 2026
