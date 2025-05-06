# Backtracking

## Category
Core Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems requiring finding all possible solutions or combinations
- Constraint satisfaction problems
- Solving puzzles (like Sudoku, N-Queens)
- Combinatorial optimization where brute force is necessary
- Path finding with constraints and multiple solutions
- Hard-level applications typically involve:
  - Complex state spaces with many variables
  - Multiple constraints that must be satisfied simultaneously
  - Optimal solution selection from many valid candidates
  - Efficient pruning to make the search tractable
  - State representations that minimize redundant search

## Core Idea / Intuition
Backtracking is an algorithmic technique that incrementally builds candidates for solutions and abandons ("backtracks") when it determines that the candidate cannot be extended to a valid solution. It's essentially a depth-first search through a state space, with early pruning of branches that cannot lead to valid solutions.

For hard problems, the key to effective backtracking is efficient state representation and aggressive pruning techniques that eliminate large portions of the search space early.

## Step-by-Step Approach
1. **Define the state space**: Clearly identify what constitutes a partial solution
2. **Define constraints**: Establish conditions for valid solutions and pruning criteria
3. **Define the goal state**: Determine when a complete valid solution is found
4. **Design recursive structure**: Implement function to explore one decision at a time
5. **Apply constraints**: Prune branches that violate constraints as early as possible
6. **Backtrack**: Undo the current choice and try alternatives
7. **Collect results**: Store valid solutions when goal state is reached

## Python-style Pseudocode Template
```python
def backtrack(state, candidates, result):
    # Check if current state is a valid complete solution
    if is_complete(state):
        result.append(state.copy())  # Add deep copy of solution
        return
    
    # Get current position/options to fill based on state
    options = get_valid_options(state, candidates)
    
    for option in options:
        # Try this option if it's valid
        if is_valid(state, option):
            # Make a choice
            apply_option(state, option)
            
            # Recursively solve rest of the problem
            backtrack(state, candidates, result)
            
            # Undo the choice (backtrack)
            undo_option(state, option)

def solve_problem(input_data):
    result = []
    initial_state = initialize_state(input_data)
    backtrack(initial_state, input_data, result)
    return result

# Problem-specific helper functions
def is_complete(state):
    # Return True if state represents a complete solution
    pass

def get_valid_options(state, candidates):
    # Return list of valid options for current decision point
    # May include pruning logic to reduce options
    pass

def is_valid(state, option):
    # Check if adding this option to state maintains validity
    pass

def apply_option(state, option):
    # Modify state to include this option
    pass

def undo_option(state, option):
    # Restore state to before this option was applied
    pass
```

## Example LeetCode Hard Problems
- LC 37: Sudoku Solver
- LC 51: N-Queens
- LC 212: Word Search II
- LC 980: Unique Paths III
- LC 1655: Distribute Repeating Integers

## Optimization Tips
- **State representation optimization**:
  - Use efficient data structures for state (bitsets, arrays instead of lists)
  - Minimize copying of states between recursive calls
- **Pruning techniques**:
  - Forward checking: anticipate future constraint violations
  - Constraint propagation: use current choice to limit future options
  - Symmetry breaking: avoid exploring symmetric solutions
  - Least-constraining value: choose options that preserve most future flexibility
- **Ordering heuristics**:
  - Most-constrained variable: select variables with fewest valid options first
  - Most-constraining variable: select variables that constrain others the most
- **Memoization**:
  - Cache results for previously seen substates (top-down DP)
  - Especially useful for overlapping subproblems

## Common Pitfalls
- **Inefficient state representation** leading to slow operations or excessive memory
- **Insufficient pruning** causing exploration of unnecessary branches
- **Not making deep copies** of solutions when storing results
- **Incorrect backtracking** - not properly undoing choices
- **Stack overflow** for deeply nested backtracking
- **Redundant constraint checking** that should be optimized
- **Missing base cases** or termination conditions
