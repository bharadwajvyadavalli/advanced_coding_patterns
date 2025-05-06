# Bellman-Ford / Floyd-Warshall

## Category
Graph Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving shortest paths in weighted graphs
- When the graph may contain negative weight edges
- All-pairs shortest path problems
- Cycle detection in weighted graphs
- Hard-level applications typically involve:
  - Path reconstruction beyond just distance calculation
  - Graphs with special constraints on paths
  - Detecting negative cycles and analyzing their impact
  - Combining with other algorithms for complex path problems
  - Optimizing for special graph structures

## Core Idea / Intuition
Bellman-Ford and Floyd-Warshall are algorithms for finding shortest paths in weighted graphs, including those with negative weights:

- **Bellman-Ford** finds the shortest paths from a single source to all other vertices, handling negative weights and detecting negative cycles. It works by relaxing all edges V-1 times (where V is the number of vertices), ensuring that shortest paths with at most V-1 edges are found.

- **Floyd-Warshall** finds shortest paths between all pairs of vertices in a weighted graph. It employs dynamic programming to consider all possible intermediate vertices in paths between each pair of vertices.

For hard problems, these algorithms often need to be extended to track additional information about paths or combined with other techniques for optimal solutions.

## Step-by-Step Approach
1. **Choose the appropriate algorithm**:
   - Bellman-Ford for single-source with negative edges
   - Floyd-Warshall for all-pairs shortest paths
2. **Set up distance arrays**: Initialize distances with appropriate starting values
3. **Implement the core algorithm**: Apply edge relaxation or path consideration
4. **Detect negative cycles**: Check for further improvement after full execution
5. **Reconstruct paths** (if needed): Track predecessors for path reconstruction
6. **Apply optimizations**: Enhance performance for specific graph structures
7. **Extract results**: Process final distances and paths for problem solution

## Python-style Pseudocode Template
```python
def bellman_ford(graph, source, n):
    # Initialize distances
    dist = [float('inf')] * n
    dist[source] = 0
    
    # Optional: Track predecessors for path reconstruction
    predecessor = [None] * n
    
    # Relax all edges |V| - 1 times
    for _ in range(n - 1):
        for u, v, weight in graph:
            if dist[u] != float('inf') and dist[u] + weight < dist[v]:
                dist[v] = dist[u] + weight
                predecessor[v] = u
    
    # Check for negative weight cycles
    negative_cycle = False
    cycle_vertex = None
    
    for u, v, weight in graph:
        if dist[u] != float('inf') and dist[u] + weight < dist[v]:
            negative_cycle = True
            cycle_vertex = v
            break
    
    # Find a vertex in the negative cycle (if any)
    if negative_cycle and cycle_vertex is not None:
        # Keep track of visited vertices to find a cycle
        visited = set()
        
        # Go back n steps to ensure we're in the cycle
        for _ in range(n):
            cycle_vertex = predecessor[cycle_vertex]
        
        # Collect cycle vertices
        cycle = []
        current = cycle_vertex
        
        while current not in visited:
            visited.add(current)
            cycle.append(current)
            current = predecessor[current]
        
        # Complete the cycle
        cycle.append(current)
        
        return None, negative_cycle, cycle
    
    return dist, negative_cycle, None

def floyd_warshall(graph, n):
    # Initialize distance matrix
    dist = [[float('inf')] * n for _ in range(n)]
    
    # Optional: Track next vertex for path reconstruction
    next_vertex = [[None] * n for _ in range(n)]
    
    # Set up initial distances
    for i in range(n):
        dist[i][i] = 0
    
    # Set up direct edges
    for u, v, weight in graph:
        dist[u][v] = weight
        next_vertex[u][v] = v
    
    # Floyd-Warshall algorithm
    for k in range(n):  # Intermediate vertex
        for i in range(n):  # Source vertex
            for j in range(n):  # Destination vertex
                if dist[i][k] != float('inf') and dist[k][j] != float('inf'):
                    if dist[i][j] > dist[i][k] + dist[k][j]:
                        dist[i][j] = dist[i][k] + dist[k][j]
                        next_vertex[i][j] = next_vertex[i][k]
    
    # Check for negative cycles (if dist[i][i] < 0 for any i)
    negative_cycles = [i for i in range(n) if dist[i][i] < 0]
    
    # Path reconstruction function
    def reconstruct_path(u, v):
        if next_vertex[u][v] is None:
            return []
        
        path = [u]
        while u != v:
            u = next_vertex[u][v]
            path.append(u)
        
        return path
    
    return dist, negative_cycles, reconstruct_path

def optimized_bellman_ford(graph, source, n):
    # Initialize distances
    dist = [float('inf')] * n
    dist[source] = 0
    
    # Track if an update occurred in each iteration
    for i in range(n):
        updated = False
        
        for u, v, weight in graph:
            if dist[u] != float('inf') and dist[u] + weight < dist[v]:
                dist[v] = dist[u] + weight
                updated = True
        
        # Early termination if no updates in this round
        if not updated:
            break
        
        # After n-1 iterations, any further updates indicate negative cycle
        if i == n - 1 and updated:
            return None, True  # Negative cycle exists
    
    return dist, False
```

## Example LeetCode Hard Problems
- LC 787: Cheapest Flights Within K Stops
- LC 1334: Find the City With the Smallest Number of Neighbors at a Threshold Distance
- LC 1368: Minimum Cost to Make at Least One Valid Path in a Grid
- LC 1462: Course Schedule IV
- LC 2203: Minimum Weighted Subgraph With the Required Paths

## Optimization Tips
- **Early termination**:
  - Stop Bellman-Ford if no updates occur in an iteration
  - Track affected vertices to only process necessary edges
- **Memory optimization**:
  - Use sparse representations for large graphs
  - Reuse distance arrays for Floyd-Warshall when possible
- **Algorithm selection**:
  - Use Johnson's algorithm for sparse graphs (combines Bellman-Ford and Dijkstra)
  - Apply SPFA (Shortest Path Faster Algorithm) for average case performance improvement
- **Problem-specific optimizations**:
  - Exploit graph structure (e.g., DAGs allow simpler algorithms)
  - Apply divide-and-conquer techniques for large graphs
  - Use bidirectional search when applicable

## Common Pitfalls
- **Handling infinity** values incorrectly in arithmetic operations
- **Not detecting negative cycles** when required by the problem
- **Incorrect path reconstruction** due to improper predecessor tracking
- **Memory limit exceeded** for large graphs with Floyd-Warshall (O(V³) space)
- **Time limit exceeded** due to inefficient implementation or unnecessary iterations
- **Not recognizing when simpler algorithms** are sufficient (e.g., using Dijkstra when no negative edges exist)
- **Incorrect initialization** of distance matrices or arrays
