# State Compression Dynamic Programming

## Category
Core Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems with states that can be efficiently represented using bits
- When dealing with multiple boolean conditions or choices
- Subset-based problems with relatively small number of elements (typically n ≤ 20)
- Graph problems with subset relationships
- Hard-level applications typically involve:
  - Complex state transitions with many variables
  - Exponential state spaces that need efficient representation
  - Problems requiring tracking of visited/unused elements
  - Optimization across multiple dimensions
  - Constraint satisfaction with numerous conditions

## Core Idea / Intuition
State Compression DP uses bit manipulation to represent states compactly, leveraging the fact that a 32-bit integer can represent up to 32 boolean values. By encoding the state of a problem into bits, we can dramatically reduce memory usage and often simplify state transitions.

The key insight is recognizing when a problem's state has many binary (yes/no) components that can be combined into a single bitmask. This approach is particularly powerful for problems involving subsets, where we need to track which elements are included or excluded, or for problems requiring the exploration of all possible combinations.

For hard problems, the challenge often lies in designing efficient state transitions that utilize bitwise operations and in handling the complexity of state representation correctly.

## Step-by-Step Approach
1. **Define the state**: Determine what each bit in the mask represents
2. **Establish state representation**: Convert problem constraints to bitmasks
3. **Set up DP structure**: Define dp[state] or dp[state][additional_params]
4. **Initialize base cases**: Set known values for trivial states
5. **Formulate transitions**: Define how to move between states using bit operations
6. **Process states systematically**: Iterate through all possible states in appropriate order
7. **Extract final result**: Retrieve the answer from the completed DP array

## Python-style Pseudocode Template
```python
def state_compression_dp(n, problem_params):
    # Total number of possible states with n elements (2^n)
    total_states = 1 << n
    
    # Initialize DP array for all possible states
    dp = [initial_value] * total_states
    
    # Base case: empty set
    dp[0] = base_case_value
    
    # Iterate through all possible states
    for state in range(total_states):
        # Process current state
        
        # Option 1: Add a new element to the current state
        for element in range(n):
            # Check if element is not already in state
            if not (state & (1 << element)):
                # Create new state by adding element
                new_state = state | (1 << element)
                # Update DP value
                dp[new_state] = optimal_value(dp[new_state], dp[state] + transition_value(element))
        
        # Option 2: Process transitions based on current state configuration
        # This depends on the specific problem
        process_state_specific_transitions(state, dp)
    
    # Return final answer (often the state with all elements)
    return dp[total_states - 1]  # Or another appropriate final state

# Example: Traveling Salesman Problem (TSP)
def traveling_salesman(distances):
    n = len(distances)  # Number of cities
    total_states = 1 << n
    
    # dp[mask][last] = shortest path visiting all cities in mask and ending at 'last'
    dp = [[float('inf') for _ in range(n)] for _ in range(total_states)]
    
    # Base case: start at city 0
    dp[1][0] = 0  # Mask 1 represents only city 0 visited
    
    # Iterate through all possible states
    for mask in range(1, total_states):
        for last in range(n):
            # Skip invalid states (where last city is not in mask)
            if not (mask & (1 << last)):
                continue
            
            # Previous state without the last city
            prev_mask = mask ^ (1 << last)
            
            # Skip if previous mask is empty (except for base case)
            if prev_mask == 0:
                continue
            
            # Try all possible previous cities
            for prev in range(n):
                if mask & (1 << prev):
                    dp[mask][last] = min(
                        dp[mask][last],
                        dp[prev_mask][prev] + distances[prev][last]
                    )
    
    # Find shortest tour that visits all cities and returns to city 0
    all_visited = total_states - 1
    result = min(dp[all_visited][i] + distances[i][0] for i in range(1, n))
    
    return result

# Example: Minimum Hamming Distance with K operations
def min_hamming_distance(source, target, allowedSwaps, k):
    n = len(source)
    
    # Create graph of allowed swaps
    graph = [[] for _ in range(n)]
    for i, j in allowedSwaps:
        graph[i].append(j)
        graph[j].append(i)
    
    # Find connected components using DFS
    visited = [False] * n
    components = []
    
    for i in range(n):
        if not visited[i]:
            component = []
            dfs(graph, i, visited, component)
            components.append(component)
    
    total_hamming = 0
    
    # Process each connected component separately
    for component in components:
        m = len(component)
        
        if m <= k:
            # Can perform as many swaps as needed in this component
            # Count mismatches that cannot be fixed
            source_chars = [source[i] for i in component]
            target_chars = [target[i] for i in component]
            source_count = Counter(source_chars)
            target_count = Counter(target_chars)
            
            # Count characters that can't be matched
            hamming = 0
            for char, count in source_count.items():
                hamming += max(0, count - target_count.get(char, 0))
            
            total_hamming += hamming
        else:
            # Need to use state compression DP for optimal swaps
            # dp[mask][remaining_ops] = min hamming distance after applying operations to positions in mask
            total_states = 1 << m
            dp = [[float('inf') for _ in range(k + 1)] for _ in range(total_states)]
            
            # Base case: no positions selected, no operations
            dp[0][0] = 0
            
            for mask in range(total_states):
                for ops in range(k + 1):
                    if dp[mask][ops] == float('inf'):
                        continue
                    
                    # Try adding a new position
                    for idx in range(m):
                        pos = component[idx]
                        
                        if not (mask & (1 << idx)):
                            new_mask = mask | (1 << idx)
                            
                            # Option 1: No swap, accept mismatch if any
                            mismatch = 0 if source[pos] == target[pos] else 1
                            dp[new_mask][ops] = min(dp[new_mask][ops], dp[mask][ops] + mismatch)
                            
                            # Option 2: Swap with a previously selected position
                            if ops < k:
                                for prev_idx in range(m):
                                    if mask & (1 << prev_idx):
                                        prev_pos = component[prev_idx]
                                        
                                        # Check if swap helps
                                        original_mismatches = (0 if source[pos] == target[pos] else 1) + (0 if source[prev_pos] == target[prev_pos] else 1)
                                        after_swap_mismatches = (0 if source[prev_pos] == target[pos] else 1) + (0 if source[pos] == target[prev_pos] else 1)
                                        
                                        if after_swap_mismatches < original_mismatches:
                                            dp[new_mask][ops + 1] = min(dp[new_mask][ops + 1], dp[mask][ops] + after_swap_mismatches)
            
            # Find minimum hamming distance for this component
            component_hamming = min(dp[total_states - 1])
            total_hamming += component_hamming
    
    return total_hamming

# Example: Counting valid configurations (e.g., N-Queens state count)
def count_valid_arrangements(n):
    total_states = 1 << n
    
    # dp[row][col_mask] = number of valid arrangements for rows 0...row with columns used in col_mask
    dp = [[0 for _ in range(total_states)] for _ in range(n + 1)]
    
    # Base case: no queens placed is 1 valid arrangement
    dp[0][0] = 1
    
    for row in range(n):
        for col_mask in range(total_states):
            # Skip invalid states (more columns used than rows processed)
            if bin(col_mask).count('1') != row:
                continue
            
            # Try placing queen in each column of the next row
            for col in range(n):
                # Skip if column already used
                if col_mask & (1 << col):
                    continue
                
                # Check if placement is valid (diagonal checks can be done here)
                if is_valid_placement(row, col, col_mask, n):
                    new_col_mask = col_mask | (1 << col)
                    dp[row + 1][new_col_mask] += dp[row][col_mask]
    
    # Count all valid arrangements for the last row
    result = 0
    for col_mask in range(total_states):
        if bin(col_mask).count('1') == n:
            result += dp[n][col_mask]
    
    return result
```

