# Segment Tree / Binary Indexed Tree (Fenwick Tree)

## Category
Advanced Data Structure

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving range queries (sum, min, max, etc.) with element updates
- When you need efficient operations on array ranges (intervals)
- Dynamic data scenarios requiring both queries and modifications
- Problems with multiple query types on the same data structure
- Hard-level applications typically involve:
  - 2D or higher dimensional range queries
  - Persistent segment trees for version history
  - Lazy propagation for range updates
  - Complex query operations beyond basic sum/min/max
  - Combining with other data structures

## Core Idea / Intuition
Segment Trees and Binary Indexed Trees (Fenwick Trees) are specialized data structures that efficiently manage range queries and updates on an array. They both achieve O(log n) time complexity for these operations, though they differ in implementation details and flexibility.

Segment Trees store values in a tree structure where each node represents a range of the array. This provides greater flexibility for different query types but requires more memory. Binary Indexed Trees use a clever binary representation to efficiently handle cumulative operations (primarily sums) with less memory but are less versatile.

For hard problems, these structures often need to be extended with additional capabilities like lazy propagation, 2D implementations, or combined with other algorithms.

## Step-by-Step Approach
1. **Choose appropriate structure**: Decide between Segment Tree or BIT based on problem needs
2. **Define operations**: Determine what operations (sum, min, max, etc.) the tree will support
3. **Implement tree construction**: Build the initial tree from the input array
4. **Implement query operation**: Create efficient way to query ranges
5. **Implement update operation**: Allow modifications to individual elements or ranges
6. **Add optimizations**: Implement lazy propagation or other enhancements if needed
7. **Process operations**: Perform queries and updates according to problem requirements

## Python-style Pseudocode Template
```python
# Segment Tree implementation
class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        # Size of segment tree is approximately 4 * n
        self.tree = [0] * (4 * self.n)
        self.lazy = [0] * (4 * self.n)  # For lazy propagation
        self.build(arr, 0, 0, self.n - 1)
    
    def build(self, arr, node, start, end):
        # Build the segment tree recursively
        if start == end:
            # Leaf node
            self.tree[node] = arr[start]
            return
        
        mid = (start + end) // 2
        # Build left subtree
        self.build(arr, 2 * node + 1, start, mid)
        # Build right subtree
        self.build(arr, 2 * node + 2, mid + 1, end)
        
        # Internal node value based on operation (sum, min, max, etc.)
        self.tree[node] = self.operation(
            self.tree[2 * node + 1],
            self.tree[2 * node + 2]
        )
    
    def operation(self, a, b):
        # Define the operation (sum, min, max, etc.)
        return a + b  # Example: sum operation
    
    def update_lazy(self, node, start, end):
        # Push lazy updates down the tree
        if self.lazy[node] != 0:
            # Update current node
            self.tree[node] += self.lazy[node] * (end - start + 1)  # For sum
            
            if start != end:
                # Propagate to children
                self.lazy[2 * node + 1] += self.lazy[node]
                self.lazy[2 * node + 2] += self.lazy[node]
            
            # Reset lazy value
            self.lazy[node] = 0
    
    def query(self, node, start, end, left, right):
        # Apply lazy updates
        self.update_lazy(node, start, end)
        
        # No overlap
        if start > right or end < left:
            return 0  # Identity element for the operation
        
        # Complete overlap
        if start >= left and end <= right:
            return self.tree[node]
        
        # Partial overlap - query both children
        mid = (start + end) // 2
        left_result = self.query(2 * node + 1, start, mid, left, right)
        right_result = self.query(2 * node + 2, mid + 1, end, left, right)
        
        return self.operation(left_result, right_result)
    
    def update_range(self, node, start, end, left, right, val):
        # Apply lazy updates
        self.update_lazy(node, start, end)
        
        # No overlap
        if start > right or end < left:
            return
        
        # Complete overlap
        if start >= left and end <= right:
            # Update current node
            self.tree[node] += val * (end - start + 1)  # For sum
            
            if start != end:
                # Mark children for lazy update
                self.lazy[2 * node + 1] += val
                self.lazy[2 * node + 2] += val
            
            return
        
        # Partial overlap - update both children
        mid = (start + end) // 2
        self.update_range(2 * node + 1, start, mid, left, right, val)
        self.update_range(2 * node + 2, mid + 1, end, left, right, val)
        
        # Update current node
        self.tree[node] = self.operation(
            self.tree[2 * node + 1],
            self.tree[2 * node + 2]
        )

# Binary Indexed Tree (Fenwick Tree) implementation
class BinaryIndexedTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.bit = [0] * (self.n + 1)  # 1-indexed
        
        # Build BIT
        for i in range(len(arr)):
            self.update(i, arr[i])
    
    def update(self, index, val):
        # Convert to 1-indexed
        index += 1
        
        # Update all responsible ranges
        while index <= self.n:
            self.bit[index] += val
            # Move to the next responsible index
            index += index & -index  # Add least significant bit
    
    def prefix_sum(self, index):
        # Get sum from 0 to index
        # Convert to 1-indexed
        index += 1
        
        result = 0
        while index > 0:
            result += self.bit[index]
            # Move to the parent
            index -= index & -index  # Remove least significant bit
        
        return result
    
    def range_sum(self, left, right):
        # Get sum from left to right (inclusive)
        return self.prefix_sum(right) - self.prefix_sum(left - 1)
```

## Example LeetCode Hard Problems
- LC 307: Range Sum Query - Mutable (Segment Tree/BIT)
- LC 308: Range Sum Query 2D - Mutable (2D Segment Tree/BIT)
- LC 315: Count of Smaller Numbers After Self
- LC 493: Reverse Pairs
- LC 1649: Create Sorted Array through Instructions

## Optimization Tips
- **Memory optimization**:
  - Segment Tree: Optimize array size, especially for sparse data
  - BIT: Use compressed representation for sparse ranges
- **Lazy propagation** for Segment Trees:
  - Essential for efficient range updates
  - Delay updates until nodes are actually needed
- **Multi-dimensional optimization**:
  - Use compressed coordinate mapping for sparse 2D grids
  - Use BIT for 2D queries when possible (simpler implementation)
- **Query optimization**:
  - Cache frequent queries
  - Optimize operation order for complex queries
- **Special techniques**:
  - Merge sort tree for complex order statistics queries
  - Persistent segment tree for historical queries

## Common Pitfalls
- **Off-by-one errors** with zero vs. one indexing (particularly with BIT)
- **Incorrect tree size allocation** leading to array index out of bounds
- **Missing lazy propagation** for range updates
- **Inefficient operation implementation** for complex queries
- **Not handling edge cases** properly (empty ranges, single elements)
- **Incorrect identity element** for different operations
- **Memory limit exceeded** for large arrays or multiple trees
