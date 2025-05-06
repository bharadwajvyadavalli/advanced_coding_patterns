# Dijkstra's Algorithm

## Category
Graph Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Finding the shortest path from a source node to all other nodes in a weighted graph
- When the graph has non-negative edge weights only
- When you need the exact minimum distance (not just any path)
- Problems involving optimal routing or navigation
- Hard-level applications typically involve:
  - Multiple constraints or conditions on valid paths
  - Dynamic graph modifications during traversal
  - Multi-stage path finding with changing criteria
  - Large graphs requiring optimized implementations
  - Path reconstruction, not just distance calculation

## Core Idea / Intuition
Dijkstra's algorithm is a greedy algorithm that finds the shortest path from a source node to all other nodes in a weighted graph. It works by maintaining a priority queue of vertices, always selecting the vertex with the minimum distance next, and relaxing all its adjacent edges. This guarantees that once a vertex is processed, we've found the shortest path to it.

For hard problems, we often need to augment the basic algorithm with additional state tracking, custom priority functions, or integrate it with other algorithms to solve complex graph problems efficiently.

## Step-by-Step Approach
1. **Initialize distances**: Set distance to source as 0, all others as infinity
2. **Create priority queue**: Add source vertex with distance 0
3. **Process vertices**: Remove vertex with minimum distance from priority queue
4. **Relax edges**: Update distances to adjacent vertices if a shorter path is found
5. **Add updated vertices**: Put vertices with updated distances back in the queue
6. **Repeat**: Continue until priority queue is empty or target is reached
7. **Optional: Reconstruct path**: Use parent pointers to rebuild the shortest path

## Python-style Pseudocode Template
```python
def dijkstra(graph, source, target=None):
    # Step 1: Initialize distances and parent pointers
    distances = {vertex: float('infinity') for vertex in graph}
    distances[source] = 0
    parents = {vertex: None for vertex in graph}
    
    # Step 2: Initialize priority queue
    priority_queue = [(0, source)]  # (distance, vertex)
    
    # Step 3: Process vertices
    while priority_queue:
        current_distance, current_vertex = heappop(priority_queue)
        
        # Skip if we've found a better path already
        if current_distance > distances[current_vertex]:
            continue
        
        # Early termination if target is reached
        if target and current_vertex == target:
            break
        
        # Step 4 & 5: Relax edges and update queue
        for neighbor, weight in graph[current_vertex].items():
            distance = current_distance + weight
            
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                parents[neighbor] = current_vertex
                heappush(priority_queue, (distance, neighbor))
    
    # Step 7: Reconstruct path if target is specified
    if target:
        path = []
        current = target
        while current:
            path.append(current)
            current = parents[current]
        return distances[target], path[::-1]
    
    return distances, parents
```

## Example LeetCode Hard Problems
- LC 743: Network Delay Time
- LC 882: Reachable Nodes In Subdivided Graph
- LC 1631: Path With Minimum Effort
- LC 1786: Number of Restricted Paths From First to Last Node
- LC 2093: Minimum Cost to Reach City With Discounts

## Optimization Tips
- **Efficient priority queue implementation**:
  - Use binary heap for general cases (O(log n) operations)
  - Consider Fibonacci heap for large sparse graphs (O(1) decrease-key)
  - Use buckets/counting sort for integer weights with small range
- **Early termination**:
  - Stop when target is reached if only single-target path is needed
  - Use bidirectional search for single-target problems
- **State reduction**:
  - Prune vertices that cannot lead to better solutions
  - Use heuristics (A* algorithm) when additional information is available
- **Preprocessing**:
  - Compute distances between important vertices offline
  - Use contraction hierarchies for large static graphs

## Common Pitfalls
- **Using with negative weights** (Dijkstra's doesn't work with negative edges; use Bellman-Ford instead)
- **Incorrect priority queue updates** leading to duplicate vertices
- **Inefficient adjacency list representation** causing slow edge traversal
- **Not handling disconnected graphs** properly
- **Incorrect termination conditions** leading to incomplete solutions
- **Memory overflow** for very large graphs (consider chunking or specialized implementations)
- **Not distinguishing between no path** and infinite distance cases
