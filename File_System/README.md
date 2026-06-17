# File System Directory Structures

Interactive visualizations of different directory structure approaches for organizing files in file systems.

## Overview

Directory structures define how file systems organize and locate files. Different approaches have trade-offs between simplicity, access speed, scalability, and user-friendliness. This module demonstrates three fundamental directory organization schemes.

## File System Fundamentals

### What is a Directory

A directory is a special file that contains:
- File names (identifiers)
- File metadata (size, permissions, timestamps)
- File locations (block addresses)
- Hierarchical organization information

### Directory Operations

- **Search**: Find file by name
- **Create**: Add new file entry
- **Delete**: Remove file entry
- **List**: Display directory contents
- **Traverse**: Navigate hierarchy

## Directory Structures

### 1. Single-Level Directory

**Concept**:
```
All files in one flat directory
No subdirectories
Files in single namespace
```

**Structure**:
```
Directory:
├── file1.txt (blocks: 10-15)
├── file2.doc (blocks: 20-25)
├── file3.exe (blocks: 30-35)
├── file4.pdf (blocks: 40-45)
└── file5.csv (blocks: 50-55)
```

**File Access**:
- Single directory lookup
- Linear search through directory
- No path needed (no hierarchy)

**Characteristics**:
- Simplest structure
- Single flat namespace
- All files at same level
- No organization possible

**Advantages**:
✅ Simple implementation
✅ Fast access (all at one level)
✅ Minimal overhead
✅ Easy to understand

**Disadvantages**:
❌ Name collisions (duplicate names)
❌ No organization
❌ Scalability poor
❌ Cannot group related files
❌ Security issues (no isolation)

**Name Conflicts**:
```
Problem: Two files named "document.txt" from different users
Solution: Globally unique names (user1_document.txt) - ugly!
```

**Example**:
```
File System:
- report.txt
- invoice.pdf
- spreadsheet.xlsx
- photo.jpg
- memo.txt

Problem: Two "memo.txt" cannot coexist
```

**Time Complexity**:
- Search: O(n) linear scan
- Create: O(1)
- Delete: O(n)

**Usage**:
- Very early operating systems (UNIX v1)
- Embedded systems with few files
- Rarely used in modern systems

### 2. Two-Level Directory

**Concept**:
```
Master File Directory (MFD) contains user directories
Each user has own User File Directory (UFD)
Files organized by user
```

**Structure**:
```
Master File Directory (MFD):
├── user1/
│   ├── file1.txt
│   ├── file2.doc
│   └── work/
│       └── project1.pdf
├── user2/
│   ├── report.xlsx
│   └── photo.jpg
└── admin/
    ├── config.sys
    └── logs.txt
```

**File Access**:
- Two-level lookup
- Path: `username/filename`
- Explicit user identification required

**Characteristics**:
- Two-level hierarchy
- One level of nesting
- Isolates users
- Limited organization

**Advantages**:
✅ Isolates users
✅ Prevents name collisions between users
✅ Simple implementation
✅ Reasonable performance
✅ Better organization than single-level
✅ Security: Users cannot access other files

**Disadvantages**:
❌ Only one level of nesting
❌ Cannot organize within user
❌ Still limited grouping
❌ Cannot share files easily between users
❌ Still relatively flat

**Example**:
```
User alice:
- Document/report.txt
- Document/data.xlsx
- Pictures/photo.jpg
- Pictures/vacation.jpg

User bob:
- Code/program.cpp
- Code/library.h
- Data/input.txt
```

**Name Collision Resolution**:
- Same filename in different users: OK
- Same filename within user: Still conflict
- Solution: Unique names within UFD

**Time Complexity**:
- Search: O(users × files_per_user) = O(n)
- Create: O(1)
- Delete: O(n)

**Historical Usage**:
- Early Unix systems
- Mainframe systems
- Simple multi-user systems

### 3. Tree-Structured Directory (Hierarchical)

**Concept**:
```
Unlimited nesting levels
Directories contain files and subdirectories
Full hierarchy possible
Root directory at top
```

