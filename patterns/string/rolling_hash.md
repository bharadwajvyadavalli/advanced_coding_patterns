# Rolling Hash

## Category
String Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Efficient substring search and comparison
- Detecting repeated substrings or patterns
- String similarity and plagiarism detection
- Working with sliding windows over strings
- Hard-level applications typically involve:
  - Multiple hash values for verification
  - Substring manipulation with complex constraints
  - Combining with other string algorithms
  - Two-dimensional rolling hash for matrix problems
  - Dynamic string modifications with efficient updates

## Core Idea / Intuition
A Rolling Hash is a hash function where the input is hashed in a window that moves through the input. When the window moves, the hash function is efficiently updated based on the added and removed elements, avoiding the need to fully recompute the hash.

The key insight is that a well-designed rolling hash function allows constant-time updates when the window shifts, enabling linear time solutions for problems that would otherwise require quadratic time. This is particularly useful for string algorithms like Rabin-Karp pattern matching.

For hard problems, rolling hash is often combined with other techniques or extended to handle complex constraints efficiently.

## Step-by-Step Approach
1. **Choose hash function**: Select an appropriate hash function with good distribution
2. **Initialize hash**: Calculate hash for the initial window
3. **Define update mechanism**: Implement efficient hash updates for window slides
4. **Slide window**: Move window through input while updating hash
5. **Handle collisions**: Implement verification for potential matches
6. **Process results**: Extract required information from matched positions
7. **Extend for problem**: Apply problem-specific modifications

