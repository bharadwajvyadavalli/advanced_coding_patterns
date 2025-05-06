# Binary Search

## Category
Core Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving sorted arrays, lists, or matrices
- When searching for a specific value or position that satisfies certain conditions
- Problems asking for the kth element with specific properties
- When optimization or minimization/maximization is required within a defined range
- Hard-level applications typically involve:
  - Multi-dimensional binary search
  - Binary search on answer space rather than indices
  - Binary search with complex predicates or conditions
  - Combined with other algorithms (e.g., DP, greedy)

## Core Idea / Intuition
Binary Search reduces the search space by half in each step, achieving O(log n) time complexity instead of O(n) linear search. It works by repeatedly dividing the search interval in half and determining which half contains the target value based on comparison.

For hard problems, binary search is often applied to the answer space itself rather than indices, especially in optimization problems where we need to find the minimum or maximum value that satisfies certain conditions.

## Step-by-Step Approach
1. **Define search space**: Identify the search range (left and right boundaries)
2. **Define search condition**: Create a clear predicate function that determines which half to search
3. **Execute binary search**: Repeatedly divide search space in half until convergence
4. **Handle edge cases**: Consider empty arrays, single elements, duplicates
5. **Post-processing**: Verify the final result meets all required conditions
6. **Binary search on answer**: For optimization problems, define search space as possible answer values

## Python-style Pseudocode Template
```python
def binary_search(nums, target):
    # Standard binary search on sorted array
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = left + (right - left) // 2  # Avoids integer overflow
        
        if nums[mid] == target:
            return mid  # Found target
        elif nums[mid] < target:
            left = mid + 1  # Target in right half
        else:
            right = mid - 1  # Target in left half
    
    return -1  # Target not found

def binary_search_on_answer(possible_answers, condition):
    # Binary search on answer space for optimization problems
    left, right = min(possible_answers), max(possible_answers)
    
    # For minimization problems
    result = right  # Initialize with maximum value
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if condition(mid):
            # This value works, but keep searching for better (smaller) values
            result = mid
            right = mid - 1
        else:
            # This value doesn't work, search in larger values
            left = mid + 1
    
    return result

def condition(value):
    # Problem-specific condition that returns True if value satisfies requirements
    # For example: Can we finish all tasks within 'value' time?
    # Or: Can we place k elements with minimum distance 'value'?
    pass
```

## Example LeetCode Hard Problems
- LC 4: Median of Two Sorted Arrays
- LC 410: Split Array Largest Sum
- LC 1044: Longest Duplicate Substring
- LC 1235: Maximum Profit in Job Scheduling
- LC 1631: Path With Minimum Effort

## Optimization Tips
- **Choosing the right search boundaries**:
  - For standard binary search: use array indices
  - For answer space: use the minimum and maximum possible answer values
- **Predicate function optimization**:
  - Design efficient condition functions, as they're called O(log n) times
  - Precompute values when possible to speed up condition evaluation
- **Edge case handling**:
  - Handle empty arrays, single elements
  - Consider inclusive vs. exclusive bounds carefully
- **Advanced techniques**:
  - Rotated array binary search for partially sorted arrays
  - Matrix binary search for 2D arrays
  - Parallel binary search for multiple targets

## Common Pitfalls
- **Off-by-one errors** in boundary conditions (left <= right vs. left < right)
- **Integer overflow** when calculating mid-point (use left + (right - left) // 2)
- **Infinite loops** due to incorrect pointer updates
- **Imprecise condition functions** leading to incorrect results
- **Not handling duplicates** when they affect the condition
- **Incorrectly defining search space** for binary search on answer
- **Failing to verify final result** with all constraints
