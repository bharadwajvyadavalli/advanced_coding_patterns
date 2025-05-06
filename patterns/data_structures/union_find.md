# Union Find (Disjoint Set Union - DSU)

## Category
Advanced Data Structure

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving connected components in graphs
- Dynamic connectivity queries
- Detecting cycles in undirected graphs
- Determining if two elements are in the same set
- Finding minimum spanning trees (Kruskal's algorithm)
- Hard-level applications typically involve:
  - Handling dynamic graph modifications
  - Combining Union Find with other algorithms
  - Implementing complex operations beyond basic union and find
  - Optimizing for large numbers of operations
  - Maintaining additional component properties

## Core Idea / Intuition
Union Find (also known as Disjoint Set Union or DSU) is a data structure that tracks a partition of a set into disjoint subsets. It provides near-constant time operations for adding new sets, merging sets, and determining whether elements are in the same set. The structure uses tree-based representation with two key optimizations: "union by rank/size" and "path compression" to achieve amortized time complexity of O(α(n)) per operation, where α is the inverse Ackermann function (practically constant).

For hard problems, Union Find often needs to be enhanced with additional functionality or combined with other algorithms to efficiently solve complex graph connectivity problems.

## Step-by-Step Approach
1. **Initialize the structure**: Create a separate set for each element
2. **Implement find operation**: Find the representative (root) of a set
3. **Implement union operation**: Merge two sets by connecting their representatives
4. **Apply optimizations**: Implement path compression and union by rank/size
5. **Process operations**: Perform unions and finds according to problem requirements
6. **Extend functionality**: Add component-specific data or custom operations as needed
7. **Analyze results**: Extract information about resulting connected components

## Python-style Pseudocode Template
```python
class UnionFind:
    def __init__(self, n):
        # Initialize each element as its own parent
        self.parent = list(range(n))
        # Track size or rank of each component
        self.size = [1] * n
        # Optional: Track number of connected components
        self.count = n
    
    def find(self, x):
        # Find the root/representative of the set containing x
        # With path compression
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        # Merge the sets containing x and y
        root_x = self.find(x)
        root_y = self.find(y)
        
        # Already in the same set
        if root_x == root_y:
            return False
        
        # Union by size: attach smaller tree to root of larger tree
        if self.size[root_x] < self.size[root_y]:
            root_x, root_y = root_y, root_x
        
        # Merge root_y into root_x
        self.parent[root_y] = root_x
        self.size[root_x] += self.size[root_y]
        
        # Decrement component count
        self.count -= 1
        return True
    
    def connected(self, x, y):
        # Check if x and y are in the same set
        return self.find(x) == self.find(y)
    
    def component_size(self, x):
        # Get size of component containing x
        return self.size[self.find(x)]
    
    def get_component_count(self):
        # Get number of distinct components
        return self.count

def solve_with_union_find(n, edges):
    # Initialize Union-Find structure
    uf = UnionFind(n)
    
    # Process edges
    for u, v in edges:
        uf.union(u, v)
    
    # Extract required information based on problem
    result = extract_result(uf)
    return result

# Extended Union-Find with additional functionality
class ExtendedUnionFind(UnionFind):
    def __init__(self, n):
        super().__init__(n)
        # Additional component properties
        self.component_data = [{} for _ in range(n)]
    
    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)
        
        if root_x == root_y:
            return False
        
        # Union by size with custom data merging
        if self.size[root_x] < self.size[root_y]:
            root_x, root_y = root_y, root_x
        
        self.parent[root_y] = root_x
        self.size[root_x] += self.size[root_y]
        
        # Merge component data (problem-specific)
        self.merge_component_data(root_x, root_y)
        
        self.count -= 1
        return True
    
    def merge_component_data(self, root_x, root_y):
        # Implement problem-specific data merging
        pass
```

## Example LeetCode Hard Problems
- LC 305: Number of Islands II
- LC 765: Couples Holding Hands
- LC 803: Bricks Falling When Hit
- LC 1579: Remove Max Number of Edges to Keep Graph Fully Traversable
- LC 1627: Graph Connectivity With Threshold

## Optimization Tips
- **Path compression**: Flattening the tree structure during find operations
- **Union by rank/size**: Attaching smaller trees to larger ones to minimize tree height
- **Memory optimization**:
  - Use array-based implementation for numeric elements
  - Use sparse representation for large domains with few elements
- **Batch processing**:
  - Process operations in offline mode when possible
  - Sort operations to minimize redundant work
- **Advanced techniques**:
  - Use weighted quick union for additional performance
  - Implement incremental connectivity for dynamic problems
  - Use split operations for hard disconnection problems

## Common Pitfalls
- **Not implementing path compression** leading to linear-time operations
- **Skipping union by rank/size** resulting in unbalanced trees
- **Incorrect parent updates** during path compression
- **Off-by-one errors** when mapping problem elements to Union-Find indices
- **Not handling disconnected components** correctly
- **Inefficient component data tracking** in extended implementations
- **Redundant find operations** that could be cached
