# Depth-First Search (DFS)

## Category
Core Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Graph or tree traversal problems where you need to explore as far as possible along each branch
- Problems involving path finding, connectivity, or component analysis
- When searching for specific paths or configurations
- When generating all possible solutions (similar to backtracking)
- Hard-level applications typically involve:
  - Complex state representations
  - Pruning strategies to reduce search space
  - Combining DFS with dynamic programming (memoization)
  - Multi-constraint path finding
  - Complex graph structures (directed, weighted, or cyclical)

## Core Idea / Intuition
Depth-First Search explores a path to its deepest point before backtracking and exploring alternative paths. It prioritizes depth over breadth, making it well-suited for problems where complete paths need to be explored or when the solution is likely to be found deep in the search tree.

For hard problems, DFS is often augmented with memoization, pruning techniques, or combined with other algorithms to efficiently handle complex state spaces and reduce redundant computations.

## Step-by-Step Approach
1. **Define the state representation**: Determine what constitutes a "node" in your search space
2. **Define goal and base conditions**: Establish when to stop the search (success or failure)
3. **Define transition rules**: Determine how to move from one state to adjacent states
4. **Implement DFS traversal**: Use recursion or an explicit stack for traversal
5. **Track visited states**: Avoid revisiting states to prevent cycles or redundant work
6. **Optimize search space**: Apply pruning techniques to eliminate unpromising paths early
7. **Handle result collection**: Gather and process results as needed during traversal

## Python-style Pseudocode Template
```python
def dfs(graph, start, visited=None):
    # Recursive DFS implementation
    if visited is None:
        visited = set()
    
    # Mark current node as visited
    visited.add(start)
    
    # Process the current node
    process_node(start)
    
    # Recursively visit all unvisited neighbors
    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
    
    return visited

def dfs_iterative(graph, start):
    # Iterative DFS implementation using a stack
    visited = set()
    stack = [start]
    
    while stack:
        vertex = stack.pop()
        
        if vertex not in visited:
            # Mark current node as visited
            visited.add(vertex)
            
            # Process the current node
            process_node(vertex)
            
            # Add unvisited neighbors to stack
            # (add in reverse order to maintain the same traversal as recursive)
            for neighbor in reversed(graph[vertex]):
                if neighbor not in visited:
                    stack.append(neighbor)
    
    return visited

def dfs_with_state(state, visited=None, memo=None):
    # DFS with complex state and memoization for hard problems
    if visited is None:
        visited = set()
    if memo is None:
        memo = {}
    
    # Return cached result if state already processed
    if state in memo:
        return memo[state]
    
    # Check if current state is a goal state
    if is_goal_state(state):
        return goal_value
    
    # Mark current state as visited
    visited.add(state)
    
    result = initial_value  # Depends on problem (min/max/count/etc)
    
    # Generate and explore all possible next states
    for next_state in generate_next_states(state):
        if next_state not in visited:
            # Recursively explore next state
            sub_result = dfs_with_state(next_state, visited, memo)
            
            # Update result based on problem requirements
            result = update_result(result, sub_result)
    
    # Cache and return result
    memo[state] = result
    return result
```

## Example LeetCode Hard Problems
- LC 301: Remove Invalid Parentheses
- LC 329: Longest Increasing Path in a Matrix
- LC 332: Reconstruct Itinerary
- LC 980: Unique Paths III
- LC 1559: Detect Cycles in 2D Grid

## Optimization Tips
- **Memoization** (top-down DP):
  - Cache results for previously visited states
  - Especially useful when states can be reached through multiple paths
- **State pruning**:
  - Use bounds checking to eliminate branches that can't lead to optimal solutions
  - Implement early stopping when conditions can't be satisfied
- **State representation**:
  - Use compact representations like tuples, bitmasks, or hashes for states
  - Design state transitions to minimize redundant states
- **Optimized data structures**:
  - Use sets or hash maps for O(1) visited checks
  - Use adjacency lists for sparse graphs
- **Iterative vs. recursive implementation**:
  - Use iterative implementation for deep graphs to avoid stack overflow
  - Use recursive for cleaner code when depth is controlled

## Common Pitfalls
- **Stack overflow** in recursive implementations for deep graphs
- **Memory exhaustion** when tracking too many visited states
- **Infinite loops** if not properly tracking visited nodes in cyclic graphs
- **Redundant state exploration** without proper memoization
- **Incorrect neighborhood generation** leading to missing valid paths
- **Excessive pruning** that eliminates valid solutions
- **Inefficient state representation** leading to slow comparisons or excessive memory use
