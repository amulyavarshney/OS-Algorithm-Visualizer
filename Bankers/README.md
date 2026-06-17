# Banker's Algorithm

An interactive visualization of the Banker's Algorithm for resource allocation and deadlock avoidance in operating systems.

## Overview

The Banker's Algorithm is a resource allocation and deadlock avoidance algorithm that tests for safety by simulating the allocation of predetermined maximum possible amounts of all resources. It ensures that the system never enters an unsafe state, thus preventing deadlock.

## Algorithm Concept

### Core Idea

The algorithm is named after banking: a banker never allocates cash in a way that would prevent fulfilling all customer loans. Similarly, an OS never allocates resources in a way that would leave the system in an unsafe state.

### Safety State

A system state is **safe** if:
1. There exists a sequence of processes that can complete
2. Each process in the sequence needs only available resources plus resources held by processes earlier in sequence
3. When processes complete, they release resources for others

An **unsafe state** doesn't guarantee deadlock but makes it possible.

## Data Structures

### Required Information

**Available Vector (Available)**
- Number of available units of each resource
- Updated as processes release resources

**Maximum Matrix (Max)**
- Maximum demand of each process for each resource type
- Declared at process startup

**Allocation Matrix (Alloc)**
- Resources currently allocated to each process
- Updated dynamically

**Need Matrix (Need)**
- Remaining resources each process might request
- Calculated as: `Need[i][j] = Max[i][j] - Alloc[i][j]`

### Example
```
Resources: 3 (A, B, C)
Processes: 3 (P0, P1, P2)

Max Matrix:
    A  B  C
P0  7  5  3
P1  3  2  2
P2  9  0  2

Allocation Matrix:
    A  B  C
P0  0  1  0
P1  2  0  0
P2  3  0  2

Available: [3, 3, 1]
Need Matrix (Max - Alloc):
    A  B  C
P0  7  4  3
P1  1  2  2
P2  6  0  0
```

## Algorithm Steps

### 1. Safety Algorithm

Determines if a state is safe:

**Input**: Current system state
**Output**: Safe sequence or "Unsafe"

**Steps**:
```
1. Initialize Work = Available, Finish = false for all processes
2. Find process i where:
   - Finish[i] == false
   - Need[i] <= Work
3. If found:
   - Add Allocation[i] to Work
   - Set Finish[i] = true
   - Go to step 2
4. If all Finish[i] == true: System is safe
5. If no such process found: System is unsafe
```

### 2. Resource Request Algorithm

When process requests resources:

**Input**: Process P_i requests resources Request[i]

**Steps**:
```
1. Check if Request[i] <= Need[i]
   - If false: Error (exceeded maximum)

2. Check if Request[i] <= Available
   - If false: Process waits

3. If sufficient available:
   - Tentatively allocate resources
   - Update: Available, Allocation, Need
   - Run safety algorithm
   
4. If state remains safe:
   - Grant resources
   - Update actual allocation
   
5. If unsafe:
   - Deny request
   - Restore previous state
   - Process waits
```

## Features of This Module

### Interactive Visualization

**Input Parameters**:
- Number of resource types
- Number of processes
- Maximum demand for each process
- Initial allocation matrix
- Need matrix

**Running the Algorithm**:
1. Enter number of resources
2. Define resource type names
3. Specify number of processes
4. Input maximum demands
5. Input initial allocations
6. Simulation processes requests
7. View safety analysis

### Output Information

**Safety Check Results**:
- Safe sequence (if exists)
- Unsafe indication
- Available resources
- Resource allocation table
- Need matrix

**Step-by-Step Execution**:
- Shows resource allocation process
- Demonstrates safety checking
- Illustrates deadlock avoidance

## Module Files

```
Bankers/
├── README.md          # This file
├── banker.html        # Main visualization interface
├── banker.js          # Algorithm implementation
├── wiki.html          # Educational content
├── style.css          # Module-specific styling
└── stl.css            # Additional styles
```

## Key Concepts

### Deadlock Prevention vs Avoidance

**Prevention**: Ensure deadlock conditions never hold
- Deny one deadlock condition
- Very restrictive, low resource utilization

