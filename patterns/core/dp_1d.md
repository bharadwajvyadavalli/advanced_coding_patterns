# 1D Dynamic Programming

## Category
Core Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving optimization over a linear sequence
- When seeking maximum/minimum values with dependency on previous decisions
- Problems requiring counting arrangements or ways to achieve a state
- When choices at one position affect future positions
- Hard-level applications typically involve:
  - Complex state transitions with multiple dependencies
  - Optimization with multiple constraints
  - Non-obvious recurrence relations
  - Calculating the exact solution for large inputs
  - Problems requiring efficient space optimization

## Core Idea / Intuition
1D Dynamic Programming solves problems by breaking them down into a linear sequence of sub-problems, where the solution at each position depends on solutions to earlier positions. The approach builds solutions iteratively, reusing previously computed results to avoid redundant calculations.

The key insight is identifying a recurrence relation that expresses the solution at position i in terms of solutions at positions j < i. By filling a one-dimensional DP array in the appropriate order, we ensure all dependencies are resolved before they're needed.

For hard problems, the challenge often lies in formulating the correct state representation and transition function, as well as optimizing the solution for space and time constraints.

## Step-by-Step Approach
1. **Define the state**: Determine what dp[i] represents
2. **Establish base cases**: Initialize known starting values
3. **Formulate recurrence relation**: Define how dp[i] relates to previous states
4. **Determine iteration order**: Ensure dependencies are computed before needed
5. **Implement solution**: Fill the DP array following the recurrence relation
6. **Optimize space** (if needed): Reduce from O(n) to O(1) if only recent states matter
7. **Extract final answer**: Return the appropriate value from the DP array

## Python-style Pseudocode Template
```python
def one_dimensional_dp(input_data):
    n = len(input_data)
    
    # Step 1: Initialize DP array
    dp = [initial_value] * (n + 1)  # Size might vary based on problem
    
    # Step 2: Set base cases
    dp[0] = base_case_value  # Often 0 or 1, depending on problem
    
    # Step 3: Fill DP array according to recurrence relation
    for i in range(1, n + 1):
        # Consider all possible transitions to position i
        for j in range(some_range):
            dp[i] = optimal_value(dp[i], dp[i-j] + transition_value(input_data, i, j))
    
    # Step 4: Return final answer
    return dp[n]  # Or another appropriate value

# Example: Maximum Subarray Sum
def max_subarray_sum(nums):
    if not nums:
        return 0
    
    n = len(nums)
    # dp[i] represents the maximum subarray sum ending at index i
    dp = [0] * n
    
    # Base case
    dp[0] = nums[0]
    
    # Fill DP array
    for i in range(1, n):
        # Either extend previous subarray or start a new one
        dp[i] = max(nums[i], dp[i-1] + nums[i])
    
    # Return maximum value in dp array
    return max(dp)

# Example: Longest Increasing Subsequence
def longest_increasing_subsequence(nums):
    if not nums:
        return 0
    
    n = len(nums)
    # dp[i] represents the length of the longest increasing subsequence ending at index i
    dp = [1] * n  # Every element by itself forms a subsequence of length 1
    
    for i in range(1, n):
        for j in range(i):
            if nums[i] > nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)
    
    return max(dp)

# Example: Coin Change (Minimum number of coins to make an amount)
def coin_change(coins, amount):
    # dp[i] represents the minimum number of coins needed to make amount i
    dp = [float('inf')] * (amount + 1)
    
    # Base case
    dp[0] = 0  # 0 coins needed to make amount 0
    
    for i in range(1, amount + 1):
        for coin in coins:
            if i - coin >= 0:
                dp[i] = min(dp[i], dp[i - coin] + 1)
    
    return dp[amount] if dp[amount] != float('inf') else -1

# Example: Space-optimized 1D DP (Fibonacci)
def fibonacci(n):
    if n <= 1:
        return n
    
    # Only need to keep track of the last two values
    prev, curr = 0, 1
    
    for i in range(2, n + 1):
        prev, curr = curr, prev + curr
    
    return curr
```

## Example LeetCode Hard Problems
- LC 32: Longest Valid Parentheses
- LC 123: Best Time to Buy and Sell Stock III
- LC 188: Best Time to Buy and Sell Stock IV
- LC 1235: Maximum Profit in Job Scheduling
- LC 1531: String Compression II

## Optimization Tips
- **State reduction**:
  - Identify the minimal information needed to represent a state
  - Use primitive types instead of objects for faster operations
- **Space optimization**:
  - Use rolling arrays or variables when only recent states matter
  - Reuse the same array for problems with specific dependency patterns
- **Preprocessing**:
  - Sort or transform input to simplify state transitions
  - Precompute values that will be reused frequently
- **Computation order**:
  - Process states in dependency order to ensure correct results
  - Consider bidirectional DP for certain problems

## Common Pitfalls
- **Incorrect state definition** leading to missing or redundant information
- **Wrong base cases** causing incorrect initialization
- **Overlooking edge cases** such as empty input or boundary conditions
- **Incorrect recurrence relation** not capturing all transitions or constraints
- **Integer overflow** for problems with large inputs or accumulated values
- **Inefficient implementation** neglecting opportunities for space optimization
- **Not recognizing when 1D DP is applicable** versus other approaches
