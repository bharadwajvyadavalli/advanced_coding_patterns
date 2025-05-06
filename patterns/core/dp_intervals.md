# Interval Dynamic Programming

## Category
Core Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving dividing sequences into optimal segments
- When selecting or operating on intervals or subarrays
- Problems requiring computations on all possible subranges
- String or array problems with nested interval considerations
- Hard-level applications typically involve:
  - Multiple criteria for evaluating intervals
  - Nested interval dependencies
  - Optimal partitioning with complex scoring functions
  - Interval merging or splitting with constraints
  - Non-local optimization across different intervals

## Core Idea / Intuition
Interval DP deals with problems where we need to find optimal ways to process ranges or segments of an array or string. The key insight is representing a subproblem as dp[i][j], denoting the optimal solution for the interval from index i to j. By systematically building solutions for smaller intervals first (often using a bottom-up approach with increasing interval sizes), we can solve for larger intervals using previously computed results.

The pattern typically involves a recurrence relation where dp[i][j] is computed by considering different ways to process the interval [i,j], such as splitting it at different positions k where i ≤ k < j, or by extending a smaller interval based on boundary elements.

For hard problems, the challenge often lies in identifying the correct state representation, formulating the recursive relationship that connects different intervals, and efficiently implementing the solution to handle larger inputs.

## Step-by-Step Approach
1. **Define the state**: Establish what dp[i][j] represents for interval [i,j]
2. **Establish base cases**: Set values for trivial intervals (often single elements or empty ranges)
3. **Determine processing order**: Usually by interval length (shorter to longer)
4. **Formulate recurrence relation**: Define how dp[i][j] depends on smaller intervals
5. **Implement nested loops**: Process intervals of increasing length
6. **Handle edge cases**: Consider special cases for certain interval patterns
7. **Extract final result**: Typically found in dp[0][n-1] for the full array

