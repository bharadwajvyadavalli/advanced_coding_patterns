# Meet in the Middle

## Category
Special Technique

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems with exponential search space that can be divided into two smaller subproblems
- When brute force approach is too slow but the problem size is still moderately small
- Optimization problems where the search space needs to be reduced
- When naively exploring all possibilities exceeds time limits
- Hard-level applications typically involve:
  - Computing all possible subsets or combinations
  - Finding special sums or patterns within arrays
  - Optimizing discrete parameters within constraints
  - Dividing and conquering NP-hard problems with small inputs
  - Combining with other techniques for efficient solutions

## Core Idea / Intuition
Meet in the Middle is a technique that splits the problem into two roughly equal parts, solves each part independently, and then combines the results. This reduces the time complexity from O(2^n) to O(2^(n/2)), making previously intractable problems solvable.

The key insight is that exploring all possibilities in two halves and then combining the results is much faster than exploring the entire search space at once, especially when there's a way to efficiently match the results from both halves.

For hard problems, this technique often needs to be combined with efficient data structures and algorithms to handle the matching phase optimally.

## Step-by-Step Approach
1. **Divide the problem**: Split the input into two roughly equal parts
2. **Compute all possibilities**: Generate all possible states/results for each half
3. **Process and store**: Organize the results from the first half for efficient lookup
4. **Match with second half**: Find combinations that satisfy the problem constraints
5. **Combine results**: Merge the partial solutions to get the final answer
6. **Optimize matching phase**: Use appropriate data structures (hash tables, binary search, etc.)
7. **Handle edge cases**: Consider special cases and boundary conditions

## Python-style Pseudocode Template
```python
def meet_in_the_middle(arr, target):
    n = len(arr)
    
    # Divide the array into two halves
    mid = n // 2
    first_half = arr[:mid]
    second_half = arr[mid:]
    
    # Generate all possible subset sums for the first half
    first_sums = []
    generate_subset_sums(first_half, 0, 0, first_sums)
    
    # Sort for binary search or use a hash map for O(1) lookup
    first_sums.sort()
    
    # Generate and match subset sums for the second half
    result = 0  # or other initial value based on problem
    generate_and_match(second_half, 0, 0, target, first_sums, result)
    
    return result

def generate_subset_sums(arr, index, current_sum, sums):
    # Base case: all elements processed
    if index == len(arr):
        sums.append(current_sum)
        return
    
    # Include current element
    generate_subset_sums(arr, index + 1, current_sum + arr[index], sums)
    
    # Exclude current element
    generate_subset_sums(arr, index + 1, current_sum, sums)

def generate_and_match(arr, index, current_sum, target, first_sums, result):
    # Base case: all elements processed
    if index == len(arr):
        # Find matching sum from first half
        # Binary search approach:
        complement = target - current_sum
        count = count_occurrences(first_sums, complement)
        result += count
        return
    
    # Include current element
    generate_and_match(arr, index + 1, current_sum + arr[index], target, first_sums, result)
    
    # Exclude current element
    generate_and_match(arr, index + 1, current_sum, target, first_sums, result)

def count_occurrences(sorted_array, target):
    # Binary search to find first and last occurrence
    left = bisect_left(sorted_array, target)
    right = bisect_right(sorted_array, target)
    return right - left

# Example: Count subsets with sum equal to target
def count_subsets_with_sum(arr, target):
    n = len(arr)
    mid = n // 2
    
    # Generate all subset sums for first half
    first_half = arr[:mid]
    first_sums = {}  # Using dictionary for frequency counting
    
    for mask in range(1 << len(first_half)):
        subset_sum = 0
        for i in range(len(first_half)):
            if mask & (1 << i):
                subset_sum += first_half[i]
        
        first_sums[subset_sum] = first_sums.get(subset_sum, 0) + 1
    
    # Generate and match from second half
    second_half = arr[mid:]
    count = 0
    
    for mask in range(1 << len(second_half)):
        subset_sum = 0
        for i in range(len(second_half)):
            if mask & (1 << i):
                subset_sum += second_half[i]
        
        complement = target - subset_sum
        if complement in first_sums:
            count += first_sums[complement]
    
    return count

# Example: Find closest subset sum to target
def closest_subset_sum(arr, target):
    n = len(arr)
    mid = n // 2
    
    # Generate all subset sums for first half
    first_half = arr[:mid]
    first_sums = []
    
    for mask in range(1 << len(first_half)):
        subset_sum = 0
        for i in range(len(first_half)):
            if mask & (1 << i):
                subset_sum += first_half[i]
        first_sums.append(subset_sum)
    
    first_sums.sort()
    
    # Generate subset sums for second half and find closest match
    second_half = arr[mid:]
    closest = float('inf')
    
    for mask in range(1 << len(second_half)):
        subset_sum = 0
        for i in range(len(second_half)):
            if mask & (1 << i):
                subset_sum += second_half[i]
        
        # Find closest value in first_sums to (target - subset_sum)
        complement = target - subset_sum
        
        # Binary search for closest element
        idx = bisect_left(first_sums, complement)
        
        if idx > 0:
            closest = min(closest, abs(target - (subset_sum + first_sums[idx-1])))
        
        if idx < len(first_sums):
            closest = min(closest, abs(target - (subset_sum + first_sums[idx])))
    
    return target - closest  # Return the closest sum
```

## Example LeetCode Hard Problems
- LC 805: Split Array With Same Average
- LC 956: Tallest Billboard
- LC 1755: Closest Subsequence Sum
- LC 1982: Find Array Given Subset Sums
- LC 2035: Partition Array Into Two Arrays to Minimize Sum Difference

## Optimization Tips
- **Efficient data structures**:
  - Use hash maps for O(1) lookups when exact matches are needed
  - Use sorted arrays with binary search for range queries
  - Consider using bitsets for compact representation of subsets
- **Pruning techniques**:
  - Apply bounds to skip unpromising combinations
  - Use problem constraints to reduce search space
  - Implement early termination when possible
- **Memory optimizations**:
  - Store only necessary information
  - Use compression when dealing with large states
  - Consider trading time for space if memory is a constraint
- **Parallelization**:
  - Compute the two halves in parallel if possible
  - Distribute the matching phase across multiple processes

## Common Pitfalls
- **Unbalanced division** of the problem leading to suboptimal performance
- **Memory limit exceeded** due to storing too many states
- **Incorrect handling of duplicates** in the input
- **Inefficient matching phase** that negates the advantage of splitting
- **Overlooking edge cases** (empty sets, single element, etc.)
- **Not considering all constraints** during the matching phase
- **Using suboptimal data structures** for lookup or matching operations
