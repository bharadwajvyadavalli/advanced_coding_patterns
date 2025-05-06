# Minimum Spanning Tree (Kruskal, Prim)

## Category
Graph Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems requiring connecting all vertices with minimum total edge weight
- Network design with minimum cost connections
- Clustering problems with distance-based metrics
- Problems involving finding critical connections
- Hard-level applications typically involve:
  - Constrained spanning trees (bottleneck, degree-constrained)
  - Multiple minimum spanning trees with specific properties
  - Dynamic MST with changing graph structure
  - Combining MST with other algorithms for optimization
  - MST variants (maximum spanning tree, k-MST)

## Core Idea / Intuition
A Minimum Spanning Tree (MST) is a subset of edges from a connected, undirected graph that connects all vertices with the minimum possible total edge weight, without forming cycles. The two primary algorithms for finding MSTs are:

- **Kruskal's Algorithm**: Sorts all edges by weight and greedily adds edges that don't create cycles, using Union-Find to detect cycles.
- **Prim's Algorithm**: Builds the tree incrementally, starting from a vertex and adding the lowest-weight edge to a vertex not yet in the tree.

For hard problems, MST algorithms often need to be modified to handle additional constraints or combined with other techniques for optimal solutions to complex network problems.

## Step-by-Step Approach
1. **Choose the appropriate algorithm**:
   - Kruskal for sparse graphs
   - Prim for dense graphs
2. **Prepare data structures**: Set up Union-Find (Kruskal) or priority queue (Prim)
3. **Sort edges** (Kruskal) or **initialize source** (Prim)
4. **Build MST**: Add edges according to algorithm rules
5. **Track additional properties**: Maintain problem-specific information
6. **Handle special constraints**: Apply problem-specific modifications
7. **Extract results**: Process the final MST for problem solution

## Python-style Pseudocode Template
```python
# Kruskal's Algorithm
def kruskal_mst(n, edges):
    # Initialize Union-Find data structure
    parent = list(range(n))
    rank = [0] * n
    
    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])  # Path compression
        return parent[x]
    
    def union(x, y):
        root_x = find(x)
        root_y = find(y)
        
        if root_x == root_y:
            return False
        
        # Union by rank
        if rank[root_x] < rank[root_y]:
            parent[root_x] = root_y
        elif rank[root_x] > rank[root_y]:
            parent[root_y] = root_x
        else:
            parent[root_y] = root_x
            rank[root_x] += 1
        
        return True
    
    # Sort edges by weight
    edges.sort(key=lambda x: x[2])
    
    mst_edges = []
    mst_weight = 0
    
    # Process edges in order of increasing weight
    for u, v, weight in edges:
        if find(u) != find(v):  # If adding this edge doesn't create a cycle
            union(u, v)
            mst_edges.append((u, v, weight))
            mst_weight += weight
            
            # Early termination if MST is complete
            if len(mst_edges) == n - 1:
                break
    
    # Check if MST spans the entire graph
    if len(mst_edges) != n - 1:
        return None, float('inf')  # Graph is not connected
    
    return mst_edges, mst_weight

# Prim's Algorithm
def prim_mst(n, edges):
    # Build adjacency list
    graph = [[] for _ in range(n)]
    for u, v, weight in edges:
        graph[u].append((v, weight))
        graph[v].append((u, weight))  # For undirected graph
    
    # Track included vertices
    in_mst = [False] * n
    
    # Min-heap for edge selection
    min_heap = [(0, 0, -1)]  # (weight, vertex, parent)
    
    mst_edges = []
    mst_weight = 0
    
    while min_heap and len(mst_edges) < n - 1:
        weight, vertex, parent = heapq.heappop(min_heap)
        
        if in_mst[vertex]:
            continue
        
        in_mst[vertex] = True
        
        if parent != -1:
            mst_edges.append((parent, vertex, weight))
            mst_weight += weight
        
        # Add all edges from current vertex to the heap
        for neighbor, edge_weight in graph[vertex]:
            if not in_mst[neighbor]:
                heapq.heappush(min_heap, (edge_weight, neighbor, vertex))
    
    # Check if MST spans the entire graph
    if len(mst_edges) != n - 1:
        return None, float('inf')  # Graph is not connected
    
    return mst_edges, mst_weight

# Maximum Spanning Tree (variant)
def maximum_spanning_tree(n, edges):
    # Negate weights and use Kruskal's or Prim's algorithm
    negated_edges = [(u, v, -weight) for u, v, weight in edges]
    mst_edges, mst_weight = kruskal_mst(n, negated_edges)
    
    if mst_edges is None:
        return None, float('-inf')
    
    # Restore original weights
    original_edges = [(u, v, -weight) for u, v, weight in mst_edges]
    original_weight = -mst_weight
    
    return original_edges, original_weight

# Bottleneck Spanning Tree (minimize the maximum edge weight)
def bottleneck_spanning_tree(n, edges):
    # Sort edges by weight
    edges.sort(key=lambda x: x[2])
    
    # Binary search on the maximum edge weight
    def can_build_spanning_tree(max_weight):
        # Use only edges with weight <= max_weight
        valid_edges = [(u, v, w) for u, v, w in edges if w <= max_weight]
        uf = UnionFind(n)
        
        for u, v, _ in valid_edges:
            uf.union(u, v)
        
        # Check if all vertices are connected
        return uf.count == 1
    
    left, right = 0, max(edge[2] for edge in edges)
    result = right
    
    while left <= right:
        mid = (left + right) // 2
        
        if can_build_spanning_tree(mid):
            result = mid
            right = mid - 1
        else:
            left = mid + 1
    
    return result
```

## Example LeetCode Hard Problems
- LC 1135: Connecting Cities With Minimum Cost
- LC 1168: Optimize Water Distribution in a Village
- LC 1489: Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree
- LC 1584: Min Cost to Connect All Points
- LC 1631: Path With Minimum Effort

## Optimization Tips
- **Algorithm selection**:
  - Use Kruskal for sparse graphs (E << V²)
  - Use Prim for dense graphs (E ≈ V²)
  - Consider Borůvka's algorithm for parallel implementations
- **Data structure optimizations**:
  - Implement efficient Union-Find with path compression and union by rank
  - Use Fibonacci heap for Prim's algorithm for theoretical O(E + V log V)
  - Apply indexed priority queue for faster decrease-key operations
- **Problem-specific optimizations**:
  - Apply binary search for bottleneck or threshold problems
  - Use incremental updates for dynamic MST problems
  - Exploit graph structure for faster processing
- **Early termination**:
  - Stop when n-1 edges are collected
  - Terminate when impossible conditions are detected

## Common Pitfalls
- **Incorrect edge representation** for undirected graphs (forgetting to add both directions)
- **Improper cycle detection** in Kruskal's algorithm
- **Inefficient priority queue operations** in Prim's algorithm
- **Not handling disconnected graphs** appropriately
- **Mishandling equal-weight edges** when specific ordering is required
- **Overlooking problem constraints** that affect MST construction
- **Memory limit exceeded** for large dense graphs