**Structure**:
```
Root (/)
├── home/
│   ├── alice/
│   │   ├── Documents/
│   │   │   ├── report.txt
│   │   │   └── data.xlsx
│   │   ├── Pictures/
│   │   │   ├── photo.jpg
│   │   │   └── vacation/
│   │   │       └── beach.jpg
│   │   └── Downloads/
│   │       └── file.zip
│   └── bob/
│       └── Code/
│           └── project1/
│               ├── main.cpp
│               └── lib.h
├── var/
│   ├── log/
│   │   ├── syslog
│   │   └── auth.log
│   └── tmp/
└── usr/
    ├── bin/
    └── lib/
```

**File Access**:
- Hierarchical path lookup
- Full path: `/home/alice/Documents/report.txt`
- Absolute path (from root) or relative path (from current)
- Can traverse up and down

**Characteristics**:
- Unlimited nesting
- True hierarchy
- Full organization possible
- Modern standard

**Advantages**:
✅ Unlimited nesting and organization
✅ Logical grouping of related files
✅ Familiar to users
✅ Scalability excellent
✅ Good namespace management
✅ Supports project-based organization
✅ Security: Fine-grained permissions per directory

**Disadvantages**:
❌ More complex implementation
❌ More overhead (multiple lookups)
❌ Path name complexity
❌ Circular links possible (protection needed)
❌ Harder to understand initially

**Example Project Organization**:
```
/project/
├── src/
│   ├── main.cpp
│   ├── utils.cpp
│   └── utils.h
├── include/
│   └── utils.h
├── build/
│   ├── main.o
│   └── utils.o
├── docs/
│   ├── README.md
│   └── API.md
├── tests/
│   ├── test_utils.cpp
│   └── run_tests.sh
└── CMakeLists.txt
```

**Time Complexity**:
- Search: O(depth × files_per_directory) = O(d × f)
- Create: O(d) - path traversal
- Delete: O(d) - path traversal

**Path Resolution**:
```
Absolute Path: /home/alice/Documents/report.txt
- Start at root /
- Enter directory home
- Enter directory alice
- Enter directory Documents
- Find file report.txt

Relative Path: ../Pictures/photo.jpg
- From current directory
- Go up one level (..)
- Enter Pictures
- Find photo.jpg
```

**Usage**:
- All modern operating systems
- Linux, Windows, macOS
- Standard in all systems since 1980s+

## Directory Implementation

### Linear List
```
Simple array/list of directory entries
Each entry: (filename, metadata, block address)
Search: O(n) linear scan
```

**Directory Entry**:
```cpp
struct DirectoryEntry {
    char filename[256];
    int inode_number;
    int type; // file or directory
    int size;
    // ... more metadata
};
```

### Hash Table
```
Hash directory entries by filename
Search: O(1) average
Insert/Delete: O(1) average
More complex but faster
```

## Comparison Table

| Aspect | Single-Level | Two-Level | Tree |
|--------|-------------|-----------|------|
| Nesting | 1 | 2 | Unlimited |
| Organization | Poor | Fair | Excellent |
| Search Speed | Fast | Fast | Fair (depends on depth) |
| Scalability | Poor | Fair | Excellent |
| Implementation | Simple | Simple | Complex |
| User Isolation | No | Yes | Yes (per-directory) |
| File Organization | None | Limited | Full |
| Modern Use | Rare | Historical | Standard |

## Module Structure

```
File_System/
├── README.md                      # This file
├── index.html                     # Directory structure overview
├── single_level_directory.html    # Single-level visualization
├── two_level_directory.html       # Two-level visualization
├── tree_directory.html            # Tree structure visualization
├── css/                           # Styling
├── js/                            # Implementations
└── images/                        # Reference images
```

## How to Use

### Basic Workflow

1. **Select Directory Structure**:
   - Single-level directory
   - Two-level directory
   - Tree-structured directory

2. **Create Directory Structure**:
   - Create directories
   - Create files
   - Organize hierarchy

3. **Perform Operations**:
   - Search/find files
   - Navigate hierarchy
   - Create/delete directories
   - Create/delete files

4. **Visualize**:
   - Tree diagram
   - Directory path display
   - File listings

