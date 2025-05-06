# Greedy Algorithms

## Category
Core Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems where making locally optimal choices leads to a globally optimal solution
- Optimization problems with overlapping subproblems but without optimal substructure
- When sorting or prioritizing elements provides a clear path to the solution
- Problems involving scheduling, interval selection, or resource allocation
- Hard-level applications typically involve:
  - Multiple conflicting greedy criteria that must be balanced
  - Non-obvious greedy choice property
  - Combination with other algorithms (DP, BFS/DFS)
  - Mathematical proof required to verify correctness
  - Multi-phase greedy approaches

## Core Idea / Intuition
Greedy algorithms make the locally optimal choice at each step with the hope of finding a global optimum. Unlike dynamic programming, greedy algorithms make decisions based solely on the current state without reconsidering past choices. The key property is the "greedy choice property," which ensures that a globally optimal solution can be constructed by making locally optimal choices.

For hard problems, the greedy approach often requires careful proof of correctness, as it's not always obvious that local optimality leads to global optimality. Many hard greedy problems also require combining the greedy strategy with other techniques.

## Step-by-Step Approach
1. **Identify the optimal substructure**: Verify that the problem can be solved by making a sequence of choices
2. **Define the greedy choice**: Determine the criterion for making the locally optimal choice at each step
3. **Prove greedy choice property**: Verify (mathematically or intuitively) that local choices lead to global optimum
4. **Design algorithm structure**: Implement a way to make and apply greedy choices efficiently
5. **Optimize data structures**: Select appropriate structures to efficiently find and apply greedy choices
6. **Handle edge cases**: Consider special scenarios where the greedy approach might fail
7. **Implement solution**: Code the greedy algorithm with appropriate optimizations

## Python-style Pseudocode Template
```python
def greedy_solution(input_data):
    # Preprocess or transform input if needed
    processed_data = preprocess(input_data)
    
    # Sort or prioritize elements based on greedy criterion
    sorted_elements = sort_by_greedy_criterion(processed_data)
    
    # Initialize result and tracking variables
    result = initial_value
    current_state = initial_state
    
    # Iterate through elements in priority order
    for element in sorted_elements:
        # Check if element can be selected based on constraints
        if can_select(element, current_state):
            # Apply greedy choice
            result = apply_choice(result, element)
            current_state = update_state(current_state, element)
    
    # Return final result after all selections
    return result

def multi_phase_greedy(input_data):
    # Some hard problems require multiple greedy phases
    
    # Phase 1: Apply first greedy criterion
    intermediate_result = first_phase_greedy(input_data)
    
    # Phase 2: Apply second greedy criterion on intermediate result
    final_result = second_phase_greedy(intermediate_result)
    
    return final_result

def hybrid_greedy_dp(input_data):
    # Some hard problems combine greedy with DP
    
    # Apply greedy preprocessing
    processed_data = greedy_preprocess(input_data)
    
    # Apply DP on processed data
    result = dynamic_programming_solution(processed_data)
    
    return result
```

## Example LeetCode Hard Problems
- LC 45: Jump Game II
- LC 135: Candy
- LC 630: Course Schedule III
- LC 871: Minimum Number of Refueling Stops
- LC 1505: Minimum Possible Integer After at Most K Adjacent Swaps On Digits

## Optimization Tips
- **Efficient data structures**:
  - Use priority queues/heaps for dynamic selection of best elements
  - Use balanced binary search trees for range operations
  - Use hash maps for quick lookups and updates
- **Sorting optimizations**:
  - Choose appropriate sorting criteria for the problem
  - Use custom comparators for complex sorting needs
  - Consider partial sorting when complete sorting isn't necessary
- **Incremental updates**:
  - Maintain running state efficiently rather than recomputing
  - Use lazy evaluation when possible
- **Problem-specific optimizations**:
  - Exploit mathematical properties specific to the problem
  - Use offline preprocessing for static inputs

## Common Pitfalls
- **Assuming greedy works without proof**: Not all problems with optimal substructure can be solved greedily
- **Incorrect greedy criterion**: Selecting the wrong metric for local optimization
- **Not considering all constraints**: Missing constraints that affect choice validity
- **Inefficient implementation** of the greedy choice selection
- **Failing with edge cases**: Not handling special cases where greedy approach breaks down
- **Overlooking sorting requirements**: Not sorting or prioritizing elements correctly
- **Premature optimization**: Optimizing before ensuring the greedy approach is correct
