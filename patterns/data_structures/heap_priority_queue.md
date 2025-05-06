# Heap / Priority Queue

## Category
Advanced Data Structure

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems requiring frequent access to the maximum or minimum element
- When you need to efficiently find the k-th largest/smallest element
- In graph algorithms like Dijkstra's or Prim's
- For scheduling or interval-based problems
- As part of merge operations in divide and conquer algorithms
- Hard-level applications typically involve:
  - Multi-dimensional priority or complex ordering
  - Dynamic updates to priorities during processing
  - Custom comparators for complex objects
  - Merging multiple heaps efficiently
  - Optimizing time complexity with heaps when other approaches would be O(n²)

## Core Idea / Intuition
A Heap or Priority Queue is a specialized tree-based data structure that satisfies the heap property: in a max heap, for any given node, the value of the node is greater than or equal to the values of its children. In a min heap, the value of the node is less than or equal to the values of its children. This structure allows O(1) access to the maximum/minimum element and O(log n) insertion and deletion operations.

For hard problems, the key insight is often how to leverage the heap to efficiently maintain a dynamic order of elements based on complex criteria or combined with other algorithms for optimal solutions.

## Step-by-Step Approach
1. **Identify the priority metric**: Determine what values should determine element priority
2. **Choose heap type**: Decide between min-heap, max-heap, or custom heap
3. **Define element structure**: Determine what data needs to be stored with each element
4. **Implement operations**: Add, remove, and update elements with proper priority
5. **Process elements**: Extract elements in priority order and process them
6. **Handle dynamic updates**: Update priorities when necessary based on new information
7. **Optimize heap operations**: Minimize the number of operations for performance

## Python-style Pseudocode Template
```python
import heapq  # Python's built-in min-heap

def heap_solution(items):
    # Min-heap example (for max-heap, negate values)
    heap = []
    
    # Insert items into heap with O(log n) per item
    for item in items:
        priority = calculate_priority(item)
        heapq.heappush(heap, (priority, item))
    
    # Process items in priority order
    result = []
    while heap:
        priority, item = heapq.heappop(heap)
        
        # Process item
        processed_item = process_item(item)
        result.append(processed_item)
        
        # Optionally push new items based on the processed item
        new_items = generate_new_items(item)
        for new_item in new_items:
            new_priority = calculate_priority(new_item)
            heapq.heappush(heap, (new_priority, new_item))
    
    return result

def k_elements_solution(items, k):
    # Find k smallest/largest elements
    
    # For k smallest elements
    heap = []
    for item in items:
        heapq.heappush(heap, item)
        if len(heap) > k:
            heapq.heappop(heap)  # Remove largest element if heap exceeds k
    
    return heap  # Contains k smallest elements

def custom_heap_solution(items):
    # For custom objects or complex priority
    class HeapItem:
        def __init__(self, item):
            self.item = item
            self.primary = calculate_primary_priority(item)
            self.secondary = calculate_secondary_priority(item)
        
        def __lt__(self, other):
            # Custom comparison for multi-criteria priority
            if self.primary != other.primary:
                return self.primary < other.primary
            return self.secondary < other.secondary
    
    # Create heap with custom objects
    heap = [HeapItem(item) for item in items]
    heapq.heapify(heap)
    
    # Process heap
    result = []
    while heap:
        item = heapq.heappop(heap).item
        result.append(process_item(item))
    
    return result
```

## Example LeetCode Hard Problems
- LC 23: Merge k Sorted Lists
- LC 295: Find Median from Data Stream
- LC 407: Trapping Rain Water II
- LC 632: Smallest Range Covering Elements from K Lists
- LC 1675: Minimize Deviation in Array

## Optimization Tips
- **Heap creation efficiency**:
  - Use `heapify` (O(n)) instead of n insertions (O(n log n)) when possible
  - Pre-allocate heap capacity for performance in languages that support it
- **Avoid repeated operations**:
  - Batch updates when possible
  - Use lazy deletion (mark as deleted) instead of immediate removal
  - Use decrease-key operation when available
- **Memory efficiency**:
  - Store only necessary information in the heap
  - Use primitive types when possible
  - Remove unneeded elements
- **Custom heap implementations**:
  - Fibonacci heap for theoretical O(1) decrease-key operations
  - Pairing heap for practical efficiency
  - D-ary heap for faster insertions with infrequent extractions

## Common Pitfalls
- **Incorrect heap choice** (min vs. max) for the problem
- **Not handling duplicate priorities** properly
- **Inefficient custom comparators** leading to inconsistent behavior
- **Not tracking element indices** when updates are needed
- **Redundant heap operations** that could be optimized
- **Memory overflow** for large datasets
- **Not leveraging heap properties** fully to optimize the solution
