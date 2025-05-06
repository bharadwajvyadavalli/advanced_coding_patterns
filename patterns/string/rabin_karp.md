# Rabin-Karp Algorithm

## Category
String Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Efficient pattern matching in strings
- Finding multiple occurrences of patterns in a text
- When multiple pattern search is needed
- String similarity and plagiarism detection
- Hard-level applications typically involve:
  - Multiple pattern matching simultaneously
  - 2D pattern matching in matrices
  - Substring problems with complex conditions
  - Rolling hash for sliding window problems
  - Combining with other string algorithms

## Core Idea / Intuition
The Rabin-Karp algorithm uses a rolling hash function to efficiently search for pattern matches in a text. Instead of comparing characters directly, it computes hash values for the pattern and text windows, dramatically reducing the number of character comparisons needed. When hash values match, it verifies the match by checking the actual substring.

The key insight is that a properly designed rolling hash function allows computing the hash of the next window in O(1) time from the previous window, enabling linear time complexity for string matching.

For hard problems, Rabin-Karp is often extended to handle multiple patterns or combined with other string algorithms for optimal solutions to complex string manipulation problems.

## Step-by-Step Approach
1. **Choose hash function**: Select an appropriate rolling hash function
2. **Compute pattern hash**: Calculate hash value for the pattern
3. **Initialize window**: Compute hash for the first text window
4. **Slide window**: Update hash value efficiently when window shifts
5. **Check potential matches**: Verify matches when hash values are equal
6. **Handle hash collisions**: Implement proper collision detection
7. **Process results**: Collect matches or derived information

## Python-style Pseudocode Template
```python
def rabin_karp(text, pattern):
    # Constants for the hash function
    d = 256  # Number of characters in the input alphabet
    q = 10**9 + 7  # A large prime number
    
    n = len(text)
    m = len(pattern)
    
    # Handle edge cases
    if m > n:
        return []
    if m == 0:
        return list(range(n + 1))
    
    # Calculate hash value for pattern and first window of text
    pattern_hash = 0
    text_hash = 0
    
    # Calculate the value of h = d^(m-1) % q
    h = 1
    for i in range(m - 1):
        h = (h * d) % q
    
    # Calculate initial hash values
    for i in range(m):
        pattern_hash = (d * pattern_hash + ord(pattern[i])) % q
        text_hash = (d * text_hash + ord(text[i])) % q
    
    # List to store all matches
    matches = []
    
    # Slide the pattern over text one by one
    for i in range(n - m + 1):
        # Check if hash values match
        if pattern_hash == text_hash:
            # Check for actual character match (handle collisions)
            match = True
            for j in range(m):
                if text[i + j] != pattern[j]:
                    match = False
                    break
            
            if match:
                matches.append(i)
        
        # Calculate hash value for next window
        if i < n - m:
            # Remove leading digit, add trailing digit
            text_hash = (d * (text_hash - ord(text[i]) * h) + ord(text[i + m])) % q
            
            # Handle negative values
            if text_hash < 0:
                text_hash += q
    
    return matches

def multi_pattern_rabin_karp(text, patterns):
    # Hash set to store pattern hashes
    pattern_hashes = {}
    
    # Constants for the hash function
    d = 256
    q = 10**9 + 7
    
    # Find the length of the smallest pattern
    min_len = min(len(p) for p in patterns)
    
    # Precompute hashes for all patterns
    for pattern in patterns:
        pattern_hash = 0
        for i in range(len(pattern)):
            pattern_hash = (d * pattern_hash + ord(pattern[i])) % q
        
        # Store pattern and its hash
        if pattern_hash in pattern_hashes:
            pattern_hashes[pattern_hash].append(pattern)
        else:
            pattern_hashes[pattern_hash] = [pattern]
    
    # Initialize result for each pattern
    results = {pattern: [] for pattern in patterns}
    
    # Perform Rabin-Karp for each possible pattern length
    for pattern_len in set(len(p) for p in patterns):
        h = 1
        for i in range(pattern_len - 1):
            h = (h * d) % q
        
        # Initialize window hash
        window_hash = 0
        for i in range(min(pattern_len, len(text))):
            window_hash = (d * window_hash + ord(text[i])) % q
        
        # Slide window
        for i in range(len(text) - pattern_len + 1):
            # Check if current hash matches any pattern hash
            if window_hash in pattern_hashes:
                # Check actual patterns with this hash
                for pattern in pattern_hashes[window_hash]:
                    if len(pattern) == pattern_len and text[i:i+pattern_len] == pattern:
                        results[pattern].append(i)
            
            # Update window hash
            if i < len(text) - pattern_len:
                window_hash = (d * (window_hash - ord(text[i]) * h) + ord(text[i + pattern_len])) % q
                if window_hash < 0:
                    window_hash += q
    
    return results

def rolling_hash_for_substrings(s, length):
    # Used for problems like finding duplicate substrings
    seen = {}
    result = []
    
    # Constants for hash function
    d = 26  # Assuming lowercase English letters
    q = 10**9 + 7
    
    # Compute initial hash and h = d^(length-1) % q
    h = 1
    for i in range(length - 1):
        h = (h * d) % q
    
    curr_hash = 0
    for i in range(length):
        curr_hash = (curr_hash * d + (ord(s[i]) - ord('a'))) % q
    
    # Add first window
    seen[curr_hash] = 0
    
    # Slide window
    for i in range(1, len(s) - length + 1):
        # Remove leading character
        curr_hash = (curr_hash - h * (ord(s[i-1]) - ord('a'))) % q
        # Add trailing character
        curr_hash = (curr_hash * d + (ord(s[i+length-1]) - ord('a'))) % q
        # Handle negative values
        if curr_hash < 0:
            curr_hash += q
        
        # Check if hash seen before
        if curr_hash in seen:
            # Verify actual substring match (handle collisions)
            prev_index = seen[curr_hash]
            if s[prev_index:prev_index+length] == s[i:i+length]:
                result.append(s[i:i+length])
        else:
            seen[curr_hash] = i
    
    return result
```

## Example LeetCode Hard Problems
- LC 1044: Longest Duplicate Substring
- LC 1062: Longest Repeating Substring
- LC 1147: Longest Chunked Palindrome Decomposition
- LC 1392: Longest Happy Prefix
- LC 1923: Longest Common Subpath

## Optimization Tips
- **Hash function selection**:
  - Use multiple hash functions to reduce collisions
  - Choose prime modulus to minimize hash collisions
  - Adjust base value based on character set (e.g., 26 for lowercase letters)
- **Collision handling**:
  - Use separate verification step for potential matches
  - Implement multi-hash approach for critical applications
- **Computational optimizations**:
  - Precompute power values for faster hash calculations
  - Use integer operations instead of modular exponentiation
  - Apply bit manipulation tricks for power-of-2 hash functions
- **Memory optimizations**:
  - Store only necessary hash values
  - Use hash maps for efficient lookup
  - Apply sliding window techniques to avoid redundant computations

## Common Pitfalls
- **Hash collisions** leading to false positives
- **Numeric overflow** in hash calculations
- **Incorrect rolling hash implementation** when updating window
- **Inefficient verification** of potential matches
- **Improper modulus operations** with negative numbers
- **Not accounting for empty patterns** or other edge cases
- **Using inappropriate hash function** for the specific character set
