# Topological Sort

## Category
Graph Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving directed acyclic graphs (DAGs)
- Dependency resolution and scheduling problems
- Task sequencing with prerequisites
- Determining valid ordering based on constraints
- Hard-level applications typically involve:
  - Multiple valid orderings with specific properties
  - Finding lexicographically smallest valid ordering
  - Cycle detection and handling in directed graphs
  - Combining with other algorithms (DP, DFS, BFS)
  - Constraints on the ordering beyond simple dependencies

## Core Idea / Intuition
Topological Sort produces a linear ordering of vertices in a directed acyclic graph (DAG) such that for every directed edge (u, v), vertex u comes before vertex v in the ordering. The core insight is that a DAG must have at least one vertex with no incoming edges (a source) and removing this vertex still results in a DAG, allowing for recursive application of this principle.

For hard problems, topological sorting often needs to be extended with additional processing to handle complex constraints or combined with other algorithms to solve multi-dimensional problems.

## Step-by-Step Approach
1. **Create graph representation**: Build adjacency list or matrix from edges
2. **Calculate in-degree**: Count incoming edges for each vertex
3. **Initialize source queue**: Add vertices with zero in-degree
4. **Process sources**: Remove vertices and update in-degrees
5. **Check for cycles**: Ensure all vertices are processed
6. **Apply additional constraints**: Handle problem-specific requirements
7. **Return result**: Provide the final topological ordering

## Python-style Pseudocode Template
```python
from collections import defaultdict, deque

def topological_sort(n, edges):
    # Create adjacency list
    graph = defaultdict(list)
    in_degree = [0] * n
    
    # Build the graph
    for u, v in edges:
        graph[u].append(v)
        in_degree[v] += 1
    
    # Initialize queue with vertices having no dependencies
    queue = deque()
    for i in range(n):
        if in_degree[i] == 0:
            queue.append(i)
    
    # Process vertices in topological order
    result = []
    while queue:
        # For lexicographically smallest ordering (if needed)
        # Use a min-heap or sort the queue
        
        # Process current vertex
        vertex = queue.popleft()
        result.append(vertex)
        
        # Update dependencies
        for neighbor in graph[vertex]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    
    # Check if graph has a cycle
    if len(result) != n:
        return None  # Graph has a cycle
    
    return result

def dfs_topological_sort(n, edges):
    # Alternative DFS-based implementation
    graph = defaultdict(list)
    
    # Build the graph
    for u, v in edges:
        graph[u].append(v)
    
    # Track visited and being-visited vertices
    visited = [0] * n  # 0: unvisited, 1: being visited, 2: visited
    
    result = []
    
    def dfs(node):
        # Return True if a cycle is detected
        if visited[node] == 1:
            return True  # Back edge = cycle
        if visited[node] == 2:
            return False  # Already processed
        
        visited[node] = 1  # Mark as being visited
        
        # Visit neighbors
        for neighbor in graph[node]:
            if dfs(neighbor):
                return True
        
        visited[node] = 2  # Mark as fully visited
        result.append(node)
        return False
    
    # Visit all vertices
    for i in range(n):
        if visited[i] == 0:
            if dfs(i):
                return None  # Graph has a cycle
    
    # Reverse for correct topological order
    return result[::-1]

def lexicographically_smallest_topo_sort(n, edges):
    # Create adjacency list
    graph = defaultdict(list)
    in_degree = [0] * n
    
    # Build the graph
    for u, v in edges:
        graph[u].append(v)
        in_degree[v] += 1
    
    # Initialize min-heap with vertices having no dependencies
    # Use min-heap for lexicographically smallest ordering
    min_heap = []
    for i in range(n):
        if in_degree[i] == 0:
            heapq.heappush(min_heap, i)
    
    # Process vertices in topological order
    result = []
    while min_heap:
        # Get smallest vertex with no dependencies
        vertex = heapq.heappop(min_heap)
        result.append(vertex)
        
        # Update dependencies
        for neighbor in sorted(graph[vertex]):  # Sort for deterministic processing
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                heapq.heappush(min_heap, neighbor)
    
    # Check if graph has a cycle
    if len(result) != n:
        return None  # Graph has a cycle
    
    return result
```

## Example LeetCode Hard Problems
- LC 269: Alien Dictionary
- LC 329: Longest Increasing Path in a Matrix
- LC 1203: Sort Items by Groups Respecting Dependencies
- LC 1591: Strange Printer II
- LC 2392: Build a Matrix With Conditions

## Optimization Tips
- **Graph representation**:
  - Use adjacency lists for sparse graphs
  - Use bit manipulation for small graphs to reduce memory
- **Queue optimizations**:
  - Use a min-heap for lexicographically smallest ordering
  - Use a max-heap for reverse ordering
  - Use multiple queues for prioritizing different vertex types
- **Algorithm selection**:
  - Use Kahn's algorithm (BFS-based) for explicit cycle detection
  - Use DFS for implicit cycle detection and when post-ordering is beneficial
- **Problem-specific optimizations**:
  - Prune unnecessary edges
  - Group vertices with similar properties
  - Combine with dynamic programming for optimization problems

## Common Pitfalls
- **Not handling cycles** properly in directed graphs
- **Incorrect graph construction** from problem constraints
- **Inefficient data structures** for specific problem requirements
- **Missing edge cases** (disconnected components, single-node cycles)
- **Incorrect ordering logic** for lexicographical constraints
- **Not tracking all dependencies** from the problem description
- **Confusion between 0-indexed and 1-indexed vertex labels**
