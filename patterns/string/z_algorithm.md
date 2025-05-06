# Z-Algorithm

## Category
String Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Pattern matching with linear time complexity
- Finding all occurrences of a pattern in text
- String prefix-related problems
- Longest substring/subsequence matching problems
- Hard-level applications typically involve:
  - Multiple pattern matching
  - Substring analysis with complex constraints
  - Combining with other string algorithms
  - Optimizing string manipulations
  - Precomputation for string queries

## Core Idea / Intuition
The Z-Algorithm is a linear-time string matching algorithm that creates a Z-array where Z[i] represents the length of the longest substring starting at position i that is also a prefix of the string. This array is constructed in linear time using clever reuse of previously computed values, allowing for efficient string matching and analysis.

The key insight is the maintenance of a "Z-box" (a substring for which we have already computed Z-values), which allows us to reuse computation results and achieve linear time complexity for string matching.

For hard problems, the Z-Algorithm is often extended with additional processing or combined with other string algorithms to efficiently solve complex string problems.

## Step-by-Step Approach
1. **Concatenate pattern and text**: Create a string pattern + separator + text
2. **Initialize Z-array**: Set up array to store Z-values
3. **Compute Z-values**: Calculate for each position using the Z-box optimization
4. **Process Z-array**: Find matches or extract other required information
5. **Apply problem-specific logic**: Use Z-values for the particular problem
6. **Optimize Z-computation**: Use efficient implementation to achieve linear time
7. **Extract results**: Process the Z-array to solve the original problem

## Python-style Pseudocode Template
```python
def z_algorithm(s):
    n = len(s)
    z = [0] * n
    
    # First element is always 0 (no proper prefix of itself)
    
    # [L, R] define the current Z-box
    L, R = 0, 0
    
    for i in range(1, n):
        # If i is inside the Z-box, use previously computed values
        if i <= R:
            # i' is the equivalent position in the prefix
            i_prime = i - L
            
            # If the value doesn't stretch beyond the Z-box, just copy it
            if i + z[i_prime] - 1 < R:
                z[i] = z[i_prime]
                continue
            
            # Otherwise, we can only guarantee up to the end of Z-box
            z[i] = R - i + 1
        
        # Extend the match from the right boundary
        while i + z[i] < n and s[z[i]] == s[i + z[i]]:
            z[i] += 1
        
        # Update Z-box if this match extends beyond the current box
        if i + z[i] - 1 > R:
            L = i
            R = i + z[i] - 1
    
    return z

def string_matching_with_z(pattern, text):
    # Concatenate pattern and text with a unique separator
    combined = pattern + "$" + text
    z_values = z_algorithm(combined)
    
    pattern_length = len(pattern)
    matches = []
    
    # Find all occurrences where Z-value equals pattern length
    for i in range(pattern_length + 1, len(combined)):
        if z_values[i] == pattern_length:
            # Convert to original text index
            matches.append(i - pattern_length - 1)
    
    return matches

def longest_common_prefix(strings):
    # Find the longest common prefix of multiple strings
    if not strings:
        return ""
    
    reference = strings[0]
    min_length = min(len(s) for s in strings)
    
    for i in range(1, len(strings)):
        # Create combined string for Z-algorithm
        combined = strings[i] + "$" + reference
        z = z_algorithm(combined)
        
        # Find the longest prefix match
        max_prefix_length = 0
        for j in range(len(strings[i]) + 1, len(combined)):
            max_prefix_length = max(max_prefix_length, min(z[j], j - len(strings[i]) - 1))
        
        # Update reference to be the current common prefix
        reference = reference[:max_prefix_length]
        
        if not reference:
            return ""
    
    return reference

def string_repetition_detection(s):
    # Detect if string is a repetition of a substring
    n = len(s)
    z = z_algorithm(s)
    
    for i in range(1, n):
        # If i divides string length and z[i] equals remaining length
        if n % i == 0 and z[i] == n - i:
            return True, i  # String is repeated, substring length is i
    
    return False, n  # String is not a repetition

def longest_palindromic_prefix(s):
    # Find the longest palindromic prefix
    # Create reversed string for comparison
    reversed_s = s[::-1]
    
    # Concatenate and find Z-values
    combined = s + "$" + reversed_s
    z = z_algorithm(combined)
    
    # Check for palindromic prefixes
    max_length = 0
    n = len(s)
    
    for i in range(n + 1, 2 * n + 1):
        # Check if equivalent position in reverse string has matching prefix
        index_in_reversed = 2 * n - i
        
        if i + z[i] == 2 * n + 1:
            # This is a palindromic prefix
            palindrome_length = z[i]
            max_length = max(max_length, palindrome_length)
    
    return s[:max_length]
```

## Example LeetCode Hard Problems
- LC 214: Shortest Palindrome
- LC 336: Palindrome Pairs
- LC 1392: Longest Happy Prefix
- LC 1397: Find All Good Strings
- LC 1960: Maximum Product of the Length of Two Palindromic Substrings

## Optimization Tips
- **Z-box optimization**:
  - Proper maintenance of [L, R] boundaries for Z-box
  - Efficient reuse of previously computed Z-values
- **Computational efficiency**:
  - Avoid redundant character comparisons
  - Early termination when possible
  - Use efficient string concatenation
- **Memory optimization**:
  - In-place construction of Z-array
  - Avoid unnecessary temporary strings
  - Combine pattern matching logic with Z-computation
- **Combined with other algorithms**:
  - Use with KMP for complex pattern matching
  - Combine with Manacher's for palindrome problems
  - Integrate with rolling hash for sliding window problems

## Common Pitfalls
- **Incorrect Z-box maintenance**: Improper updating of [L, R] boundaries
- **Off-by-one errors**: Incorrect indexing in the Z-array
- **Inefficient string operations**: Redundant concatenations or comparisons
- **Misinterpreting Z-values**: Incorrect mapping to the original problem
- **Improper separator selection**: Using a character that might appear in the strings
- **Not handling empty strings**: Failing to account for this edge case
- **Overlooking optimization opportunities**: Not reusing computed Z-values effectively
