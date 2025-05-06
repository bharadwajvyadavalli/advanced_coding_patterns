# 2D Dynamic Programming

## Category
Core Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving optimization over two distinct variables or dimensions
- When dealing with matrices, grids, or two sequences simultaneously
- Problems that require matching, alignment, or comparison of two arrays
- When subproblems depend on previous results in two dimensions
- Hard-level applications typically involve:
  - Complex state transitions with interdependent dimensions
  - Multiple constraints affecting both dimensions
  - Grid-based optimization with obstacles or special conditions
  - String operations like editing, matching, or aligning
  - Path optimization with varying constraints

## Core Idea / Intuition
2D Dynamic Programming extends the 1D approach by organizing subproblems in a two-dimensional table, where each cell (i,j) represents the solution to a subproblem defined by two parameters. This pattern is particularly powerful for problems involving pairwise relationships, such as comparing two strings or finding optimal paths in a grid.

The key insight is recognizing how states at position (i,j) depend on previous states, typically some combination of (i-1,j), (i,j-1), and (i-1,j-1). By filling the table in the correct order (usually top-to-bottom, left-to-right), we ensure all dependencies are resolved before they're needed.

For hard problems, the challenge often involves complex state definitions, intricate transition functions, and optimizing the solution for time and space constraints.

## Step-by-Step Approach
1. **Define the state**: Clarify what dp[i][j] represents
2. **Establish base cases**: Initialize known values (often zeros in first row/column)
3. **Formulate recurrence relation**: Define how dp[i][j] depends on previous states
4. **Determine iteration order**: Ensure dependencies are computed before needed
5. **Implement solution**: Fill the DP table following the recurrence relation
6. **Optimize space** (if needed): Reduce from O(m×n) to O(min(m,n)) if only recent rows/columns matter
7. **Extract final answer**: Return the appropriate value from the DP table

## Python-style Pseudocode Template
```python
def two_dimensional_dp(sequence1, sequence2):
    m, n = len(sequence1), len(sequence2)
    
    # Step 1: Initialize DP table
    dp = [[initial_value for _ in range(n + 1)] for _ in range(m + 1)]
    
    # Step 2: Set base cases
    for i in range(m + 1):
        dp[i][0] = base_case_value_i  # Often 0 or i, depending on problem
    
    for j in range(n + 1):
        dp[0][j] = base_case_value_j  # Often 0 or j, depending on problem
    
    # Step 3: Fill DP table according to recurrence relation
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            # Consider all possible transitions to position (i,j)
            if condition(sequence1[i-1], sequence2[j-1]):
                dp[i][j] = transition_1(dp[i-1][j-1])
            else:
                dp[i][j] = optimal_value(
                    transition_2(dp[i-1][j]),      # Option 1
                    transition_3(dp[i][j-1]),      # Option 2
                    transition_4(dp[i-1][j-1])     # Option 3
                )
    
    # Step 4: Return final answer
    return dp[m][n]  # Or another appropriate value

# Example: Edit Distance (Levenshtein Distance)
def edit_distance(word1, word2):
    m, n = len(word1), len(word2)
    
    # dp[i][j] = minimum operations to convert word1[0...i-1] to word2[0...j-1]
    dp = [[0 for _ in range(n + 1)] for _ in range(m + 1)]
    
    # Base cases: empty string to non-empty string conversions
    for i in range(m + 1):
        dp[i][0] = i  # Delete all characters
    
    for j in range(n + 1):
        dp[0][j] = j  # Insert all characters
    
    # Fill DP table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i-1] == word2[j-1]:
                # Characters match, no operation needed
                dp[i][j] = dp[i-1][j-1]
            else:
                # Take minimum of insert, delete, or replace
                dp[i][j] = 1 + min(
                    dp[i][j-1],      # Insert
                    dp[i-1][j],      # Delete
                    dp[i-1][j-1]     # Replace
                )
    
    return dp[m][n]

# Example: Longest Common Subsequence
def longest_common_subsequence(text1, text2):
    m, n = len(text1), len(text2)
    
    # dp[i][j] = length of LCS of text1[0...i-1] and text2[0...j-1]
    dp = [[0 for _ in range(n + 1)] for _ in range(m + 1)]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    return dp[m][n]

# Example: Minimum Path Sum in Grid
def min_path_sum(grid):
    if not grid or not grid[0]:
        return 0
    
    m, n = len(grid), len(grid[0])
    
    # dp[i][j] = minimum path sum to reach position (i,j)
    dp = [[0 for _ in range(n)] for _ in range(m)]
    
    # Base case: top-left corner
    dp[0][0] = grid[0][0]
    
    # First row: can only come from left
    for j in range(1, n):
        dp[0][j] = dp[0][j-1] + grid[0][j]
    
    # First column: can only come from above
    for i in range(1, m):
        dp[i][0] = dp[i-1][0] + grid[i][0]
    
    # Fill rest of table
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]
    
    return dp[m-1][n-1]

# Example: Space-optimized 2D DP (using only one row/column)
def space_optimized_lcs(text1, text2):
    m, n = len(text1), len(text2)
    
    # Ensure text1 is the shorter string to minimize space
    if m > n:
        text1, text2 = text2, text1
        m, n = n, m
    
    # Only need to store the current and previous row
    prev_row = [0] * (n + 1)
    curr_row = [0] * (n + 1)
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                curr_row[j] = prev_row[j-1] + 1
            else:
                curr_row[j] = max(prev_row[j], curr_row[j-1])
        
        # Update previous row
        prev_row, curr_row = curr_row, [0] * (n + 1)
    
    return prev_row[n]
```

## Example LeetCode Hard Problems
- LC 10: Regular Expression Matching
- LC 44: Wildcard Matching
- LC 72: Edit Distance
- LC 115: Distinct Subsequences
- LC 1463: Cherry Pickup II

## Optimization Tips
- **State reduction**:
  - Identify the minimal information needed to represent a state
  - Consider if a simpler recurrence relation can achieve the same result
- **Space optimization**:
  - Use rolling arrays when only the current and previous rows/columns matter
  - For many problems, only O(min(m,n)) space is needed instead of O(m×n)
- **Calculation optimizations**:
  - Skip unnecessary calculations when certain conditions are met
  - Process only relevant cells in sparse problems
  - Memoize expensive function calls
- **Problem transformation**:
  - Convert between top-down and bottom-up approaches based on problem characteristics
  - Consider alternative state representations for clearer transitions

## Common Pitfalls
- **Incorrect state definition** leading to incomplete or redundant information
- **Wrong base cases** causing incorrect initialization of the DP table
- **Boundary condition errors** in the table iteration (off-by-one errors)
- **Incorrect recurrence relation** not capturing all possible transitions
- **Not handling edge cases** like empty arrays or single-element arrays
- **Memory limit exceeded** due to inefficient space usage
- **Time limit exceeded** due to unnecessary recalculations
