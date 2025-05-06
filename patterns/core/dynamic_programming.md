# Dynamic Programming (DP)

## Category
Core Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems exhibiting optimal substructure and overlapping subproblems
- Optimization problems (maximization, minimization)
- Counting problems (number of ways to achieve something)
- Problems asking for the best/fewest/maximum/minimum result
- Hard-level applications typically involve:
  - Multi-dimensional state spaces
  - Complex state transitions
  - Non-obvious state representation
  - Space optimization requirements
  - Combining DP with other techniques (greedy, binary search)

## Core Idea / Intuition
Dynamic Programming breaks down complex problems into simpler overlapping subproblems and solves each subproblem only once, storing the results for future use. The two key properties that suggest DP are:
1. **Optimal Substructure**: Optimal solution can be constructed from optimal solutions of its subproblems
2. **Overlapping Subproblems**: The same subproblems are solved multiple times

For hard problems, the challenge often lies in correctly identifying the state representation, transition functions, and optimizing for space complexity.

## Step-by-Step Approach
1. **Identify DP characteristics**: Verify optimal substructure and overlapping subproblems
2. **Define state representation**: Determine variables needed to represent a subproblem
3. **Establish recurrence relation**: Define how to transition between states
4. **Identify base cases**: Define simplest subproblems with known answers
5. **Decide implementation approach**: Choose top-down (memoization) or bottom-up (tabulation)
6. **Optimize space complexity**: Reduce dimensions when possible
7. **Implement solution**: Code the DP approach with appropriate data structures
8. **Trace and test**: Verify with examples and edge cases

## Python-style Pseudocode Template
```python
# Top-down approach with memoization
def solve_top_down(input_data):
    # Initialize memoization cache
    memo = {}
    
    def dp(state):
        # Return if state already computed
        if state in memo:
            return memo[state]
        
        # Base cases
        if is_base_case(state):
            return base_case_value(state)
        
        # Recurrence relation
        result = initial_value  # Depends on problem (min/max/sum)
        
        for next_state in get_next_states(state):
            sub_result = dp(next_state)
            result = combine_results(result, sub_result)
        
        # Store result in memo and return
        memo[state] = result
        return result
    
    # Start DP from initial state
    return dp(initial_state(input_data))

# Bottom-up approach with tabulation
def solve_bottom_up(input_data):
    # Initialize DP table
    dp_table = initialize_table(input_data)
    
    # Fill in base cases
    for base_state in get_base_states(input_data):
        dp_table[base_state] = base_case_value(base_state)
    
    # Fill DP table in order of subproblem dependency
    for state in get_states_in_order(input_data):
        for prev_state in get_prev_states(state):
            dp_table[state] = combine_results(
                dp_table[state],
                transition(dp_table[prev_state], prev_state, state)
            )
    
    # Return final state result
    return dp_table[final_state(input_data)]

# Space-optimized bottom-up approach (when only recent states matter)
def solve_space_optimized(input_data):
    # Initialize reduced DP storage (often just previous row/state)
    prev_states = initialize_prev_states(input_data)
    
    # Iterate through states, maintaining only necessary previous state information
    for state in get_states_in_order(input_data):
        current_state = calculate_current_state(prev_states, state)
        prev_states = update_prev_states(prev_states, current_state, state)
    
    # Extract result from final state
    return extract_result(prev_states)
```

## Example LeetCode Hard Problems
- LC 10: Regular Expression Matching
- LC 44: Wildcard Matching
- LC 72: Edit Distance
- LC 115: Distinct Subsequences
- LC 1463: Cherry Pickup II

## Optimization Tips
- **State representation**:
  - Use tuples as keys for memoization (faster than strings)
  - Use integers or bit manipulation for states when possible
  - Eliminate redundant state variables
- **Space optimization**:
  - Reduce dimensionality when only recent states matter
  - Use rolling arrays for 1D or 2D problems
  - Reuse the same array when direction of computation allows
- **Computation order**:
  - Ensure states are processed in correct dependency order
  - Process base cases first
  - Combine DP with divide-and-conquer for further optimization
- **Memory management**:
  - Clear memo cache for large input sizes in iterative scenarios
  - Use weak references or LRU caches for very large state spaces

## Common Pitfalls
- **Incorrect state definition** that doesn't capture all necessary information
- **Missing or incorrect base cases** leading to wrong results or infinite recursion
- **Incorrect recurrence relation** that doesn't properly connect subproblems
- **Not handling edge cases** (empty input, single element, etc.)
- **Inefficient state traversal order** causing unnecessary computation
- **Memory limit exceeded** due to inefficient state representation
- **Time limit exceeded** due to redundant calculations or inappropriate approach