5. **Analyze**:
   - Search efficiency
   - Organization patterns
   - Access paths

### Interactive Features

- **Directory Creation**: Add directories
- **File Creation**: Add files to directories
- **File Search**: Find files by name or path
- **Path Display**: Show current location and path
- **Tree Visualization**: Visual representation
- **Statistics**: File/directory counts

## Key Operations

### Search
```
Scenario: Find file named "report.txt"

Single-level: Linear scan through directory
Two-level: Find user, then linear scan UFD
Tree: Traverse path, then search directory
```

### Create File
```
Scenario: Create file at /home/alice/Documents/new.txt

Path Traversal:
1. Start at root (/)
2. Find "home" directory
3. Find "alice" directory
4. Find "Documents" directory
5. Add entry for "new.txt"
```

### Delete File
```
Scenario: Delete /home/alice/Documents/old.txt

Path Traversal: Same as create
Final: Remove "old.txt" entry from Documents
```

### List Directory
```
Scenario: List contents of /home/alice/Documents/

Actions:
1. Traverse path to Documents
2. Read directory file
3. Display all entries
```

## Modern Real-World Examples

### Linux Directory Structure
```
/ (root)
├── /bin - Essential user commands
├── /etc - System configuration
├── /home - User home directories
├── /var - Variable data (logs, cache)
├── /usr - User programs and data
├── /tmp - Temporary files
└── /root - Root user home
```

### Windows Directory Structure
```
C:\
├── Users\
│   ├── Alice\
│   │   ├── Documents
│   │   ├── Downloads
│   │   ├── Pictures
│   │   └── AppData
│   └── Bob\
├── Program Files
├── Windows
└── ProgramData
```

### Project Directory
```
/MyProject/
├── /src - Source code
├── /build - Compiled output
├── /docs - Documentation
├── /tests - Test files
└── /resources - Assets
```

## Symlinks and Special Files

### Symbolic Links (Symlinks)
```
Special file containing path to another file
Not actual copy, just reference
Can point across filesystems
Can create cycles (protection needed)
```

**Example**:
```
/home/alice/current_project → /home/alice/projects/project2024

Accessing /home/alice/current_project redirects to project2024
```

### Hard Links
```
Directory entry pointing to same inode
Multiple names for same file
Cannot cross filesystems
Cannot link directories (prevents cycles)
```

## Security Considerations

### Access Control
```
Each directory can have permissions:
- Read (list contents)
- Write (create/delete files)
- Execute (enter directory)
```

### Path Validation
```
Protect against:
- Symlink traversal attacks
- Directory escape attempts (../..)
- Unauthorized access
```

## Special Directories

```
. (current directory)
.. (parent directory)
~ (home directory) - shorthand
/ (root directory)
/root (root user home)
```

## Efficiency Considerations

### Search Speed
- Single-level: O(n) - all files
- Two-level: O(1) to O(n) - per user
- Tree: O(depth × files) - by directory

### Navigation Overhead
- Single: No navigation needed
- Two: One redirect per user
- Tree: Multiple directory reads

### Caching Strategies
- Cache directory entries
- Cache inode information
- Cache path components

## Historical Evolution

1. **1960s-70s**: Single-level directories
2. **1970s-80s**: Two-level directories
3. **1980s+**: Tree-structured directories (standard)
4. **Modern**: Enhanced with permissions, links, extended attributes

## Practice Problems

1. Design directory structure for software project
2. Trace path resolution for complex paths
3. Design permissions for multi-user system
4. Identify file locations in hierarchy
5. Calculate search complexity for different structures

## Educational Outcomes

Understanding this module teaches:
1. File system organization concepts
2. Directory structure trade-offs
3. Path resolution mechanisms
4. Namespace management
5. Practical filesystem design

## References

- Silberschatz, Galvin, Gagne - "Operating System Concepts"
- Tanenbaum, A.S. - "Modern Operating Systems"
- Linux Filesystem Hierarchy Standard (FHS)
- Windows Filesystem documentation

---

**Module Created**: 2026  
**Last Updated**: 2026
