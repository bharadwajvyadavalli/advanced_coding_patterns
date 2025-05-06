# LeetCode Hard Pattern Templates

A comprehensive collection of 40 advanced algorithm patterns optimized for solving LeetCode Hard problems.

## Overview

This repository contains detailed template implementations and explanations for all major algorithmic patterns that appear in challenging technical interviews and competitive programming. Each pattern is documented with a consistent structure including core intuition, step-by-step approach, pseudocode template, optimization tips, and common pitfalls.

These templates are specifically designed for tackling **LeetCode Hard** difficulty problems, focusing on the advanced applications and edge cases of each pattern.

## How to Use This Repository

1. **Learn a Pattern**: Study the explanation, core intuition, and step-by-step approach
2. **Understand the Implementation**: Review the pseudocode template to grasp the pattern's implementation
3. **Apply to Problems**: Practice with the example LeetCode Hard problems listed for each pattern
4. **Optimize Your Solution**: Use the optimization tips to improve your approach
5. **Avoid Common Mistakes**: Be aware of the common pitfalls associated with each pattern

## Pattern Categories

### Core Patterns
- [Sliding Window](./patterns/core/sliding_window.md) - Find optimal subarrays or substrings using a dynamic window
- [Two Pointers](./patterns/core/two_pointers.md) - Process arrays or linked lists with coordinated pointers
- [Fast and Slow Pointers](./patterns/core/fast_slow_pointers.md) - Detect cycles and find middle elements
- [Binary Search](./patterns/core/binary_search.md) - Efficiently find elements in sorted data or search answer spaces
- [Depth-First Search (DFS)](./patterns/core/dfs.md) - Traverse deeply into data structures before backtracking
- [Breadth-First Search (BFS)](./patterns/core/bfs.md) - Explore all neighbors at current depth before moving deeper
- [Backtracking](./patterns/core/backtracking.md) - Build solutions incrementally and abandon unpromising paths
- [Dynamic Programming (DP)](./patterns/core/dynamic_programming.md) - Solve complex problems by breaking them into simpler subproblems
- [Greedy Algorithms](./patterns/core/greedy.md) - Make locally optimal choices to find global optimum

### Dynamic Programming Specializations
- [1D Dynamic Programming](./patterns/core/dp_1d.md) - Linear state problems like subsequences and optimization
- [2D Dynamic Programming](./patterns/core/dp_2d.md) - Matrix operations and two-sequence comparisons
- [Knapsack Dynamic Programming](./patterns/core/dp_knapsack.md) - Resource allocation and subset selection
- [Interval Dynamic Programming](./patterns/core/dp_intervals.md) - Optimal processing of ranges and segments
- [State Compression DP](./patterns/core/dp_state_compression.md) - Using bits to represent complex states

### Advanced Data Structures
- [Heap / Priority Queue](./patterns/data_structures/heap_priority_queue.md) - Efficiently maintain the maximum/minimum element
- [Trie (Prefix Tree)](./patterns/data_structures/trie.md) - Optimize string operations and prefix matching
- [Union Find (Disjoint Set)](./patterns/data_structures/union_find.md) - Track and merge disjoint sets efficiently
- [Segment Tree / Fenwick Tree](./patterns/data_structures/segment_tree.md) - Handle range queries and updates in logarithmic time
- [Monotonic Stack / Queue](./patterns/data_structures/monotonic_stack_queue.md) - Maintain elements in strictly increasing/decreasing order

### Math & Bit Manipulation
- [Bit Manipulation](./patterns/math_bit/bit_manipulation.md) - Use bitwise operations for efficient algorithms
- [Mathematics](./patterns/math_bit/mathematics.md) - Apply number theory, combinatorics, and modular arithmetic
- [Prefix Sum / Difference Array](./patterns/math_bit/prefix_sum.md) - Optimize range sum queries and updates

