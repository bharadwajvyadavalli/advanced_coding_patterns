# Sliding Window

## Category
Core Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving arrays or strings where you need to find a subarray/substring that meets certain criteria
- Input constraints that involve contiguous sequences
- When asked to find maximum, minimum, or optimal subarray/substring with specific properties
- Problems that involve a "window" of elements that grows or shrinks
- Hard-level constraints typically involve:
  - Variable-sized windows rather than fixed-size
  - Multiple criteria that must be satisfied simultaneously
  - Nested conditions within the window

## Core Idea / Intuition
The Sliding Window pattern is a technique that transforms a complex subarray problem into a matter of maintaining and updating relevant information about a "window" of elements. Instead of repeatedly calculating the same values for overlapping subarrays (which leads to O(n²) or worse), the sliding window approach achieves O(n) by intelligently maintaining state as the window expands and contracts.

For hard problems, we typically implement a variable-sized window that grows and shrinks based on complex conditions, often tracking multiple variables simultaneously.

## Step-by-Step Approach
1. **Identify window boundaries**: Define two pointers (left and right) that represent the current window
2. **Initialize state variables**: Set up counters, maps, or data structures to track required conditions
3. **Expand window**: Move right pointer to include new elements until window violates constraints
4. **Contract window**: Move left pointer to exclude elements until window satisfies constraints again
5. **Update result**: Record optimal results during expansion/contraction
6. **Repeat**: Continue process until right pointer reaches end of input

## Python-style Pseudocode Template
```python
def sliding_window(arr):
    # Step 1: Initialize state variables
    left = 0
    result = initial_value  # Depends on problem (min/max/count)
    state = {}  # Map, counter, or complex data structure to track window state
    
    # Step 2: Iterate with right pointer
    for right in range(len(arr)):
        # Step 3: Include element at right pointer (expand window)
        update_state_on_expansion(state, arr[right])
        
        # Step 4: Contract window if needed
        while window_needs_contraction(state):
            update_state_on_contraction(state, arr[left])
            left += 1
        
        # Step 5: Update result if current window is valid
        if is_valid_window(state):
            result = update_result(result, right - left + 1, state)
    
    return result
```

## Example LeetCode Hard Problems
- LC 76: Minimum Window Substring
- LC 992: Subarrays with K Different Integers
- LC 1425: Constrained Subsequence Sum
- LC 2106: Maximum Fruits Harvested After at Most K Steps
- LC 2281: Sum of Total Strength of Wizards

## Optimization Tips
- **Use appropriate data structures**: 
  - HashMap/Counter for frequency tracking
  - Deque for sliding max/min problems
  - Heap for finding k-th elements within window
- **Avoid redundant computations**:
  - Incrementally update rather than recalculating from scratch
  - Use monotonic data structures when finding min/max in window
- **Precomputing**:
  - Consider precomputing frequency arrays or prefix sums
  - For string problems, convert to integer representations when possible

## Common Pitfalls
- **Off-by-one errors** when updating window boundaries
- **Improper state initialization** leading to incorrect initial windows
- **Inefficient window contraction** (contracting one element at a time when multiple could be removed)
- **Failing to handle empty result** cases (when no valid window exists)
- **Incorrect state updates** during expansion/contraction
- **Redundant validations** inside the loop that could be simplified
- **Not recognizing when Two Pointers is actually more appropriate** than Sliding Window
