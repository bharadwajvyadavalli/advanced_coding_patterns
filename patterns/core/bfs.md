# Breadth-First Search (BFS)

## Category
Core Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Finding the shortest path in unweighted graphs or grids
- Level-order traversal of trees or graphs
- Problems involving the minimum number of steps or operations
- When all neighbors should be explored before moving deeper
- Hard-level applications typically involve:
  - Multi-source BFS (starting from multiple points)
  - BFS with state or multi-dimensional visited tracking
  - BFS with priority queues (technically Dijkstra's algorithm)
  - Complex state transitions or transformations
  - Bidirectional BFS for performance optimization

## Core Idea / Intuition
Breadth-First Search explores all nodes at the current depth level before moving to nodes at the next depth level. It uses a queue data structure to keep track of nodes to visit, ensuring that nodes are processed in order of their distance from the starting point.

For hard problems, BFS is often combined with complex state representations, multiple queues, or bidirectional search to efficiently solve problems with large search spaces or complex constraints.

## Step-by-Step Approach
1. **Define the state representation**: Determine what constitutes a "node" in your search space
2. **Initialize queue and visited set**: Start with the initial state(s)
3. **Process levels**: Explore all nodes at current level before moving to next level
4. **Track depth/distance**: Keep track of level number or distance from source
5. **Define goal condition**: Establish when to return the result
6. **Handle result collection**: Process results as nodes are dequeued
7. **Optimize search space**: Apply techniques to reduce state space or improve performance

## Python-style Pseudocode Template
```python
from collections import deque

def bfs(graph, start):
    # Standard BFS implementation
    queue = deque([start])
    visited = {start}  # Use set for O(1) lookups
    level = 0
    
    while queue:
        # Process all nodes at current level
        level_size = len(queue)
        
        for _ in range(level_size):
            node = queue.popleft()
            
            # Process the current node
            process_node(node, level)
            
            # Check if goal is reached
            if is_goal(node):
                return construct_result(node, level)
            
            # Add unvisited neighbors to queue
            for neighbor in graph[node]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)
        
        # Move to next level
        level += 1
    
    return no_solution_found_value

def multi_source_bfs(graph, sources):
    # BFS starting from multiple sources simultaneously
    queue = deque([(source, 0) for source in sources])  # (node, distance)
    visited = set(sources)
    
    while queue:
        node, distance = queue.popleft()
        
        # Process current node
        process_node(node, distance)
        
        # Add unvisited neighbors
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, distance + 1))
    
    return result

def bidirectional_bfs(graph, start, target):
    # Bidirectional BFS - search from both start and target
    if start == target:
        return 0
    
    # Forward and backward queues
    forward_queue = deque([start])
    backward_queue = deque([target])
    
    # Forward and backward visited sets with distances
    forward_visited = {start: 0}
    backward_visited = {target: 0}
    
    while forward_queue and backward_queue:
        # Expand from the smaller frontier for efficiency
        if len(forward_queue) <= len(backward_queue):
            distance = expand_frontier(graph, forward_queue, forward_visited, backward_visited)
        else:
            distance = expand_frontier(graph, backward_queue, backward_visited, forward_visited)
        
        if distance is not None:
            return distance
    
    return -1  # No path found

def expand_frontier(graph, queue, visited, other_visited):
    node = queue.popleft()
    distance = visited[node]
    
    for neighbor in graph[node]:
        if neighbor in visited:
            continue
        
        visited[neighbor] = distance + 1
        queue.append(neighbor)
        
        # Check if the frontiers meet
        if neighbor in other_visited:
            return distance + 1 + other_visited[neighbor]
    
    return None
```

## Example LeetCode Hard Problems
- LC 127: Word Ladder
- LC 317: Shortest Distance from All Buildings
- LC 864: Shortest Path to Get All Keys
- LC 1293: Shortest Path in a Grid with Obstacles Elimination
- LC 2092: Find All People With Secret

## Optimization Tips
- **Queue optimization**:
  - Use deque for efficient O(1) append and popleft operations
  - Use priority queue when states have different "costs" (becomes Dijkstra's algorithm)
- **Visited state optimization**:
  - Use hashmaps or sets for O(1) lookups
  - For grids, use 2D arrays for faster access than dictionaries
  - Use tuples or bit manipulation for compact state representation
- **Search space reduction**:
  - Use bidirectional BFS to substantially reduce search space
  - Prune states that can't lead to valid solutions
  - Use A* search (BFS + heuristic) when additional information is available
- **Memory optimization**:
  - Store only necessary information in visited set
  - Reuse visited set as distance map

## Common Pitfalls
- **Not tracking visited nodes** leading to infinite loops in graphs with cycles
- **Inefficient state representation** causing slow comparisons or excessive memory
- **Queue overflow** for problems with vast state spaces
- **Forgetting level tracking** when level information is needed
- **Using BFS when DFS is more appropriate** (e.g., for finding any valid path rather than shortest)
- **Not handling edge cases** (empty graph, disconnected components, etc.)
- **Incorrect neighbor generation** leading to invalid state transitions