## Python-style Pseudocode Template
```python
def rolling_hash(s, window_size):
    # Constants for hash function
    base = 256  # For ASCII characters
    mod = 10**9 + 7  # Large prime to reduce collisions
    
    n = len(s)
    
    # Handle edge cases
    if n == 0 or window_size > n:
        return []
    
    # Calculate hash multiplier for leading digit (base^(window_size-1))
    multiplier = 1
    for _ in range(window_size - 1):
        multiplier = (multiplier * base) % mod
    
    # Calculate initial hash value
    current_hash = 0
    for i in range(window_size):
        current_hash = (current_hash * base + ord(s[i])) % mod
    
    # Store all hash values
    hash_values = [current_hash]
    
    # Calculate rolling hash for remaining windows
    for i in range(window_size, n):
        # Remove leading digit, add trailing digit
        current_hash = ((current_hash - ord(s[i - window_size]) * multiplier) % mod + mod) % mod
        current_hash = (current_hash * base + ord(s[i])) % mod
        hash_values.append(current_hash)
    
    return hash_values

def find_repeated_substrings(s, length):
    # Find all repeated substrings of given length
    hash_to_positions = {}
    repeated = []
    
    hash_values = rolling_hash(s, length)
    
    for i, h in enumerate(hash_values):
        if h in hash_to_positions:
            # Potential match found, verify to handle collisions
            for pos in hash_to_positions[h]:
                if s[pos:pos+length] == s[i:i+length]:
                    repeated.append((i, s[i:i+length]))
                    break
            hash_to_positions[h].append(i)
        else:
            hash_to_positions[h] = [i]
    
    return repeated

def longest_repeated_substring(s):
    # Binary search for the length of longest repeated substring
    n = len(s)
    left, right = 1, n // 2
    
    result = ""
    
    while left <= right:
        mid = (left + right) // 2
        
        # Check if there's a repeated substring of length mid
        found = find_repeated_substrings(s, mid)
        
        if found:
            # Update result and search for longer substrings
            result = found[0][1]
            left = mid + 1
        else:
            # Search for shorter substrings
            right = mid - 1
    
    return result

def string_matching_rolling_hash(text, pattern):
    # Pattern matching using rolling hash (Rabin-Karp algorithm)
    t_len, p_len = len(text), len(pattern)
    
    # Base and modulus for hash function
    base = 256
    mod = 10**9 + 7
    
    # Calculate pattern hash
    pattern_hash = 0
    for i in range(p_len):
        pattern_hash = (pattern_hash * base + ord(pattern[i])) % mod
    
    # Calculate multiplier for leading digit removal
    multiplier = 1
    for _ in range(p_len - 1):
        multiplier = (multiplier * base) % mod
    
    # Initialize window hash
    window_hash = 0
    for i in range(min(t_len, p_len)):
        window_hash = (window_hash * base + ord(text[i])) % mod
    
    # Check initial window
    matches = []
    if window_hash == pattern_hash and text[:p_len] == pattern:
        matches.append(0)
    
    # Slide window and check matches
    for i in range(p_len, t_len):
        # Update hash: remove leading, add trailing
        window_hash = ((window_hash - ord(text[i - p_len]) * multiplier) % mod + mod) % mod
        window_hash = (window_hash * base + ord(text[i])) % mod
        
        # Check hash match and verify (handle collisions)
        if window_hash == pattern_hash:
            start_pos = i - p_len + 1
            if text[start_pos:start_pos + p_len] == pattern:
                matches.append(start_pos)
    
    return matches

def polynomial_rolling_hash(s):
    # A common polynomial rolling hash implementation
    p = 31  # Prime number (good for lowercase letters)
    m = 10**9 + 9  # Large prime modulus
    hash_value = 0
    p_pow = 1
    
    for c in s:
        # Add character contribution to hash
        hash_value = (hash_value + (ord(c) - ord('a') + 1) * p_pow) % m
        p_pow = (p_pow * p) % m
    
    return hash_value

def multiple_rolling_hash(s, window_size):
    # Use multiple hash functions to reduce collision probability
    # Returns a tuple of hash values for each window
    
    # Define multiple hash functions with different bases and moduli
    hash_params = [
        (257, 10**9 + 7),
        (263, 10**9 + 9),
        (271, 10**9 + 21)
    ]
    
    hash_arrays = []
    
    for base, mod in hash_params:
        # Initialize array for this hash function
        hashes = []
        
        # Calculate multiplier
        multiplier = 1
        for _ in range(window_size - 1):
            multiplier = (multiplier * base) % mod
        
        # Initial hash
        current_hash = 0
        for i in range(window_size):
            current_hash = (current_hash * base + ord(s[i])) % mod
        
        hashes.append(current_hash)
        
        # Rolling hash for remaining windows
        for i in range(window_size, len(s)):
            # Remove leading, add trailing
            current_hash = ((current_hash - ord(s[i - window_size]) * multiplier) % mod + mod) % mod
            current_hash = (current_hash * base + ord(s[i])) % mod
            hashes.append(current_hash)
        
        hash_arrays.append(hashes)
    
    # Combine hash values into tuples
    result = []
    for i in range(len(hash_arrays[0])):
        result.append(tuple(ha[i] for ha in hash_arrays))
    
    return result
```

## Example LeetCode Hard Problems
- LC 1044: Longest Duplicate Substring
- LC 1062: Longest Repeating Substring
- LC 1316: Distinct Echo Substrings
- LC 1698: Number of Distinct Substrings in a String
- LC 1923: Longest Common Subpath

## Optimization Tips
- **Hash function selection**:
  - Use multiple hash functions to reduce collision probability
  - Choose good base and modulus values (31, 37, or 257 for base; large primes for modulus)
  - Adjust based on character set (e.g., smaller base for lowercase letters only)
- **Computational optimizations**:
  - Precompute powers of the base for efficiency
  - Use modular arithmetic correctly to avoid overflow
  - Apply early termination when possible
- **Memory efficiency**:
  - Store only necessary hash values
  - Use rolling arrays if all hashes aren't needed simultaneously
  - Consider custom hash map implementations for large inputs
- **Collision handling**:
  - Implement efficient verification for potential matches
  - Use multi-hash approach to drastically reduce collision probability

## Common Pitfalls
- **Hash collisions** leading to false positives
- **Incorrect modular arithmetic** causing negative hash values
- **Numeric overflow** in hash calculations
- **Inefficient hash updates** when sliding the window
- **Poor hash function choice** leading to many collisions
- **Not verifying potential matches** to handle collisions
- **Mishandling edge cases** like empty strings or single-character inputs
