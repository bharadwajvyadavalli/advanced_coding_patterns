# State Compression (Bitmask DP)

## Category
Special Technique

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving combinations, subsets, or permutations of a small set of elements (usually n ≤ 20)
- NP-hard problems like Traveling Salesman Problem or problems requiring exploring all possible states
- Problems with states that can be efficiently represented as "on/off" for each element
- When the state space is too large for standard DP but can be compressed with bits
- Hard-level applications typically involve:
  - Multiple interacting constraints that must be tracked simultaneously
  - Complex state transitions between subproblems
  - Dependency on the entire state history rather than just a few previous states

## Core Idea / Intuition
State Compression (Bitmask DP) uses bits to represent states, where each bit indicates whether an element is included/excluded or a condition is satisfied/unsatisfied. This allows representing up to 32 (or 64) boolean states in a single integer, significantly reducing memory usage and enabling efficient state transitions through bitwise operations.

For hard problems, we often need to track complex states that would be unwieldy with conventional representations but become manageable with bitmasks.

## Step-by-Step Approach
1. **Identify elements to track**: Determine which elements or conditions need binary (on/off) tracking
2. **Create state representation**: Assign each element to a bit position (0 to n-1)
3. **Define state transitions**: Determine how operations change the state (using bitwise operations)
4. **Initialize DP table**: Create a table indexed by the bitmask states
5. **Define base cases**: Set values for initial states
6. **Fill DP table**: Process all relevant states in an order that respects dependencies
7. **Extract solution**: Retrieve final answer from the completed DP table

## Python-style Pseudocode Template
```python
def bitmask_dp(n):
    # Step 1: Initialize DP table
    # dp[mask] represents solution for state represented by mask
    dp = [initial_value] * (1 << n)  # Size 2^n for n elements
    
    # Step 2: Base case(s)
    dp[0] = base_case_value
    
    # Step 3: Fill DP table
    for mask in range(1, 1 << n):
        for i in range(n):
            # Check if bit i is set in mask
            if (mask & (1 << i)) > 0:
                # Previous state without bit i
                prev_mask = mask ^ (1 << i)
                
                # Transition from previous state
                dp[mask] = combine(dp[mask], dp[prev_mask], i)
    
    # Step 4: Return final result
    return dp[(1 << n) - 1]  # All bits set = final state
```

## Example LeetCode Hard Problems
- LC 847: Shortest Path Visiting All Nodes
- LC 943: Find the Shortest Superstring
- LC 1125: Smallest Sufficient Team
- LC 1349: Maximum Students Taking Exam
- LC 1799: Maximize Score After N Operations

## Optimization Tips
- **Pre-compute state transitions** for frequent operations
- **Use lookup tables** for common bit operations:
  - Counting bits: `__builtin_popcount()` in C++ or `bin(mask).count('1')` in Python
  - Getting lowest set bit: `mask & -mask`
  - Iterating through all subsets of a set: `subset = mask; while subset > 0: subset = (subset - 1) & mask`
- **Memory optimization**:
  - Use rolling arrays when only the previous state matters
  - Prune impossible states
- **Bitwise shortcuts**:
  - `(1 << n) - 1` creates a mask with n bits set
  - `mask & (1 << i)` checks if bit i is set
  - `mask | (1 << i)` sets bit i
  - `mask & ~(1 << i)` clears bit i
  - `mask ^ (1 << i)` toggles bit i

## Common Pitfalls
- **Incorrect bit manipulation** leading to wrong state transitions
- **Off-by-one errors** in bit positions or state indexing
- **Failing to handle empty sets** or other edge cases
- **Inefficient state enumeration** that doesn't leverage problem structure
- **Using bitmasks when state space is too large** (n > 30), causing integer overflow
- **Not recognizing when meet-in-the-middle** can be combined with bitmask DP
- **Incorrectly defining the problem state**, missing key constraints or dependencies
