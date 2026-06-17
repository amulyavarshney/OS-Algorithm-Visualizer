# File Allocation Techniques

Interactive visualizations of file allocation strategies used to store files efficiently on disk.

## Overview

File allocation defines how files are stored on disk blocks. Different allocation strategies have trade-offs between space utilization, access speed, and implementation complexity. This module demonstrates three primary file allocation techniques.

## File System Basics

### Disk Organization

```
Disk Layout:
├── Boot Block (system startup)
├── Super Block (filesystem metadata)
├── Inode/FAT Area (file metadata)
└── Data Block Area (actual file contents)
```

### File Representation

**Inode Approach** (Unix/Linux):
- File metadata in inode structure
- Separate inode and data blocks
- Pointers in inode to data blocks

**FAT Approach** (DOS/Windows):
- File Allocation Table
- Central table tracks block chains
- Simpler but centralized

## Allocation Strategies

### 1. Contiguous Allocation

**Concept**:
```
Each file occupies consecutive disk blocks
```

**File Storage**:
- File stored in sequence of contiguous blocks
- File metadata tracks: starting block and length
- Example: Blocks 10-19 for a 10-block file

**Algorithm**:
```
1. To store file of size N:
   - Find N consecutive free blocks
   - Allocate all at once
   - Store: start block and length

2. To read file:
   - Start at start_block
   - Read length blocks sequentially
```

**Characteristics**:
- Simple to implement
- Fast sequential access
- External fragmentation problem
- Difficulty growing files

**Advantages**:
✅ Fast sequential access
✅ Minimal overhead
✅ Simple to implement
✅ Efficient disk head movement

**Disadvantages**:
❌ External fragmentation
❌ Cannot easily extend files
❌ Wastes space for gaps
❌ Requires contiguous space

**Example**:
```
Disk Layout (each # = block):

File A (size 3):  [A][A][A]______[B][B]___[C][C][C][C]_
Block allocation:
- File A: Start=0, Length=3
- File B: Start=4, Length=2
- File C: Start=7, Length=4

Access A: Read blocks 0,1,2 sequentially (efficient)
Free space: Fragmented (block 3, blocks 5-6 = 3 total, not contiguous)
```

**Space Complexity**:
- Internal fragmentation: Often needed rounding
- External fragmentation: Severe

**Time Complexity**:
- Allocation: O(n) for finding contiguous space
- Access: O(1) + sequential read

### 2. Linked List Allocation

**Concept**:
```
File blocks linked together via pointers
Each block points to next block in file
```

**File Storage**:
- First block: Contains data + pointer to next
- Intermediate blocks: Data + pointer
- Last block: Data + NULL pointer
- Metadata: First block address only

**Algorithm**:
```
1. To store file:
   - Use any free blocks (scattered OK)
   - Link them with pointers
   - Store: first block address

2. To read file:
   - Start at first block
   - Follow pointers to traverse
   - Continue until NULL pointer
```

**Characteristics**:
- Uses scattered blocks
- No external fragmentation
- Pointer overhead per block
- Sequential access slow

**Advantages**:
✅ No external fragmentation
✅ Can use any free block
✅ Dynamic size growth easy
✅ Good space utilization

**Disadvantages**:
❌ Poor random access (must follow chain)
❌ Pointer overhead (reduces effective block size)
❌ Fragmentation of file across disk
❌ Reliability: Lost pointer = lost data

**Example**:
```
Disk Layout:

Block 0: [A data][→4]
Block 1: [B data][→5]
Block 2: Free
Block 3: Free
Block 4: [A data][→8]
Block 5: [B data][→9]
Block 6: Free
Block 7: [C data][→11]
Block 8: [A data][→NULL]
Block 9: [B data][→NULL]
Block 10: Free
Block 11: [C data][→NULL]

File A: Start=0 → 4 → 8 (3 blocks, scattered)
File B: Start=1 → 5 → 9 (3 blocks, scattered)
File C: Start=7 → 11 (2 blocks, scattered)

No external fragmentation!
All free space usable.
```

**Variations**:

**FAT (File Allocation Table)**:
- Centralized pointer table
- Links stored in separate FAT
- Used in DOS/early Windows
- Better reliability

```
FAT Entry for Block i = Next block in chain (or EOF)

File A:
- FAT[0] = 4 (block 0 points to block 4)
- FAT[4] = 8 (block 4 points to block 8)
- FAT[8] = EOF (end of file)
```

**Space Complexity**:
- Internal fragmentation: None
- External fragmentation: None
- Pointer overhead: One per block

**Time Complexity**:
- Allocation: O(free blocks)
- Access: O(blocks to read) - sequential only

### 3. Indexed Allocation

**Concept**:
```
File has index block containing all block pointers
Index block = table of all file block addresses
```

**File Storage**:
- Index block: Array of block pointers
- Data blocks: File contents at those addresses
- Metadata: Index block address
- Scattered allocation with central index

**Algorithm**:
```
1. To store file:
   - Allocate blocks for file data
   - Create index block with pointers
   - Store all pointers in index

2. To read block N of file:
   - Read index block
   - Get block address from index[N]
   - Read that block directly
```

**Characteristics**:
- Combined advantages of contiguous and linked
- Good random access
- Pointer overhead in index block
- Efficient space usage
- Most common in modern systems

**Advantages**:
✅ Efficient random access
✅ No external fragmentation
✅ Dynamic size growth
✅ All pointers in one place
✅ Better reliability than linked

**Disadvantages**:
❌ Index block size limitation
❌ Overhead for small files
❌ Pointer overhead (but centralized)
❌ Index block must exist

