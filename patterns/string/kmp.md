# KMP Algorithm (Knuth-Morris-Pratt)

## Category
String Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Pattern matching in strings with potential for repeated comparisons
- Finding all occurrences of a pattern in a text
- String problems requiring efficient substring search
- When naive approaches result in O(n*m) time complexity (text length n, pattern length m)
- Hard-level applications typically involve:
  - Multiple pattern searches
  - Circular/rotated strings
  - Dynamic pattern updates
  - Streaming data processing
  - Pattern matching with wildcards or variable elements

## Core Idea / Intuition
The KMP algorithm optimizes string matching by avoiding redundant comparisons when a mismatch occurs. It preprocesses the pattern to create a "partial match" table (also called "failure function" or "LPS - Longest Prefix Suffix") that tells how many characters to skip when a mismatch happens. This reduces the time complexity from O(n*m) to O(n+m).

For hard problems, we often need to extend the basic KMP with additional state tracking or combine it with other string algorithms for optimal solutions to complex string manipulation problems.

## Step-by-Step Approach
1. **Precompute LPS array**: Build table that defines longest proper prefix which is also suffix
2. **Initialize pointers**: Set text index i=0 and pattern index j=0
3. **Compare characters**: Match text[i] with pattern[j]
4. **Handle matches**: If characters match, increment both pointers
5. **Handle mismatches**: If mismatch, use LPS to determine new j value
6. **Record matches**: When j reaches pattern length, record match and continue search
7. **Repeat**: Continue until i reaches text length

## Python-style Pseudocode Template
```python
def compute_lps(pattern):
    m = len(pattern)
    lps = [0] * m  # lps[i] = longest proper prefix which is also suffix for pattern[0...i]
    
    # Length of previous longest prefix & suffix
    length = 0
    i = 1
    
    while i < m:
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            if length != 0:
                # This is the key insight of KMP
                length = lps[length - 1]
            else:
                lps[i] = 0
                i += 1
    
    return lps

def kmp_search(text, pattern):
    n, m = len(text), len(pattern)
    
    # Edge cases
    if m == 0:
        return [i for i in range(n+1)]  # Empty pattern matches everywhere
    if n < m:
        return []  # Text shorter than pattern, no matches possible
    
    # Preprocess pattern to compute LPS array
    lps = compute_lps(pattern)
    
    matches = []
    i = 0  # Index for text
    j = 0  # Index for pattern
    
    while i < n:
        # Current characters match
        if pattern[j] == text[i]:
            i += 1
            j += 1
        
        # Found complete pattern match
        if j == m:
            matches.append(i - j)  # Record start index of match
            j = lps[j - 1]  # Look for next match
        
        # Mismatch after j matches
        elif i < n and pattern[j] != text[i]:
            if j != 0:
                j = lps[j - 1]  # Use LPS to skip comparisons
            else:
                i += 1
    
    return matches
```

## Example LeetCode Hard Problems
- LC 28: Find the Index of the First Occurrence in a String (using KMP for optimal solution)
- LC 214: Shortest Palindrome
- LC 459: Repeated Substring Pattern
- LC 1392: Longest Happy Prefix
- LC 1397: Find All Good Strings

## Optimization Tips
- **LPS array computation optimizations**:
  - Compute on-the-fly during the search for single pattern searches
  - Precompute and store for multiple searches with the same pattern
- **Memory optimization**:
  - Use rolling hash combined with KMP for very long texts
  - Implement streaming KMP for huge texts that don't fit in memory
- **Multiple pattern search**:
  - Consider Aho-Corasick algorithm (extends KMP for multiple patterns)
  - Use suffix arrays for static text with many pattern queries
- **Special cases**:
  - Optimize for binary strings with bit manipulation
  - Use specialized algorithms for specific pattern types (periodic patterns, etc.)

## Common Pitfalls
- **Off-by-one errors** in the LPS computation or matching logic
- **Incorrect handling of empty patterns or texts**
- **Misunderstanding LPS array** semantics leading to incorrect updates
- **Not recognizing when simpler algorithms** are sufficient (e.g., using KMP when indexOf would work)
- **Inefficient implementations** that negate the performance advantage
- **Missing restart conditions** in the main search loop
- **Forgetting to reset pattern index** after finding a match
