# Mathematics (GCD, modulo, combinatorics)

## Category
Math & Bit Manipulation

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving number theory, combinatorics, or probability
- When direct computation would result in overflow or need for arbitrary precision
- Problems requiring mathematical properties (prime numbers, GCD, modular arithmetic)
- Optimization problems with mathematical structure
- Hard-level applications typically involve:
  - Advanced number theory concepts
  - Complex combinatorial calculations with modular arithmetic
  - Matrix exponentiation for recurrence relations
  - Mathematical optimization for efficient solutions
  - Multiple mathematical techniques combined

## Core Idea / Intuition
Mathematical problems often have elegant solutions based on number theory, combinatorics, geometry, or other mathematical principles. These solutions typically leverage fundamental properties and theorems to avoid brute force approaches, reducing time complexity from exponential or polynomial to linear or even constant time.

For hard problems, the key is identifying the underlying mathematical structure and applying the appropriate theorems, formulas, or algorithms to efficiently solve the problem.

## Step-by-Step Approach
1. **Identify mathematical structure**: Determine what branch of mathematics applies (number theory, combinatorics, etc.)
2. **Recognize patterns**: Look for mathematical patterns in the problem description
3. **Apply relevant theorems**: Use appropriate mathematical theorems or properties
4. **Implement efficient algorithms**: Apply optimized algorithms for common operations
5. **Handle edge cases**: Consider boundary conditions and special cases
6. **Manage precision and overflow**: Use modular arithmetic or appropriate data types
7. **Optimize computations**: Apply mathematical simplifications when possible

## Python-style Pseudocode Template
```python
# GCD (Euclidean algorithm)
def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

# LCM using GCD
def lcm(a, b):
    return a * b // gcd(a, b)

# Fast modular exponentiation
def pow_mod(base, exponent, modulus):
    if modulus == 1:
        return 0
    
    result = 1
    base = base % modulus
    
    while exponent > 0:
        # If exponent is odd, multiply result with base
        if exponent & 1:
            result = (result * base) % modulus
        
        # Square the base
        base = (base * base) % modulus
        # Divide exponent by 2
        exponent >>= 1
    
    return result

# Modular inverse using extended Euclidean algorithm
def mod_inverse(a, m):
    if gcd(a, m) != 1:
        return None  # Inverse doesn't exist
    
    def extended_gcd(a, b):
        if a == 0:
            return b, 0, 1
        
        gcd, x1, y1 = extended_gcd(b % a, a)
        x = y1 - (b // a) * x1
        y = x1
        
        return gcd, x, y
    
    _, x, _ = extended_gcd(a, m)
    return (x % m + m) % m

# Compute binomial coefficient with modulo
def binomial_coefficient(n, k, mod):
    # Optimization for large values
    if k > n - k:
        k = n - k
    
    result = 1
    for i in range(k):
        result = (result * (n - i)) % mod
        result = (result * mod_inverse(i + 1, mod)) % mod
    
    return result

# Prime factorization
def prime_factorization(n):
    factors = []
    
    # Check divisibility by 2
    while n % 2 == 0:
        factors.append(2)
        n //= 2
    
    # Check odd divisors
    for i in range(3, int(math.sqrt(n)) + 1, 2):
        while n % i == 0:
            factors.append(i)
            n //= i
    
    # If n is a prime greater than 2
    if n > 2:
        factors.append(n)
    
    return factors

# Matrix exponentiation for Fibonacci-like recurrences
def matrix_power(matrix, power, mod):
    n = len(matrix)
    result = [[1 if i == j else 0 for j in range(n)] for i in range(n)]
    
    while power > 0:
        if power & 1:
            result = matrix_multiply(result, matrix, mod)
        
        matrix = matrix_multiply(matrix, matrix, mod)
        power >>= 1
    
    return result

def matrix_multiply(A, B, mod):
    n = len(A)
    result = [[0 for _ in range(n)] for _ in range(n)]
    
    for i in range(n):
        for j in range(n):
            for k in range(n):
                result[i][j] = (result[i][j] + A[i][k] * B[k][j]) % mod
    
    return result
```

## Example LeetCode Hard Problems
- LC 60: Permutation Sequence
- LC 214: Shortest Palindrome (using string hashing)
- LC 372: Super Pow
- LC 878: Nth Magical Number
- LC 1622: Fancy Sequence

## Optimization Tips
- **Efficient algorithms for common operations**:
  - Use binary GCD algorithm for faster GCD computation
  - Apply fast modular exponentiation for large powers
  - Use sieve of Eratosthenes for prime generation
  - Apply matrix exponentiation for linear recurrences
- **Modular arithmetic optimizations**:
  - Precompute modular inverses for factorial calculations
  - Apply Fermat's little theorem for modular inverses of primes
  - Use Chinese remainder theorem for systems of modular equations
- **Combinatorial optimizations**:
  - Use dynamic programming for complex counting problems
  - Apply mathematical formulas instead of direct computation
  - Leverage symmetry and pattern recognition
- **Number theory tricks**:
  - Use properties of primes, GCD, LCM for simplification
  - Apply number theory theorems to reduce computation

## Common Pitfalls
- **Integer overflow** in intermediate calculations
- **Precision errors** with floating-point calculations
- **Off-by-one errors** in combinatorial formulas
- **Missing edge cases** in mathematical algorithms
- **Inefficient implementations** of mathematical functions
- **Not recognizing mathematical patterns** in problems
- **Incorrect modular arithmetic** (e.g., handling negative numbers)