## Python-style Pseudocode Template
```python
def interval_dp(arr):
    n = len(arr)
    
    # Initialize DP table for intervals [i...j]
    dp = [[initial_value for _ in range(n)] for _ in range(n)]
    
    # Base cases: intervals of length 1
    for i in range(n):
        dp[i][i] = base_case_value(arr[i])
    
    # Process intervals of increasing length
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1  # End of current interval
            
            # Initialize current interval result
            dp[i][j] = default_value
            
            # Consider all ways to process this interval
            
            # Option 1: Split interval at different positions
            for k in range(i, j):
                dp[i][j] = optimal_value(
                    dp[i][j],
                    combine_split_results(dp[i][k], dp[k+1][j], arr, i, k, j)
                )
            
            # Option 2: Process based on boundary elements
            dp[i][j] = optimal_value(
                dp[i][j],
                process_boundaries(dp[i+1][j-1], arr[i], arr[j])
            )
    
    # Return result for the entire array
    return dp[0][n-1]

# Example: Matrix Chain Multiplication
def matrix_chain_multiplication(dimensions):
    n = len(dimensions) - 1  # Number of matrices
    
    # dp[i][j] = minimum number of scalar multiplications to compute M[i...j]
    dp = [[0 for _ in range(n)] for _ in range(n)]
    
    # Base cases: single matrices require no multiplications
    # dp[i][i] = 0 (already initialized)
    
    # Process intervals of increasing length
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            
            dp[i][j] = float('inf')
            
            # Try all possible split positions
            for k in range(i, j):
                # Cost = cost of left subproblem + cost of right subproblem + cost of multiplying resulting matrices
                cost = dp[i][k] + dp[k+1][j] + dimensions[i] * dimensions[k+1] * dimensions[j+1]
                dp[i][j] = min(dp[i][j], cost)
    
    return dp[0][n-1]

# Example: Palindrome Partitioning (minimum cuts)
def palindrome_partitioning(s):
    n = len(s)
    
    # Precompute palindrome information: is_palindrome[i][j] = whether s[i...j] is a palindrome
    is_palindrome = [[False for _ in range(n)] for _ in range(n)]
    
    # Single characters are palindromes
    for i in range(n):
        is_palindrome[i][i] = True
    
    # Check palindromes of length 2
    for i in range(n - 1):
        if s[i] == s[i + 1]:
            is_palindrome[i][i + 1] = True
    
    # Check palindromes of length > 2
    for length in range(3, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            if s[i] == s[j] and is_palindrome[i + 1][j - 1]:
                is_palindrome[i][j] = True
    
    # dp[i] = minimum cuts needed for s[0...i]
    dp = [i for i in range(n)]  # Maximum cuts needed
    
    for j in range(n):
        if is_palindrome[0][j]:
            dp[j] = 0  # Whole string is a palindrome, no cuts needed
            continue
        
        for i in range(j):
            if is_palindrome[i + 1][j]:
                dp[j] = min(dp[j], dp[i] + 1)
    
    return dp[n-1]

# Example: Burst Balloons
def burst_balloons(nums):
    # Add boundaries with value 1
    nums = [1] + nums + [1]
    n = len(nums)
    
    # dp[i][j] = maximum coins obtained by bursting all balloons in range (i,j)
    # Note: i and j are not burst
    dp = [[0 for _ in range(n)] for _ in range(n)]
    
    # Process intervals of increasing length
    for length in range(1, n - 1):
        for left in range(1, n - length):
            right = left + length
            
            # Try all possible balloons to burst last in this interval
            for k in range(left, right + 1):
                # Last burst: nums[left-1] * nums[k] * nums[right+1]
                # Plus optimal solutions for subarrays
                gain = nums[left - 1] * nums[k] * nums[right + 1]
                dp[left][right] = max(
                    dp[left][right],
                    dp[left][k-1] + gain + dp[k+1][right]
                )
    
    return dp[1][n-2]  # Result for the original array

# Example: Stone Game (whether first player can win)
def stone_game(piles):
    n = len(piles)
    
    # dp[i][j][0] = maximum score difference for player 1 in range [i,j]
    # dp[i][j][1] = score of player 1 in range [i,j]
    dp = [[[0, 0] for _ in range(n)] for _ in range(n)]
    
    # Base cases: single piles
    for i in range(n):
        dp[i][i][0] = piles[i]  # Difference is the pile value
        dp[i][i][1] = piles[i]  # Score is the pile value
    
    # Process intervals of increasing length
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            
            # Take left pile
            take_left = piles[i] + dp[i+1][j][1] - dp[i+1][j][0]
            
            # Take right pile
            take_right = piles[j] + dp[i][j-1][1] - dp[i][j-1][0]
            
            if take_left > take_right:
                dp[i][j][0] = take_left
                dp[i][j][1] = dp[i+1][j][1]
            else:
                dp[i][j][0] = take_right
                dp[i][j][1] = dp[i][j-1][1]
    
    # Player 1 wins if score difference > 0
    return dp[0][n-1][0] > 0
```

## Example LeetCode Hard Problems
- LC 312: Burst Balloons
- LC 546: Remove Boxes
- LC 664: Strange Printer
- LC 1000: Minimum Cost to Merge Stones
- LC 1547: Minimum Cost to Cut a Stick

## Optimization Tips
- **Precomputation**:
  - Calculate and store interval properties (like palindromes) before main DP
  - Use auxiliary data structures to speed up interval queries
- **Order of computation**:
  - Process subproblems in order of increasing length
  - Consider diagonal traversal of the DP table for more efficient cache usage
- **Memory optimization**:
  - For some problems, only O(n) space is needed instead of O(n²)
  - Use bit manipulation to compress state representation
- **Pruning techniques**:
  - Skip invalid or impossible intervals
  - Use bounds to eliminate unpromising partitioning points
  - Apply problem-specific constraints to reduce search space

## Common Pitfalls
- **Incorrect interval interpretation**: Off-by-one errors in defining [i,j]
- **Wrong processing order**: Not ensuring smaller subproblems are solved first
- **Missing base cases**: Incorrectly handling single-element or empty intervals
- **Boundary issues**: Not properly addressing the inclusion/exclusion of endpoints
- **Inefficient implementation**: Redundant recalculations or suboptimal state representation
- **Memory limit exceeded**: Not optimizing space for large inputs
- **Time limit exceeded**: Not recognizing opportunities for pruning or optimization
