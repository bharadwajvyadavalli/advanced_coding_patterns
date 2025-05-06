# Manacher's Algorithm

## Category
String Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Finding all palindromic substrings in a string
- Determining the longest palindromic substring
- Problems involving palindrome properties
- Efficient palindrome analysis of large strings
- Hard-level applications typically involve:
  - Multiple palindrome constraints
  - Palindromes with specific properties
  - Combining with other string algorithms
  - Optimizing palindrome-based operations
  - Solving complex dynamic string problems

## Core Idea / Intuition
Manacher's Algorithm finds all palindromic substrings of a string in linear time, which is a significant improvement over the naive O(n²) approach. It does this by cleverly reusing previously computed information about palindromes to avoid redundant comparisons.

The key insight is the concept of a palindrome's "center" and "radius," and the observation that when processing a new position, we can leverage information from already processed palindromes. The algorithm handles both odd and even-length palindromes by inserting special characters between each original character.

For hard problems, Manacher's Algorithm often needs to be combined with additional processing or other algorithms to solve complex string problems efficiently.

## Step-by-Step Approach
1. **Transform the string**: Insert special characters to handle both odd and even-length palindromes
2. **Initialize arrays**: Set up array to store palindrome radii
3. **Process centers**: Calculate palindrome radii for each center position
4. **Reuse information**: Leverage symmetry around palindrome centers
5. **Expand palindromes**: Grow palindromes when symmetry doesn't apply
6. **Apply problem-specific logic**: Use palindrome information for the particular problem
7. **Extract results**: Process palindrome data to solve the original problem

## Python-style Pseudocode Template
```python
def manachers_algorithm(s):
    # Transform the string to handle both odd and even-length palindromes
    # Insert '#' between characters and at boundaries
    # Example: "abc" -> "^#a#b#c#$"
    # '^' and '$' are sentinels to avoid boundary checks
    transformed = "^"
    for c in s:
        transformed += "#" + c
    transformed += "#$"
    
    n = len(transformed)
    
    # Array to store palindrome radii (excluding the center)
    p = [0] * n
    
    # Variables to track the center and right boundary of the rightmost palindrome
    center = 0
    right = 0
    
    # Process each position
    for i in range(1, n - 1):
        # Initialize p[i] using symmetry if possible
        if right > i:
            # Mirror of i with respect to center
            mirror = 2 * center - i
            
            # Initialize p[i] as the minimum of:
            # 1. The distance to right boundary
            # 2. The radius of the mirrored position
            p[i] = min(right - i, p[mirror])
        
        # Expand palindrome centered at position i
        while transformed[i + p[i] + 1] == transformed[i - p[i] - 1]:
            p[i] += 1
        
        # Update center and right boundary if this palindrome extends beyond the current rightmost palindrome
        if i + p[i] > right:
            center = i
            right = i + p[i]
    
    # Find the maximum palindrome length and its center
    max_len = 0
    max_center = 0
    
    for i in range(1, n - 1):
        if p[i] > max_len:
            max_len = p[i]
            max_center = i
    
    # Extract the longest palindromic substring
    start = (max_center - max_len) // 2  # Integer division
    end = start + max_len
    
    return s[start:end], p

def count_palindromic_substrings(s):
    # Find the number of palindromic substrings
    _, p = manachers_algorithm(s)
    
    # Each value in p represents a palindrome's radius
    # Sum of radii gives the total number of palindromic substrings
    # Subtract the added special characters in counting
    count = 0
    
    for radius in p:
        count += (radius + 1) // 2
    
    return count

def all_palindromic_substrings(s):
    # Get all palindromic substrings with their positions
    _, p = manachers_algorithm(s)
    
    palindromes = []
    transformed = "^" + "#".join(s) + "#$"
    
    for i in range(1, len(transformed) - 1):
        center = i // 2
        
        # Handle odd and even-length palindromes
        is_even = (i % 2 == 0)
        
        for j in range(p[i]):
            radius = (j + 1) // 2
            
            if is_even:
                # Even-length palindrome
                start = center - radius
                end = center + radius
            else:
                # Odd-length palindrome
                start = center - radius
                end = center + radius + 1
            
            if start >= 0 and end <= len(s):
                palindromes.append((start, s[start:end]))
    
    return palindromes

def minimum_cuts_palindrome_partitioning(s):
    # Minimum cuts needed to partition string into palindromes
    n = len(s)
    
    # First find all palindromic information using Manacher's
    _, p = manachers_algorithm(s)
    
    # Create a 2D table to track palindromes
    # is_palindrome[i][j] = True if s[i...j] is a palindrome
    is_palindrome = [[False] * n for _ in range(n)]
    
    # Populate is_palindrome using p array
    transformed = "^" + "#".join(s) + "#$"
    
    for i in range(1, len(transformed) - 1):
        center = i // 2
        is_even = (i % 2 == 0)
        
        for j in range(p[i]):
            radius = (j + 1) // 2
            
            if is_even:
                # Even-length palindrome
                start = center - radius
                end = center + radius - 1
            else:
                # Odd-length palindrome
                start = center - radius
                end = center + radius
            
            if start >= 0 and end < n:
                is_palindrome[start][end] = True
    
    # DP to find minimum cuts
    # cuts[i] = minimum cuts needed for s[0...i]
    cuts = [i for i in range(n)]
    
    for i in range(n):
        if is_palindrome[0][i]:
            cuts[i] = 0
            continue
        
        for j in range(i):
            if is_palindrome[j+1][i]:
                cuts[i] = min(cuts[i], cuts[j] + 1)
    
    return cuts[n-1]
```

## Example LeetCode Hard Problems
- LC 5: Longest Palindromic Substring
- LC 214: Shortest Palindrome
- LC 336: Palindrome Pairs
- LC 647: Palindromic Substrings
- LC 1960: Maximum Product of the Length of Two Palindromic Substrings

## Optimization Tips
- **String transformation optimization**:
  - Avoid extra space by using indices directly in the original string
  - Implement calculations that work directly with the untransformed string
- **Computation efficiency**:
  - Early termination when applicable
  - Avoid redundant palindrome checks
  - Cache repeated operations
- **Memory optimization**:
  - Store only necessary information for the specific problem
  - Use bit manipulation for compact representation of short palindromes
- **Combining with other algorithms**:
  - Integrate with dynamic programming for optimization problems
  - Use with hash functions for faster string comparisons
  - Combine with KMP or Z-algorithm for complex pattern matching

## Common Pitfalls
- **Off-by-one errors** in palindrome center and radius calculations
- **Incorrect handling of odd vs. even-length palindromes**
- **Transformation boundary issues** when adding special characters
- **Miscalculating original string indices** from transformed positions
- **Inefficient expansion** beyond known palindrome boundaries
- **Overlooking symmetry properties** that could optimize computation
- **Not handling edge cases** (empty strings, single characters, etc.)