### Graph Patterns
- [Topological Sort](./patterns/graph/topological_sort.md) - Order nodes in a directed acyclic graph
- [Dijkstra's Algorithm](./patterns/graph/dijkstra.md) - Find shortest paths in weighted graphs
- [Bellman-Ford / Floyd-Warshall](./patterns/graph/bellman_ford_floyd_warshall.md) - Handle graphs with negative weights
- [Minimum Spanning Tree](./patterns/graph/minimum_spanning_tree.md) - Find the minimum-weight tree connecting all vertices

### String Patterns
- [KMP Algorithm](./patterns/string/kmp.md) - Efficiently search for patterns in strings
- [Rabin-Karp Algorithm](./patterns/string/rabin_karp.md) - Use rolling hash for string matching
- [Z-Algorithm](./patterns/string/z_algorithm.md) - Linear-time pattern matching and string analysis
- [Manacher's Algorithm](./patterns/string/manachers.md) - Find all palindromic substrings in linear time
- [Rolling Hash](./patterns/string/rolling_hash.md) - Efficiently hash sliding windows of data

### Special Techniques
- [Top-K Elements](./patterns/special/top_k_elements.md) - Find/maintain k largest/smallest elements efficiently
- [Meet in the Middle](./patterns/special/meet_in_the_middle.md) - Split problems in half to reduce exponential complexity
- [Recursion with Memoization](./patterns/special/recursion_memoization.md) - Top-down dynamic programming with caching
- [State Compression](./patterns/special/state_compression.md) - Use bits to represent states in dynamic programming
- [Sweep Line Algorithm](./patterns/special/sweep_line.md) - Process intervals or geometric problems efficiently
- [Reservoir Sampling](./patterns/special/reservoir_sampling.md) - Sample elements from streams of unknown size
- [Binary Search on Answer](./patterns/special/binary_search_answer.md) - Apply binary search to find optimal values
- [Tree Traversals & BST](./patterns/special/tree_traversals.md) - Advanced tree manipulation techniques
- [Linked List Manipulation](./patterns/special/linked_list.md) - Complex pointer operations and list transformations
- [Matrix Traversal / Flood Fill](./patterns/special/matrix_traversal.md) - Process 2D grids and connected components

## Template Structure

Each pattern template follows a consistent structure:

1. **Pattern Name and Category**
2. **Difficulty Level** - Focused on LeetCode Hard
3. **When to Use** - Guidelines for recognizing when the pattern applies
4. **Core Idea / Intuition** - Fundamental concepts behind the pattern
5. **Step-by-Step Approach** - Detailed implementation strategy
6. **Python-style Pseudocode Template** - Ready-to-adapt implementation
7. **Example LeetCode Hard Problems** - Specific problems that use this pattern
8. **Optimization Tips** - Techniques to improve time/space complexity
9. **Common Pitfalls** - Mistakes to avoid when applying the pattern

## Getting Started

1. Clone this repository:
   ```
   git clone https://github.com/your-username/leetcode-hard-pattern-templates.git
   ```

2. Browse to the pattern category you're interested in learning

3. Start with core patterns before moving to more specialized techniques

4. Practice by applying these patterns to actual LeetCode problems

## Dynamic Programming Guide

Dynamic Programming is one of the most challenging algorithm paradigms to master. We've expanded our coverage with specialized templates:

1. **General DP Overview** - Fundamental concepts and approaches
2. **1D Dynamic Programming** - Linear state dependencies like subsequence problems
3. **2D Dynamic Programming** - Matrix operations and pairwise comparisons
4. **Knapsack Dynamic Programming** - Resource allocation optimization
5. **Interval Dynamic Programming** - Optimal range processing techniques
6. **State Compression DP** - Using bit manipulation for complex states

Each DP template provides detailed examples, intuitions, and optimizations specific to that subtype.

## Best Practices for Problem Solving

1. **Identify the Pattern**: Learn to recognize which pattern applies to a problem
2. **Adapt the Template**: Use the provided pseudocode as a starting point
3. **Optimize Incrementally**: Start with a working solution, then optimize
4. **Test Edge Cases**: Pay special attention to the common pitfalls section
5. **Combine Patterns**: Many hard problems require combining multiple patterns

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request to:

- Add new patterns
- Improve existing explanations
- Add more example problems
- Fix bugs or optimize implementations

See [CONTRIBUTING.md](./CONTRIBUTING.md) for more details.

## License

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.
