# Trie (Prefix Tree)

## Category
Advanced Data Structure

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving string operations, particularly prefix matching
- When you need to efficiently store and search for words in a dictionary
- Problems requiring auto-complete, spell checking, or prefix-based queries
- When there's a need to find all words with a common prefix
- Hard-level applications typically involve:
  - Complex traversal patterns or multi-criteria searches
  - Dynamic trie modifications during problem solving
  - Combining tries with other data structures (e.g., hashmap, heap)
  - Advanced operations like wildcard matching or edit distance

## Core Idea / Intuition
A Trie is a tree-like data structure where each node represents a character, and paths from root to leaf form complete words or strings. Unlike binary search trees, nodes in a trie don't store entire keys but rather parts of keys. This structure enables O(m) lookup, insertion, and deletion operations, where m is the length of the key, regardless of the number of keys in the trie.

For hard problems, we often need to augment the basic trie structure with additional information at each node, perform complex traversals, or combine tries with other algorithms for optimal solutions.

## Step-by-Step Approach
1. **Design the node structure**: Define what each node contains (character, isEndOfWord flag, children pointers, additional data)
2. **Implement basic operations**: Create functions for insertion, search, and deletion
3. **Add problem-specific functionality**: Extend the trie with methods needed for the specific problem
4. **Optimize traversal**: Define efficient ways to explore the trie based on problem requirements
5. **Handle edge cases**: Consider empty strings, non-existing keys, and other special scenarios
6. **Combine with other structures/algorithms**: If needed, integrate the trie with other techniques

## Python-style Pseudocode Template
```python
class TrieNode:
    def __init__(self):
        self.children = {}  # Map: character -> TrieNode
        self.is_end_of_word = False
        self.additional_data = None  # Problem-specific data

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
        # Set additional data if needed
    
    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word
    
    def starts_with_prefix(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
    
    # Problem-specific operations
    def problem_specific_operation(self, input):
        # Implement based on problem requirements
        pass
```

## Example LeetCode Hard Problems
- LC 212: Word Search II
- LC 336: Palindrome Pairs
- LC 425: Word Squares
- LC 642: Design Search Autocomplete System
- LC 1032: Stream of Characters

## Optimization Tips
- **Space optimization**:
  - Use array instead of hashmap for children when character set is limited (e.g., lowercase letters)
  - Implement compressed tries (similar to radix trees) for sparse character distributions
  - Use bitmap to indicate which children exist for dense tries
- **Advanced operations**:
  - Implement wildcard matching with recursive DFS
  - Use breadth-first search for level-order operations
  - Add backlinks (failure links) for Aho-Corasick algorithm in string matching
- **Memory management**:
  - Implement node deletion with proper reference cleanup
  - Use memory pools for node allocation in languages with manual memory management

## Common Pitfalls
- **Forgetting to mark end of words** leading to incorrect word validation
- **Not properly handling edge cases** like empty strings or prefixes
- **Memory overconsumption** when storing large dictionaries without compression
- **Inefficient traversal** that doesn't leverage trie structure
- **Not combining with other algorithms** when appropriate (e.g., DFS for word search)
- **Overlooking character set assumptions** (ASCII vs. Unicode)
- **Poor implementation of deletion** leading to memory leaks or dangling nodes