## Example LeetCode Hard Problems
- LC 691: Stickers to Spell Word
- LC 847: Shortest Path Visiting All Nodes
- LC 943: Find the Shortest Superstring
- LC 1434: Number of Ways to Wear Different Hats to Each Other
- LC 1799: Maximize Score After N Operations

## Optimization Tips
- **Bit manipulation shortcuts**:
  - `1 << n` to create a mask with the nth bit set
  - `mask & (1 << i)` to check if bit i is set
  - `mask | (1 << i)` to set bit i
  - `mask & ~(1 << i)` to clear bit i
  - `mask ^ (1 << i)` to toggle bit i
  - `mask & (mask - 1)` to clear the lowest set bit
  - `mask & -mask` to isolate the lowest set bit
- **Iteration optimizations**:
  - Iterate through subsets: `subset = mask; while subset > 0: subset = (subset - 1) & mask`
  - Iterate through all set bits: `bit = 0; while (1 << bit) <= mask: if mask & (1 << bit): process(bit); bit += 1`
- **Compact state representation**:
  - Use integer keys for hash maps instead of tuples for faster lookup
  - Combine multiple parameters into a single state when possible
  - Exploit symmetry to reduce the number of states
- **Memory management**:
  - Use sparse representations for very large state spaces
  - Consider top-down DP with memoization for problems where only a fraction of states are visited

## Common Pitfalls
- **Off-by-one errors** in bit positions or masks
- **Integer overflow** when the number of elements approaches 32
- **Incorrect state transitions** due to misunderstanding bit operations
- **Mishandling base cases** leading to incorrect counting or optimization
- **Inefficient bit operations** that could be simplified
- **Not recognizing when state compression is applicable** or when it adds unnecessary complexity
- **Memory limit exceeded** for problems with too many states or additional parameters
