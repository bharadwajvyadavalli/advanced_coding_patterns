# Bit Manipulation

## Category
Math & Bit Manipulation

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving sets, subsets, or combinations of a small number of elements
- When you need to track or manipulate binary states efficiently
- Problems where operations on individual bits are required
- Optimization problems where space complexity is critical
- Hard-level applications typically involve:
  - Complex bit operations across multiple positions simultaneously
  - Bitwise operations combined with DP or other algorithms
  - Efficient subsets/subsequence generation and processing
  - Bit-level optimizations in number-theoretic problems

## Core Idea / Intuition
Bit manipulation leverages bitwise operations to perform tasks efficiently at the binary level. Operations like AND, OR, XOR, shifting, and masking enable powerful techniques for problems involving sets, flags, or binary states. By representing data at the bit level, we can achieve significant optimizations in both time and space complexity.

For hard problems, we often combine bit manipulation with other algorithmic techniques or use advanced bit operations to solve complex problems that would be inefficient with standard approaches.

## Step-by-Step Approach
1. **Identify bit representation**: Determine how to map problem elements to bits
2. **Choose appropriate operations**: Select bitwise operations based on problem requirements
3. **Implement efficient state transitions**: Define how operations change the state
4. **Optimize bit-level algorithms**: Reduce operations using bit-level tricks
5. **Handle edge cases**: Consider boundary conditions, empty sets, overflow
6. **Combine with other algorithms**: Integrate with DP, graphs, etc. if needed

## Python-style Pseudocode Template
```python
def bit_manipulation_solution(nums):
    # Step 1: Initialize result and bit state
    result = initial_value
    state = 0  # Or other initial state
    
    # Step 2: Process elements
    for num in nums:
        # Step 3: Apply bit operations based on problem
        
        # Example: Set a bit
        state |= (1 << position)
        
        # Example: Check if bit is set
        if state & (1 << position):
            # Process when bit is set
            pass
        
        # Example: Toggle a bit
        state ^= (1 << position)
        
        # Example: Clear a bit
        state &= ~(1 << position)
        
        # Example: Extract lowest set bit
        lowest_bit = state & -state
        
        # Example: Clear lowest set bit
        state &= (state - 1)
        
        # Update result based on current state
        result = update_result(result, state)
    
    return result
```

## Example LeetCode Hard Problems
- LC 1125: Smallest Sufficient Team
- LC 1178: Number of Valid Words for Each Puzzle
- LC 1255: Maximum Score Words Formed by Letters
- LC 1611: Minimum One Bit Operations to Make Integers Zero
- LC 1659: Maximize Grid Happiness

## Optimization Tips
- **Bit manipulation operations**:
  - Count set bits: `x.bit_count()` in Python, `__builtin_popcount(x)` in C++
  - Find lowest set bit: `x & -x`
  - Clear lowest set bit: `x & (x - 1)`
  - Generate all subsets of a set: `for subset in range(1 << n)`
  - Check if power of 2: `x & (x - 1) == 0 and x > 0`
  - Set all right bits: `(1 << position) - 1`
- **Advanced techniques**:
  - Gosper's hack for generating combinations
  - Gray codes for minimal bit changes
  - Fenwick trees / Binary Indexed Trees for range queries
  - SIMD instructions for parallel bit operations
- **Memory optimization**:
  - Use bit vectors instead of boolean arrays
  - Pack multiple values into a single integer

## Common Pitfalls
- **Overflow issues** when shifting or operating on large integers
- **Confusion with operator precedence** (use parentheses liberally)
- **Sign extension** problems with arithmetic right shifts
- **Off-by-one errors** in bit positions or bit counting
- **Using bit manipulation when clearer alternatives exist**
- **Platform-dependent behavior** (32-bit vs 64-bit integers)
- **Endianness issues** when working with byte representations
