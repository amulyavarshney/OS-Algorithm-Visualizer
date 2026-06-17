# OS-Algorithm-Visualizer

An interactive web-based visualization platform for learning Operating System algorithms and concepts. This project provides intuitive, interactive demonstrations of critical OS concepts including process scheduling, synchronization, memory management, and disk I/O operations.

## Overview

This educational platform helps students and developers understand complex Operating System algorithms through visual, interactive simulations. Each module allows users to input parameters, run algorithms, and observe real-time visualizations of execution.

## Key Features

- **Interactive Visualizations**: Real-time algorithm execution with visual feedback
- **Educational Content**: Integrated wiki/reference pages for each algorithm
- **Modular Design**: Independent modules for different OS concepts
- **User-Friendly Interface**: Bootstrap-based responsive design
- **Multiple Algorithms**: Each category includes multiple algorithm variations

## Modules

The project contains 8 major modules covering essential OS concepts:

### 1. [Process Scheduling](./Process%20Scheduling/README.md)
CPU scheduling algorithms that determine process execution order.
- First Come First Serve (FCFS)
- Shortest Job First (SJF)
- Round Robin (RR)
- Priority Scheduling
- Shortest Remaining Time First (SRTF)
- Multilevel Queue Scheduling

### 2. [Process Synchronization](./Process%20Synchronization/README.md)
Classic synchronization problems demonstrating inter-process coordination.
- Producer-Consumer Problem
- Reader-Writer Problem
- Dining Philosophers Problem
- Cigarette Smokers Problem
- Sleeping Barber Problem

### 3. [Banker's Algorithm](./Bankers/README.md)
Resource allocation and deadlock avoidance using the Banker's safety algorithm.
- Interactive resource allocation simulation
- Safety state checking
- Deadlock avoidance visualization

### 4. [Memory Management](./MFT/README.md)
Memory allocation strategies and management techniques.
- First Fit allocation
- Best Fit allocation
- Worst Fit allocation
- Next Fit allocation

### 5. [Page Replacement](./page%20replacement/README.md)
Virtual memory page replacement algorithms for efficient memory utilization.
- First In First Out (FIFO)
- Least Recently Used (LRU)
- Most Recently Used (MRU)
- Most Frequently Used (MFU)
- Second Chance (Clock) Algorithm
- Optimal Page Replacement (OPR)
- Comparative analysis tool

### 6. [Disk Scheduling](./Disk/README.md)
Disk I/O scheduling algorithms for optimizing access patterns.
- First Come First Serve (FCFS)
- Shortest Seek Time First (SSTF)
- SCAN Algorithm
- Circular SCAN (C-SCAN)
- LOOK Algorithm
- C-LOOK Algorithm

### 7. [File Allocation](./FileAllocationAlgorithms/README.md)
Strategies for allocating disk space to files.
- Contiguous Allocation
- Linked List Allocation
- Indexed Allocation

### 8. [File System Management](./File_System/README.md)
Directory structure organization and file system hierarchy.
- Single-Level Directory
- Two-Level Directory
- Tree-Structured Directory

## Technology Stack

- **Frontend**: HTML5, CSS3, Bootstrap 4.5
- **Visualization**: Plotly.js (for graphs and charts)
- **Scripting**: Vanilla JavaScript
- **Libraries**: jQuery, Font Awesome Icons
- **Fonts**: Google Fonts (Lato, Catamaran, Muli)

## Project Structure

```
OS-Algorithm-Visualizer/
├── index.html                    # Main landing page
├── app.css                        # Global styles
├── Process Scheduling/            # CPU scheduling algorithms
│   ├── src/                      # Individual algorithm pages
│   ├── js/                       # Algorithm implementations
│   └── css/                      # Module styling
├── Process Synchronization/       # Synchronization problems
├── Bankers/                      # Banker's algorithm
├── MFT/                          # Memory management
├── page replacement/             # Page replacement algorithms
├── Disk/                         # Disk scheduling
├── FileAllocationAlgorithms/     # File allocation strategies
├── File_System/                  # File system structures
├── assets/                       # Static assets
│   ├── bootstrap/               # Bootstrap framework
│   ├── fonts/                   # Custom fonts
│   ├── img/                     # Images
│   └── js/                      # Global scripts
├── css/                          # Additional stylesheets
├── scss/                         # SCSS source files
├── vendor/                       # External libraries
└── LICENSE                       # Project license
```

## Usage

### Getting Started

1. Open `index.html` in a modern web browser
2. Use the navigation dropdown to select an algorithm module
3. Navigate to the specific algorithm you want to visualize
4. Input parameters as required by the algorithm
5. Click "Run" or "Execute" to visualize the algorithm
6. Review results and educational content

### Example Workflows

**Process Scheduling Visualization:**
1. Navigate to Process Scheduling from main menu
2. Select an algorithm (e.g., FCFS)
3. Enter process details (burst times, arrival times)
4. Execute to see scheduling timeline

**Page Replacement Comparison:**
1. Go to Page Replacement module
2. Input page reference sequence
3. Compare multiple algorithms side-by-side
4. Analyze page fault statistics

## Browser Compatibility

- Chrome/Chromium (recommended)
- Firefox
- Safari
- Edge
- Any modern browser supporting ES6+ JavaScript

## Navigation

- **Main Page**: `index.html` - Central hub with algorithm categories
- **Module Pages**: Each module has its own directory with specific algorithms
- **Wiki Pages**: Educational references for each algorithm (typically `wiki.html` files)

## Educational Value

This visualizer serves as:
- **Learning Tool**: Understand algorithm behavior through visualization
- **Study Guide**: Integrated educational content for each concept
- **Teaching Aid**: Demonstrate algorithms in real-time to students
- **Reference**: Quick access to algorithm implementations and variations

## Notes

- Each module operates independently and can be accessed directly
- Algorithms accept interactive input for custom scenarios
- Visual outputs help identify algorithm differences and trade-offs
- Educational wiki pages provide theoretical background

## Future Enhancements

Potential areas for expansion:
- Real-time performance metrics and statistics
- Algorithm comparison views
- Customizable color schemes and themes
- Export visualization results
- Mobile-responsive improvements
- Additional OS algorithms (scheduling variants, synchronization solutions)
- Performance benchmarking tools

## License

See [LICENSE](./LICENSE) file for details.

## Contact

For questions or feedback, visit the [GitHub profile](https://github.com/amulyavarshney).

## References

Algorithms and concepts are based on standard Operating System textbooks and research, particularly:
- Silberschatz, Galvin, Gagne - Operating System Concepts
- Wikipedia articles on OS algorithms
- Academic CS255+ course materials

---

**Last Updated**: 2026