**Avoidance** (Banker's approach):
- Allow deadlock conditions, but avoid unsafe states
- More efficient resource utilization
- Requires advance knowledge of resource needs

### Four Deadlock Conditions

All must be true simultaneously for deadlock:
1. **Mutual Exclusion**: Resource cannot be shared
2. **Hold and Wait**: Process holds resources while waiting for others
3. **No Preemption**: Resources cannot be forcibly taken
4. **Circular Wait**: Processes form cycle of waiting

Banker's Algorithm maintains these conditions while ensuring system never enters state that could lead to deadlock.

### When Banker's Algorithm Applies

**Suitable for**:
- Systems with known maximum resource needs
- Conservative systems (safety is priority)
- Limited resource scenarios
- Real-time critical systems

**Not suitable for**:
- Dynamic maximum demands
- Unpredictable resource requirements
- High-throughput systems
- Scenarios where performance > safety

## Algorithm Advantages

✅ **Deadlock Avoidance**: Prevents deadlock occurrence
✅ **Known Sequence**: Shows safe process completion order
✅ **Resource Efficiency**: Better than prevention approaches
✅ **Safe State Guarantee**: System always safe
✅ **Fairness**: Can incorporate fairness constraints

## Algorithm Disadvantages

❌ **Advance Knowledge**: Requires knowing maximum demands upfront
❌ **Overhead**: Computational complexity for large systems
❌ **Conservative**: May deny safe requests if no sequence found
❌ **Not Practical**: Rarely fully implemented in real OS
❌ **Assumption Violation**: Real processes often exceed declared max

## Complexity Analysis

**Time Complexity**:
- Safety Algorithm: O(n² × m) where n = processes, m = resources
- Resource Request: O(n² × m) including safety check

**Space Complexity**:
- O(n × m) for storing matrices

## Real-World Implementation

### Linux/Unix
- Doesn't use Banker's Algorithm
- Uses timeout-based deadlock detection
- Focuses on prevention through ordered locking

### Windows
- No complete Banker's implementation
- Uses deadlock prevention strategies
- Resource allocation simpler than banking model

### Database Systems
- Some use variants for lock management
- Consider resource wait-for graphs
- Detect and resolve circular dependencies

## Example Execution

### Initial State
```
Process: P0, P1, P2
Resources: A=10, B=5, C=7

Max Demand:
      A  B  C
P0    7  5  3
P1    3  2  2
P2    9  0  2

Initial Allocation:
      A  B  C
P0    0  1  0
P1    2  0  0
P2    3  0  2

Available: [5, 4, 5]
Need:
      A  B  C
P0    7  4  3
P1    1  2  2
P2    6  0  0
```

### Safety Check

1. Check P1: Need[1] = [1,2,2] <= Available [5,4,5] ✓
   - Mark P1 as finished
   - Available becomes [7, 4, 5]

2. Check P0: Need[0] = [7,4,3] <= Available [7,4,5] ✓
   - Mark P0 as finished
   - Available becomes [7, 5, 5]

3. Check P2: Need[2] = [6,0,0] <= Available [7,5,5] ✓
   - Mark P2 as finished

4. **Result**: Safe sequence exists: P1 → P0 → P2

## How to Use

### Step-by-Step Usage

1. **Open banker.html** in browser
2. **Enter number of resource types** (e.g., 3)
3. **Enter number of processes** (e.g., 3)
4. **Input Maximum demand** for each process
5. **Input Initial allocation** for each process
6. **System calculates need matrix** automatically
7. **Click "Run"** to execute safety algorithm
8. **Review results**: Safe sequence or unsafe indication

### Parameter Guidelines

**Number of Resources**: 2-4 typical (A, B, C, D)
**Number of Processes**: 3-5 for educational purposes
**Demands**: Realistic based on resource type
**Allocations**: Started with partial or zero allocation

## Educational Outcomes

Understanding this module helps you:
1. Grasp deadlock avoidance strategies
2. Recognize limitations of resource allocation
3. Understand why processes need maximum declarations
4. Learn matrix-based algorithm design
5. Apply safety concepts to system design

## Variations and Extensions

**Resource Types**: Extend to multiple resource types
**Process Types**: Different maximum demands per type
**Priority Classes**: Different deadline/priority handling
**Dynamic Limits**: Mechanisms to update max demands
**Preemption**: Allow resource preemption to restart processes

## References

- Dijkstra, E.W. (1965) - Original Banker's Algorithm paper
- Silberschatz, Galvin, Gagne - "Operating System Concepts"
- Tanenbaum, A.S. - "Modern Operating Systems"

## Limitations to Remember

⚠️ **Not Production Ready**: Theoretical tool, not practical for real OS
⚠️ **Overhead**: Performance penalty for safety guarantee
⚠️ **Assumptions**: Requires static, known resource demands
⚠️ **Scalability**: Doesn't scale well to many resources/processes

## Related Concepts

- **Deadlock Detection**: Wait-for graph methods
- **Resource Graphs**: Alternative visualization
- **Dining Philosophers**: Classic deadlock problem
- **Lock Management**: Ordered locking strategies

---

**Module Created**: 2026  
**Last Updated**: 2026