**Example**:
```
File A (4 blocks):

Index Block (at block 50):
[10][15][20][25]

Data Blocks (scattered):
Block 10: [A data]
Block 15: [A data]
Block 20: [A data]
Block 25: [A data]

Access Block 2 of File A:
1. Read index block 50
2. Get pointer: index[2] = 20
3. Read block 20 directly (random access)
```

**Index Variations**:

**Single-Level Index**:
- One index block
- Limited to 256-1024 blocks per file
- Small files efficient

**Multi-Level Index**:
- Index of indexes
- Supports very large files
- More overhead for small files
- Unix inodes use this

```
Example (Unix inode):
Inode contains:
- 12 direct pointers (to data blocks)
- 1 single indirect (pointer to block of pointers)
- 1 double indirect (pointer to block of pointers to pointers)
- 1 triple indirect (for very large files)
```

**Space Complexity**:
- Internal fragmentation: None
- External fragmentation: None
- Index overhead: Depends on file size

**Time Complexity**:
- Allocation: O(1) index creation
- Random access: O(1) after index read
- Sequential access: Efficient

## Comparison Table

| Aspect | Contiguous | Linked | Indexed |
|--------|-----------|--------|---------|
| Space Utilization | Poor | Good | Good |
| Sequential Access | Excellent | Fair | Good |
| Random Access | Excellent | Poor | Excellent |
| Fragmentation | External | None | None |
| Implementation | Simple | Simple | Moderate |
| File Growth | Difficult | Easy | Easy |
| Reliability | Good | Poor | Good |
| Pointer Overhead | None | High | Medium |
| Typical Use | Early OS | Older PCs | Modern OS |

## Module Structure

```
FileAllocationAlgorithms/
├── README.md          # This file
├── contiguous.html    # Contiguous allocation visualization
├── linked.html        # Linked list allocation visualization
├── indexed.html       # Indexed allocation visualization
├── css/               # Styling
└── js/                # Implementations
```

## How to Use

### Basic Workflow

1. **Select Allocation Method**:
   - Contiguous allocation
   - Linked list allocation
   - Indexed allocation

2. **Configure Parameters**:
   - Disk size (number of blocks)
   - File specifications

3. **Input File Details**:
   - File name
   - File size (in blocks)
   - Access pattern (sequential vs random)

4. **Visualize**:
   - Disk layout
   - Block allocation
   - Space usage
   - Fragmentation

5. **Analyze Results**:
   - Space efficiency
   - Access performance
   - Allocation strategy comparison

### Interactive Features

- **Disk Visualization**: Block-by-block layout
- **File Timeline**: Allocation animation
- **Statistics**: Fragmentation, utilization
- **Comparison**: Side-by-side algorithm comparison
- **Scenario Testing**: Different file patterns

## Key Metrics

### Space Utilization

```
Utilization = (Used Space) / (Total Space) × 100
```

**Good**: > 85%
**Fair**: 70-85%
**Poor**: < 70%

### Fragmentation Ratio

**External Fragmentation** (Contiguous):
```
= (Wasted Gap Space) / (Total Space) × 100
```

**Internal Fragmentation**:
```
= (Unused Space in Block) / (Block Size) × 100
```

### Access Efficiency

- **Sequential Access**: Seek + read time
- **Random Access**: Multiple seeks required

## Real-World Usage

### Contiguous Allocation
- **ISO 9660** (CD-ROMs): Contiguous for video
- **Streaming media**: Optimal for sequential access
- **Rarely used** for general file storage

### Linked Allocation
- **Original DOS FAT**: Linked blocks with FAT
- **Legacy systems**: Simple but slow
- **Still used**: USB drives, SD cards (simplified FAT32)

### Indexed Allocation
- **Unix/Linux** (Inode-based): Industry standard
- **NTFS**: Windows modern filesystem
- **HFS+**: macOS filesystem
- **Ext4, Btrfs**: Modern Linux filesystems
- **Most modern systems**: Indexed approach

## Historical Evolution

1. **Early 1970s**: Contiguous allocation (simple)
2. **1980s**: Linked allocation with FAT (DOS)
3. **1990s+**: Indexed allocation (Unix inodes)
4. **2000s+**: Advanced indexing with B-trees

## Advanced Concepts

### Allocation Strategies

**First Fit**: Allocate first available space
**Best Fit**: Allocate smallest sufficient space
**Worst Fit**: Allocate largest available space

### Free Space Management

**Bitmap**: Bit per block (0=free, 1=used)
**Free List**: List of free blocks
**Counting**: Contiguous free blocks count

### File Grows

**Contiguous**: Cannot grow (requires reallocation)
**Linked**: Easy growth (add new block)
**Indexed**: Growth limited by index block size

## Design Considerations

1. **Block Size**:
   - Larger: Better sequential, worse utilization
   - Smaller: Opposite trade-off

2. **Pointer Size**:
   - Larger: Support bigger disks
   - Smaller: Less overhead

3. **File Types**:
   - Sequential (video, audio): Prefer contiguous-like
   - Random (database): Prefer indexed

4. **Performance**:
   - Sequential workload: Contiguous best
   - Mixed workload: Indexed best
   - Random workload: Indexed best

## Practice Problems

1. Calculate fragmentation for contiguous allocation
2. Trace linked allocation block chain
3. Calculate index block requirements
4. Compare algorithms on different file sizes
5. Design optimal strategy for workload

## Educational Outcomes

Understanding this module teaches:
1. How files are stored on disk
2. Space/time trade-offs in allocation
3. Fragmentation causes and effects
4. Algorithm selection criteria
5. Modern filesystem design

## References

- Silberschatz, Galvin, Gagne - "Operating System Concepts"
- Tanenbaum, A.S. - "Modern Operating Systems"
- Filesystem documentation (ext4, NTFS, HFS+)

---

**Module Created**: 2026  
**Last Updated**: 2026
