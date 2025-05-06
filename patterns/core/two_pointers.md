# Two Pointers

## Category
Core Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving sorted arrays, strings, or linked lists
- When asked to find pairs, triplets, or subarrays that satisfy certain conditions
- When searching for a pair of indices/elements with specified relationship
- Problems that require traversing from both ends of a data structure
- Hard-level applications typically involve:
  - Multi-dimensional arrays or complex data structures
  - Multiple conditions that must be satisfied simultaneously
  - Non-trivial pointer movement logic
  - Combining with other algorithms (binary search, hash tables)

## Core Idea / Intuition
The Two Pointers pattern uses two pointer variables to traverse a data structure, typically moving toward or away from each other. This approach transforms what would be O(n²) nested loops into O(n) linear solutions by leveraging sorted arrays or other ordering properties.

In hard problems, we often need sophisticated pointer movement logic, multiple pointers, or integration with additional data structures to track complex states.

## Step-by-Step Approach
1. **Identify pointer positions**: Determine the initial positions based on problem requirements (usually both at ends or both at beginning)
2. **Define movement conditions**: Establish clear rules for when to move each pointer
3. **Define termination conditions**: Determine when to stop the traversal
4. **Process elements**: Operate on the elements at current pointer positions
5. **Move pointers**: Update pointers according to movement conditions
6. **Repeat**: Continue until termination condition is met

## Python-style Pseudocode Template
```python
def two_pointers(arr):
    # Initialize pointers based on problem requirements
    left, right = 0, len(arr) - 1  # Common setup for opposite-direction pointers
    # For same-direction: left, right = 0, 0 or left, right = 0, 1
    
    result = initial_value  # Depends on problem (sum, count, best pair, etc.)
    
    # Continue until pointers meet termination condition
    while left < right:  # Or other condition depending on problem
        current = calculate_current_value(arr[left], arr[right])
        
        # Update result if needed
        if meets_criteria(current):
            result = update_result(result, current)
            
            # Optional: Handle duplicates if needed
            while left < right and arr[left] == arr[left + 1]:
                left += 1
            while left < right and arr[right] == arr[right - 1]:
                right -= 1
        
        # Move pointers based on problem-specific conditions
        if should_move_left(arr[left], arr[right], target):
            left += 1
        else:
            right -= 1
    
    return result
```

## Example LeetCode Hard Problems
- LC 42: Trapping Rain Water
- LC 76: Minimum Window Substring
- LC 239: Sliding Window Maximum
- LC 632: Smallest Range Covering Elements from K Lists
- LC 2444: Count Subarrays With Fixed Bounds

## Optimization Tips
- **Preprocessing**:
  - Sort the array if it's not already sorted (when order matters)
  - Create auxiliary data structures when necessary
- **Efficient pointer movement**:
  - Skip duplicates when appropriate
  - Move multiple steps when possible
  - Use binary search to find next positions in sparse data
- **Combining with other techniques**:
  - Hash tables for O(1) lookups
  - Heaps for maintaining running min/max
  - Monotonic stacks/queues for tracking extremes

## Common Pitfalls
- **Off-by-one errors** when initializing or moving pointers
- **Incorrect termination conditions** leading to infinite loops or premature exits
- **Failing to handle duplicates** when they affect the solution
- **Not considering edge cases** such as empty arrays or single-element arrays
- **Inefficient pointer movement** that doesn't leverage problem structure
- **Using Two Pointers when a more specialized algorithm** is more appropriate
- **Not recognizing when to switch from opposite-direction to same-direction** pointers
