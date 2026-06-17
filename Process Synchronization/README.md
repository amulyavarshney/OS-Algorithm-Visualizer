# Process Synchronization

This module demonstrates classic process synchronization problems that illustrate inter-process communication, mutual exclusion, and coordination challenges in concurrent systems.

## Overview

Process synchronization addresses the challenges of concurrent access to shared resources. When multiple processes or threads access shared data simultaneously, race conditions and data inconsistencies can occur. This module visualizes five classic synchronization problems and their solutions.

## Synchronization Problems

### 1. Producer-Consumer Problem
**Files**: `producer_consumer.html`, `pc_wiki.html`

**Problem Description**:
- One or more producers generate data items
- One or more consumers process data items
- Limited-size buffer stores items temporarily
- Challenge: Prevent buffer overflow (producer) and underflow (consumer)

**Key Concepts**:
- **Mutual Exclusion**: Only one process accesses buffer at a time
- **Signal/Wait**: Producers wait if buffer full; consumers wait if buffer empty
- **Synchronization Primitives**: Semaphores or monitors used for coordination

**Solutions**:
- Semaphore-based solution using mutex and counting semaphores
- Monitor-based solution with condition variables
- Bounded buffer implementation

**Real-World Applications**:
- Data pipeline systems
- Task queues in web servers
- Print spooling systems
- Video encoding pipelines

**Execution Flow**:
```
Producer: Wait if full → Add item → Signal consumer
Consumer: Wait if empty → Remove item → Signal producer
```

### 2. Reader-Writer Problem
**Files**: `reader_writer.html`, `rw_wiki.html`

**Problem Description**:
- Multiple readers can simultaneously read shared data
- Only one writer can access shared data (exclusive access)
- Readers should not read while writer is writing
- Writers should not write while readers are reading

**Variants**:
1. **First Readers-Writers Problem**: Readers have priority (no reader waits if writers waiting)
2. **Second Readers-Writers Problem**: Writers have priority (no writer waits if readers active)

**Solutions**:
- Semaphore-based with reader/writer locks
- RWLock primitive (read-write lock)
- Monitor-based coordination
- Priority-based queueing

**Real-World Applications**:
- Database systems with multiple concurrent users
- Caching systems with frequent reads
- File systems with concurrent access
- Web servers serving static content

**Key Challenge**: Balancing reader throughput with writer fairness

### 3. Dining Philosophers Problem
**Files**: `dining.html`, `dining_wiki.html`

**Problem Description**:
- N philosophers sit at round table with N chopsticks (one between each pair)
- Philosophers alternate between thinking and eating
- To eat, philosopher needs both left and right chopsticks
- Challenge: Prevent deadlock, starvation, and ensure progress

**Classic Issues**:
- **Deadlock**: All philosophers pick up left chopstick, wait for right = stuck
- **Starvation**: Philosopher never gets both chopsticks
- **Livelock**: Continuous synchronization without progress

**Solutions**:
1. **Asymmetric Solution**: Odd-numbered philosophers pick left first, even-numbered pick right first
2. **Resource Hierarchy**: Total ordering of resources (chopsticks)
3. **Arbitration**: Central waiter grants permission to eat
4. **Monitor-Based**: Shared monitor controls eating

**Lessons**:
- Illustrates deadlock conditions
- Shows importance of resource ordering
- Demonstrates synchronization techniques
- Models many real-world resource allocation problems

### 4. Cigarette Smokers Problem
**Files**: `cigarette_smokers.html`, `cigrate_smoke_wiki.html`

**Problem Description**:
- Three smokers each have one ingredient (tobacco, paper, matches)
- Smoker needs all three ingredients to smoke
- Three agents push ingredients to a table
- Challenge: Ensure proper coordination and avoid deadlock

**Complexity**:
- More complex than dining philosophers
- Requires selective notification/signaling
- Cannot be solved with binary semaphores alone
- Demonstrates limitations of semaphore-based solutions

**Solutions**:
- Monitor-based solution with condition variables
- Requires selective wake-up mechanism
- Illustrates need for advanced synchronization primitives

**Key Insight**: Some synchronization problems require more expressive primitives than simple semaphores

**Real-World Analogy**:
- Process A has resource X, needs Y and Z
- Process B has resource Y, needs X and Z
- Process C has resource Z, needs X and Y
- Central coordinator allocates resources

### 5. Sleeping Barber Problem
**Files**: `sleepingBarber.html`, `sb_wiki.html`

**Problem Description**:
- Barber gives haircuts; barbershop has limited waiting chairs
- Customers arrive and either wait or leave if shop is full
- Barber sleeps if no customers; wakes when customer arrives
- Challenge: Coordinate customers and barber efficiently

**Entities**:
- **Barber**: Sleeps when idle, serves customers when available
- **Waiting Room**: Limited capacity (N chairs)
- **Customers**: Arrive, wait (if space), or leave

**Challenge Areas**:
- Race condition: Barber might miss customer arrival
- Barber synchronization: Sleep/wake coordination
- Capacity management: Reject customers when full

