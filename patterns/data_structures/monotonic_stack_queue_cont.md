# Monotonic Stack / Monotonic Queue

## Category
Advanced Data Structure

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving finding next/previous greater/smaller elements
- Problems requiring maintaining a window with minimum/maximum property
- Optimization problems involving element comparisons in a specific order
- History-dependent problems where past elements affect current decisions
- Hard-level applications typically involve:
  - Complex monotonicity conditions
  - Multi-pass algorithms using monotonic structures
  - Combining monotonic structures with other algorithms
  - Circular array processing
  - Non-obvious monotonicity properties

## Core Idea / Intuition
A Monotonic Stack/Queue maintains elements in a strictly increasing or decreasing order. This property enables efficient solutions to problems involving comparisons between elements, especially when seeking the next or previous greater/smaller element.

The key insight is that when pushing a new element, we can remove elements that would never be used in future comparisons, thereby maintaining the monotonic property and reducing the time complexity from potentially O(n²) to O(n).

For hard problems, monotonic structures often require careful analysis to identify the appropriate monotonicity direction and may need to be combined with other techniques for optimal solutions.

## Step-by-Step Approach
1. **Identify monotonicity direction**: Determine whether increasing or decreasing order is needed
2. **Define element representation**: Decide what information to store with each element
3. **Implement push operation**: Add new elements while maintaining monotonicity
4. **Process elements**: Handle elements as they enter/leave the structure
5. **Maintain auxiliary information**: Track additional data if needed
6. **Combine with other techniques**: Integrate with other algorithms if necessary
7. **Extract results**: Derive the final solution from the processed data

## Python-style Pseudocode Template
```python
def monotonic_stack_solution(arr):
    # Initialize a monotonic stack (increasing or decreasing)
    stack = []  # Stack elements can be tuples (value, index, other_info)
    result = [None] * len(arr)
    
    # Process elements in appropriate order
    for i in range(len(arr)):
        # For a monotonic increasing stack (next smaller)
        # Remove elements that violate monotonicity
        while stack and stack[-1][0] > arr[i]:
            value, index, *other = stack.pop()
            # Process the popped element
            result[index] = process_element(value, arr[i], i - index)
        
        # Push current element
        stack.append((arr[i], i))
    
    # Process remaining elements in stack (if needed)
    while stack:
        value, index, *other = stack.pop()
        result[index] = process_remaining(value, index)
    
    return result

def monotonic_queue_solution(arr, k):
    # Monotonic queue for sliding window problems
    queue = deque()  # Store indices for window boundaries
    result = []
    
    for i in range(len(arr)):
        # Remove elements outside the current window
        while queue and queue[0] < i - k + 1:
            queue.popleft()
        
        # For a monotonic decreasing queue (window maximum)
        # Remove smaller elements that won't be maximum in future
        while queue and arr[queue[-1]] < arr[i]:
            queue.pop()
        
        # Add current element
        queue.append(i)
        
        # The front of queue is the maximum element in current window
        if i >= k - 1:
            result.append(arr[queue[0]])
    
    return result

def circular_monotonic_stack(arr):
    # Process circular array using monotonic stack
    n = len(arr)
    result = [-1] * n  # Default: no next greater element
    stack = []
    
    # Process array twice to handle circular nature
    for i in range(n * 2):
        # Get actual index for circular array
        real_index = i % n
        
        # For next greater element
        while stack and arr[stack[-1]] < arr[real_index]:
            prev_index = stack.pop()
            result[prev_index] = arr[real_index]
        
        # Only push new elements in the first iteration
        if i < n:
            stack.append(real_index)
    
    return result
```

## Example LeetCode Hard Problems
- LC 84: Largest Rectangle in Histogram
- LC 85: Maximal Rectangle
- LC 239: Sliding Window Maximum
- LC 907: Sum of Subarray Minimums
- LC 1505: Minimum Possible Integer After at Most K Adjacent Swaps On Digits

## Optimization Tips
- **Efficient element representation**:
  - Store only necessary information (value, index, etc.)
  - Use primitive types when possible
- **Processing optimizations**:
  - Single-pass algorithms when possible
  - Combine forward and backward passes for bidirectional problems
  - Use early termination when possible
- **Data structure optimizations**:
  - Use deque for monotonic queue implementation
  - Preallocate result arrays for performance
- **Problem-specific optimizations**:
  - Identify and exploit symmetry in the problem
  - Precompute values when helpful
  - Combine with dynamic programming for complex state transitions

## Common Pitfalls
- **Incorrect monotonicity direction** (increasing vs. decreasing)
- **Not handling boundary cases** properly (empty stack, stack with one element)
- **Missing edge cases** in array (duplicates, negative values, zeros)
- **Incorrect handling of remaining stack elements** after processing the array
- **Inefficient tuple packing/unpacking** in performance-critical sections
- **Not recognizing when a monotonic structure is applicable**
- **Overcomplicating the solution** with unnecessary information in the stack/queue
