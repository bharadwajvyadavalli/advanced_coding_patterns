# Prefix Sum / Difference Array

## Category
Math & Bit Manipulation

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving range queries or range updates on arrays
- When frequent subarray sum calculations are needed
- Problems requiring efficient computation of subarrays with specific properties
- When analyzing cumulative effects or running totals
- Hard-level applications typically involve:
  - Multi-dimensional prefix sums (2D or higher)
  - Combining prefix sums with other techniques (binary search, DP)
  - Complex conditions on subarrays or submatrices
  - Difference arrays for range updates with efficient queries
  - Prefix XOR or other non-standard accumulation operations

## Core Idea / Intuition
Prefix Sum transforms an array into its cumulative sum, where each element is the sum of all preceding elements including itself. This allows constant-time calculation of any subarray sum by simple subtraction: sum(arr[i:j]) = prefix[j] - prefix[i-1]. 

Difference Array is the inverse operation, storing the differences between adjacent elements. It allows O(1) range updates and can be converted back to the original array with a cumulative sum.

For hard problems, these techniques are often combined with other algorithms or extended to multiple dimensions to efficiently solve complex range-based problems.

## Step-by-Step Approach
1. **Construct prefix sum/difference array**: Precompute cumulative sum or differences
2. **Process queries/updates**: Use the precomputed arrays for efficient operations
3. **Apply relevant formulas**: Use appropriate math for specific calculations
4. **Combine with other techniques**: Integrate with binary search, hash maps, etc.
5. **Extend to multiple dimensions**: Use 2D or 3D prefix sums for matrix problems
6. **Optimize space usage**: Reduce dimensions when possible
7. **Extract results**: Derive final answer from transformed data

## Python-style Pseudocode Template
```python
# 1D Prefix Sum
def build_prefix_sum(arr):
    n = len(arr)
    prefix = [0] * (n + 1)  # 1-indexed for easier calculations
    
    for i in range(n):
        prefix[i + 1] = prefix[i] + arr[i]
    
    return prefix

def query_range_sum(prefix, left, right):
    # Sum of arr[left...right] using prefix sum
    return prefix[right + 1] - prefix[left]

# 1D Difference Array
def build_difference_array(arr):
    n = len(arr)
    diff = [0] * (n + 1)
    
    diff[0] = arr[0]
    for i in range(1, n):
        diff[i] = arr[i] - arr[i-1]
    
    return diff

def update_range(diff, left, right, value):
    # Add value to arr[left...right]
    diff[left] += value
    diff[right + 1] -= value

def reconstruct_array(diff, n):
    arr = [0] * n
    arr[0] = diff[0]
    
    for i in range(1, n):
        arr[i] = arr[i-1] + diff[i]
    
    return arr

# 2D Prefix Sum
def build_2d_prefix_sum(matrix):
    if not matrix or not matrix[0]:
        return [[]]
    
    m, n = len(matrix), len(matrix[0])
    prefix = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(m):
        for j in range(n):
            prefix[i+1][j+1] = (
                prefix[i+1][j] +       # Cell to the left
                prefix[i][j+1] -       # Cell above
                prefix[i][j] +         # Remove double-counted area
                matrix[i][j]           # Add current cell
            )
    
    return prefix

def query_submatrix_sum(prefix, row1, col1, row2, col2):
    # Sum of submatrix from (row1,col1) to (row2,col2) inclusive
    return (
        prefix[row2+1][col2+1] -       # Full area
        prefix[row2+1][col1] -         # Subtract left area
        prefix[row1][col2+1] +         # Subtract top area
        prefix[row1][col1]             # Add back doubly-subtracted area
    )

# Prefix XOR and other operations
def build_prefix_xor(arr):
    n = len(arr)
    prefix_xor = [0] * (n + 1)
    
    for i in range(n):
        prefix_xor[i + 1] = prefix_xor[i] ^ arr[i]
    
    return prefix_xor

def query_range_xor(prefix_xor, left, right):
    # XOR of arr[left...right]
    return prefix_xor[right + 1] ^ prefix_xor[left]
```

## Example LeetCode Hard Problems
- LC 1074: Number of Submatrices That Sum to Target
- LC 1444: Number of Ways of Cutting a Pizza
- LC 1478: Allocate Mailboxes
- LC 1622: Fancy Sequence
- LC 2281: Sum of Total Strength of Wizards

## Optimization Tips
- **Space optimization**:
  - Use 1D arrays for row-by-row 2D prefix sum calculations when memory is limited
  - Reuse the original array when possible to avoid extra space
- **Computational optimizations**:
  - Use inclusive vs. exclusive prefix sums based on query patterns
  - Apply binary indexed trees for dynamic prefix sums with updates
  - Combine difference arrays with segment trees for complex range operations
- **Problem-specific optimizations**:
  - Use prefix sum of monotonic functions for certain problems
  - Apply hash maps with prefix sums for subarray counting problems
  - Compress coordinates for sparse arrays or matrices
- **Algorithm combinations**:
  - Prefix sum + binary search for optimization problems
  - Prefix sum + hash map for counting subarrays with specific properties
  - Prefix sum + two pointers for sliding window problems

## Common Pitfalls
- **Off-by-one errors** in prefix sum indices (0-indexed vs. 1-indexed)
- **Incorrect initialization** of the prefix or difference arrays
- **Boundary handling errors** in range queries or updates
- **Overflow issues** with large cumulative sums
- **Not recognizing when prefix sums are applicable** (missing optimization opportunities)
- **Inefficient implementation** of multi-dimensional prefix sums
- **Redundant calculations** that negate the performance advantage
