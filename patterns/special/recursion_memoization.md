# Recursion with Memoization

## Category
Special Technique

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems exhibiting overlapping subproblems and optimal substructure
- When a top-down approach is more intuitive than bottom-up DP
- Problems with complex state transitions or multiple parameters
- When the complete state space doesn't need to be explored
- Hard-level applications typically involve:
  - Multi-dimensional state spaces
  - Complex state transitions with multiple parameters
  - Sparse state spaces where only a fraction of states are visited
  - Recursive structures like trees or graphs
  - Problems requiring backtracking with pruning

## Core Idea / Intuition
Recursion with Memoization (also known as top-down dynamic programming) solves problems by breaking them down into smaller subproblems, solving each subproblem recursively, and storing the results to avoid redundant computation. Unlike pure recursion, which can lead to exponential time complexity, memoization ensures each subproblem is solved only once, resulting in polynomial time complexity.

The key insight is recognizing when subproblems overlap and using a cache (memo) to store and retrieve previously computed results. This combines the intuitive formulation of recursive solutions with the efficiency of dynamic programming.

For hard problems, memoization is often preferable to bottom-up DP when the state space is complex or when only a fraction of the possible states are actually needed for the solution.

## Step-by-Step Approach
1. **Identify state representation**: Define what information fully describes a subproblem
2. **Establish base cases**: Determine the simplest scenarios with known answers
3. **Define recursive relation**: Formulate how to compute results from smaller subproblems
4. **Implement memoization**: Add caching mechanism to store computed results
5. **Write recursive function**: Implement the solution with cache checks and updates
6. **Handle constraints**: Ensure state bounds are properly managed
7. **Extract final result**: Return the answer to the original problem

## Python-style Pseudocode Template
```python
def solve_problem(input_data):
    # Define memoization cache
    memo = {}  # Or other appropriate data structure
    
    def dp(state):
        # Check if result already computed
        if state in memo:
            return memo[state]
        
        # Base cases
        if is_base_case(state):
            return base_case_value(state)
        
        # Recursive calculation
        result = initial_value  # Depends on problem (min/max/count)
        
        # Explore all possible transitions
        for next_state in get_next_states(state):
            sub_result = dp(next_state)
            result = combine_results(result, sub_result)
        
        # Cache and return result
        memo[state] = result
        return result
    
    # Start recursion from initial state
    return dp(initial_state(input_data))

# Example: Longest Increasing Subsequence with memoization
def longest_increasing_subsequence(nums):
    n = len(nums)
    memo = {}
    
    def dp(current_index, prev_index):
        # Check cache
        if (current_index, prev_index) in memo:
            return memo[(current_index, prev_index)]
        
        # Base case: reached end of array
        if current_index == n:
            return 0
        
        # Option 1: Skip current element
        skip = dp(current_index + 1, prev_index)
        
        # Option 2: Include current element if it forms increasing subsequence
        take = 0
        if prev_index == -1 or nums[current_index] > nums[prev_index]:
            take = 1 + dp(current_index + 1, current_index)
        
        # Cache and return result
        memo[(current_index, prev_index)] = max(skip, take)
        return memo[(current_index, prev_index)]
    
    return dp(0, -1)

# Example: Edit Distance with memoization
def edit_distance(word1, word2):
    memo = {}
    
    def dp(i, j):
        # Check cache
        if (i, j) in memo:
            return memo[(i, j)]
        
        # Base cases
        if i == len(word1):
            return len(word2) - j
        if j == len(word2):
            return len(word1) - i
        
        # If characters match, no operation needed
        if word1[i] == word2[j]:
            result = dp(i + 1, j + 1)
        else:
            # Try all three operations (insert, delete, replace)
            insert = dp(i, j + 1)
            delete = dp(i + 1, j)
            replace = dp(i + 1, j + 1)
            
            result = 1 + min(insert, delete, replace)
        
        # Cache and return result
        memo[(i, j)] = result
        return result
    
    return dp(0, 0)

# Example: Wildcard Matching with memoization
def wildcard_matching(s, p):
    memo = {}
    
    def dp(i, j):
        # Check cache
        if (i, j) in memo:
            return memo[(i, j)]
        
        # Base cases
        if j == len(p):
            return i == len(s)
        
        if i == len(s):
            # Remaining pattern must be all stars
            return all(p[x] == '*' for x in range(j, len(p)))
        
        # Current pattern character is '*'
        if p[j] == '*':
            # Either match current character and stay at '*',
            # or skip the '*' entirely
            result = dp(i + 1, j) or dp(i, j + 1)
        
        # Current pattern character is '?' or matches
        elif p[j] == '?' or p[j] == s[i]:
            result = dp(i + 1, j + 1)
        
        # Characters don't match
        else:
            result = False
        
        # Cache and return result
        memo[(i, j)] = result
        return result
    
    return dp(0, 0)
```

## Example LeetCode Hard Problems
- LC 10: Regular Expression Matching
- LC 44: Wildcard Matching
- LC 72: Edit Distance
- LC 115: Distinct Subsequences
- LC 1692: Count Ways to Distribute Candies

## Optimization Tips
- **Efficient state representation**:
  - Use tuples as keys for immutable states
  - Pack multiple parameters into a single key
  - Use frozen sets for unordered collections
- **Memory optimization**:
  - Clear memo cache for large input sizes in iterative scenarios
  - Use weak references or LRU caches for very large state spaces
  - Consider bit manipulation for compact state representation
- **Cache data structure selection**:
  - Use dictionaries for general purpose caching
  - Use arrays for continuous numeric state spaces
  - Consider custom cache implementations for complex states
- **Recursion optimization**:
  - Apply tail recursion when possible (though Python doesn't optimize for it)
  - Reorder recursion branches to hit base cases earlier
  - Use recursion limits to prevent stack overflow

## Common Pitfalls
- **Incorrect state representation** leading to cache misses
- **Mutable objects as cache keys** causing lookup failures
- **Missing base cases** resulting in infinite recursion
- **Stack overflow** for deep recursion chains
- **Inefficient state transitions** causing unnecessary computation
- **Memory limit exceeded** due to excessive state storage
- **Redundant state parameters** that could be derived or eliminated
