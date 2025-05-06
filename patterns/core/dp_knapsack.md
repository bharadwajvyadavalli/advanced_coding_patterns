# Knapsack Dynamic Programming

## Category
Core Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Resource allocation problems with capacity constraints
- Problems involving selecting items with values and weights/costs
- Subset selection problems with optimization objectives
- When deciding which elements to include to maximize/minimize a value
- Hard-level applications typically involve:
  - Multiple constraints beyond just capacity
  - Multi-dimensional knapsack problems
  - Items with dependencies or mutual exclusivity
  - Complex value functions beyond simple addition
  - Large inputs requiring optimization

## Core Idea / Intuition
The Knapsack pattern solves problems where you need to select a subset of items, each with a value and a weight, to maximize total value while staying within a weight limit. The core insight is that for each item, we have two choices: include it or exclude it, and the optimal solution builds upon solutions to smaller subproblems.

The 0/1 Knapsack variant allows either taking an item completely or leaving it (no fractional selection), while the Unbounded Knapsack allows taking an item multiple times. The pattern uses a DP table where dp[i][w] represents the maximum value achievable with first i items and weight limit w.

For hard problems, the challenge often involves complex constraints, multi-dimensional considerations, or optimizing the solution for large input sizes.

## Step-by-Step Approach
1. **Define the state**: Set up what dp[i][w] represents
2. **Establish base cases**: Initialize known values (often zeros)
3. **Formulate recurrence relation**: Define the choice between including or excluding each item
4. **Determine iteration order**: Process items and weights in appropriate sequence
5. **Implement solution**: Fill the DP table following the recurrence relation
6. **Optimize space** (if needed): Reduce from O(n×W) to O(W) if appropriate
7. **Extract result and/or selected items**: Recover the final answer and potentially the chosen items

## Python-style Pseudocode Template
```python
# 0/1 Knapsack Problem
def knapsack_01(values, weights, capacity):
    n = len(values)
    
    # Create DP table
    dp = [[0 for _ in range(capacity + 1)] for _ in range(n + 1)]
    
    # Fill DP table
    for i in range(1, n + 1):
        for w in range(capacity + 1):
            # Case 1: Current item is not included
            dp[i][w] = dp[i-1][w]
            
            # Case 2: Current item is included (if it fits)
            if weights[i-1] <= w:
                include_value = dp[i-1][w - weights[i-1]] + values[i-1]
                dp[i][w] = max(dp[i][w], include_value)
    
    return dp[n][capacity]

# Space-optimized 0/1 Knapsack
def knapsack_01_optimized(values, weights, capacity):
    n = len(values)
    
    # Only need two rows: previous and current
    dp = [0] * (capacity + 1)
    
    for i in range(n):
        # Process weights from right to left to avoid using updated values
        for w in range(capacity, weights[i] - 1, -1):
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    
    return dp[capacity]

# Unbounded Knapsack Problem (items can be used multiple times)
def unbounded_knapsack(values, weights, capacity):
    n = len(values)
    
    # Create DP array
    dp = [0] * (capacity + 1)
    
    # Fill DP array
    for w in range(1, capacity + 1):
        for i in range(n):
            if weights[i] <= w:
                dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    
    return dp[capacity]

# Reconstructing the selected items
def knapsack_with_items(values, weights, capacity):
    n = len(values)
    
    # Create DP table
    dp = [[0 for _ in range(capacity + 1)] for _ in range(n + 1)]
    
    # Fill DP table
    for i in range(1, n + 1):
        for w in range(capacity + 1):
            # Case 1: Current item is not included
            dp[i][w] = dp[i-1][w]
            
            # Case 2: Current item is included (if it fits)
            if weights[i-1] <= w:
                include_value = dp[i-1][w - weights[i-1]] + values[i-1]
                dp[i][w] = max(dp[i][w], include_value)
    
    # Reconstruct solution
    selected_items = []
    w = capacity
    
    for i in range(n, 0, -1):
        # Check if the item was included in the optimal solution
        if dp[i][w] != dp[i-1][w]:
            selected_items.append(i-1)  # Item was included
            w -= weights[i-1]
    
    return dp[n][capacity], selected_items

# Multi-constraint Knapsack (e.g., weight and volume limits)
def multi_constraint_knapsack(values, weights, volumes, max_weight, max_volume):
    n = len(values)
    
    # 3D DP table: dp[i][w][v] = max value using first i items with weight ≤ w and volume ≤ v
    dp = [[[0 for _ in range(max_volume + 1)] for _ in range(max_weight + 1)] for _ in range(n + 1)]
    
    for i in range(1, n + 1):
        for w in range(max_weight + 1):
            for v in range(max_volume + 1):
                # Case 1: Current item is not included
                dp[i][w][v] = dp[i-1][w][v]
                
                # Case 2: Current item is included (if it fits both constraints)
                if weights[i-1] <= w and volumes[i-1] <= v:
                    include_value = dp[i-1][w - weights[i-1]][v - volumes[i-1]] + values[i-1]
                    dp[i][w][v] = max(dp[i][w][v], include_value)
    
    return dp[n][max_weight][max_volume]
```

## Example LeetCode Hard Problems
- LC 474: Ones and Zeroes (Multi-dimensional Knapsack)
- LC 879: Profitable Schemes
- LC 956: Tallest Billboard
- LC 1449: Form Largest Integer With Digits That Add up to Target
- LC 1621: Number of Sets of K Non-Overlapping Line Segments

## Optimization Tips
- **Space optimization**:
  - Use a single array updated in-place for 0/1 Knapsack (iterate backwards)
  - Use a single array for Unbounded Knapsack (iterate forwards)
  - Use rolling arrays for multi-dimensional Knapsack
- **Computation optimizations**:
  - Sort items by value-to-weight ratio for greedy preprocessing
  - Precompute groups of items for certain variants
  - Apply branch and bound techniques for early pruning
- **Problem transformations**:
  - Convert between problems with weight constraints and value constraints
  - Transform related problems into Knapsack format
  - Reduce the capacity for sparse weight distributions
- **Large input handling**:
  - Use approximation algorithms for very large capacities
  - Apply scaling techniques to reduce precision while maintaining accuracy
  - Consider meet-in-the-middle for specific problem variants

## Common Pitfalls
- **Off-by-one errors** in DP table initialization and iteration
- **Incorrect direction of iteration** when optimizing space (backwards for 0/1, forwards for unbounded)
- **Not handling edge cases** like empty lists or zero capacity
- **Confusing 0/1 Knapsack and Unbounded Knapsack** implementations
- **Memory limit exceeded** for large capacities (requires space optimization)
- **Time limit exceeded** due to unnecessary calculations
- **Incorrect item selection reconstruction** when retrieving the actual items