**Solutions**:
- Semaphore solution: Barber semaphore, customer semaphore, mutex
- Monitor solution: Cleaner and more readable

**State Management**:
```
Barber States: [Sleeping] ←→ [Serving]
Customer States: [Waiting] → [Being Served] → [Leaving]
```

**Real-World Applications**:
- Thread pool workers and task queues
- Queue management systems
- Service stations with limited capacity
- Web server request handling

## Module Structure

```
Process Synchronization/
├── README.md                      # This file
├── producer_consumer.html         # Producer-Consumer visualization
├── reader_writer.html             # Reader-Writer visualization
├── dining.html                    # Dining Philosophers visualization
├── cigarette_smokers.html         # Cigarette Smokers visualization
├── sleepingBarber.html            # Sleeping Barber visualization
├── pc_wiki.html                   # Producer-Consumer educational content
├── rw_wiki.html                   # Reader-Writer educational content
├── dining_wiki.html               # Dining Philosophers educational content
├── cigrate_smoke_wiki.html        # Cigarette Smokers educational content
├── sb_wiki.html                   # Sleeping Barber educational content
├── js/                            # Algorithm implementations
├── css/                           # Module styling
└── includes/                      # Reusable components
```

## Synchronization Primitives

### Semaphore
```
Binary Semaphore (Mutex):
- Values: 0 (locked) or 1 (unlocked)
- Wait(): Decrement, block if 0
- Signal(): Increment, wake waiting process

Counting Semaphore:
- Values: 0 to N
- Used for resource pools
```

### Monitor
```
Object with synchronized access
- Mutual Exclusion: Only one process active
- Condition Variables: Wait/Signal for specific conditions
- More structured than semaphores
```

### Locks
```
Read-Write Lock:
- Multiple readers OR one writer
- Critical for reader-heavy workloads

Mutex (Mutual Exclusion):
- Simple binary lock
- Used for critical sections
```

## Key Concepts

### Race Condition
Outcome depends on timing of concurrent processes accessing shared resources.

### Critical Section
Code segment accessing shared resources that must be executed by only one process at a time.

### Mutual Exclusion
Ensuring only one process executes critical section at a time.

### Deadlock
Processes wait indefinitely for each other, no progress.

### Starvation
Process waits indefinitely while others make progress.

### Livelock
Processes continue running but make no progress (busy-waiting without blocking).

## Problem Complexity Comparison

| Problem | Complexity | Producers/Consumers | Main Synchronization |
|---------|-----------|-------------------|---------------------|
| Producer-Consumer | Simple | 1:1 or many:many | Binary semaphore |
| Reader-Writer | Medium | Many:one (read:write) | RW-Lock |
| Dining Philosophers | Complex | N symmetric | Resource ordering |
| Cigarette Smokers | Hard | 3:3 asymmetric | Selective signaling |
| Sleeping Barber | Medium | 1:many | Monitor pattern |

## How to Use

### Basic Workflow

1. **Select Problem**: Choose from navigation menu
2. **Configure Parameters**:
   - Number of processes/threads
   - Resource counts
   - Processing times
   - Buffer sizes (where applicable)
3. **Run Simulation**: Start visualization
4. **Observe**:
   - Process states (thinking, waiting, executing)
   - Resource allocation
   - Synchronization events
   - Potential deadlock/starvation

### Interactive Features

- **Step-by-Step Execution**: Watch synchronization in action
- **Event Timeline**: See sequence of operations
- **State Displays**: Current state of all processes
- **Statistics**: Deadlock/starvation detection

## Educational Value

This module teaches:
1. Concurrency challenges and complexity
2. Synchronization mechanism effectiveness
3. Deadlock and starvation conditions
4. Fairness vs efficiency trade-offs
5. Design patterns for concurrent systems

## Common Issues and Solutions

| Issue | Cause | Solution |
|-------|-------|----------|
| Deadlock | Circular wait for resources | Ordered acquisition, arbitration |
| Starvation | Unfair scheduling | Priority escalation, queueing |
| Busy-Waiting | Continuous polling | Blocking primitives, condition variables |
| Race Condition | Unprotected shared access | Mutual exclusion, atomic operations |

## Real-World Connections

- **Database Systems**: Reader-Writer patterns for concurrent access
- **Thread Pools**: Sleeping Barber model for worker management
- **Message Queues**: Producer-Consumer model for async processing
- **Cache Systems**: Read-Write lock patterns for performance
- **Operating Systems**: All problems relevant to kernel design

## Advanced Topics

- **Condition Variables**: More expressive than semaphores
- **Monitor Patterns**: Structured synchronization approach
- **Fair Queueing**: Prevent starvation
- **Priority Inheritance**: Solve priority inversion
- **Lock-Free Algorithms**: Avoid locks entirely

## References

- Silberschatz, Galvin, Gagne - "Operating System Concepts"
- Dijkstra's Semaphore Papers
- Monitor patterns and concurrent programming literature
- Real-world system implementations (Linux, Windows)

---

**Module Created**: 2026  
**Last Updated**: 2026
