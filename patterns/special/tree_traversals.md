# Tree Traversals & BST Techniques

## Category
Special Technique

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving binary trees or binary search trees
- When tree structure needs to be analyzed, modified, or traversed
- For implementing ordered operations or range queries
- When leveraging properties of balanced trees
- Hard-level applications typically involve:
  - Complex tree transformations
  - Non-standard traversal patterns
  - Advanced BST operations
  - Balancing techniques
  - Multi-criteria tree operations

## Core Idea / Intuition
Tree traversal algorithms provide systematic ways to visit every node in a tree structure. For Binary Search Trees (BSTs), additional properties like the ordering of nodes (where left child < parent < right child) enable efficient search, insertion, deletion, and range operations.

The key insight for hard problems is recognizing when to leverage specific tree properties and choosing the appropriate traversal strategy. For BSTs, understanding the in-order traversal relationship with sorted sequences is particularly powerful for complex operations.

For hard problems, these techniques often need to be combined with other algorithms or extended with additional state tracking to efficiently solve complex tree-based problems.

## Step-by-Step Approach
1. **Identify tree properties**: Determine if the tree is a binary tree, BST, balanced, or has special structures
2. **Choose traversal strategy**: Select the appropriate traversal method (pre-order, in-order, post-order, level-order)
3. **Define node processing**: Establish how to handle each node during traversal
4. **Track necessary state**: Maintain any additional information needed during traversal
5. **Apply tree-specific operations**: Implement algorithms leveraging tree properties
6. **Handle edge cases**: Consider empty trees, single nodes, and degenerate trees
7. **Optimize approach**: Apply techniques like Morris traversal for constant space if needed

## Python-style Pseudocode Template
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

# Recursive Traversal Methods
def preorder_traversal(root):
    if not root:
        return []
    
    result = [root.val]  # Process current node first
    result.extend(preorder_traversal(root.left))  # Then left subtree
    result.extend(preorder_traversal(root.right))  # Then right subtree
    
    return result

def inorder_traversal(root):
    if not root:
        return []
    
    result = inorder_traversal(root.left)  # Left subtree first
    result.append(root.val)  # Then current node
    result.extend(inorder_traversal(root.right))  # Then right subtree
    
    return result

def postorder_traversal(root):
    if not root:
        return []
    
    result = postorder_traversal(root.left)  # Left subtree first
    result.extend(postorder_traversal(root.right))  # Then right subtree
    result.append(root.val)  # Then current node
    
    return result

# Iterative Traversals
def iterative_preorder(root):
    if not root:
        return []
    
    result = []
    stack = [root]
    
    while stack:
        node = stack.pop()
        result.append(node.val)
        
        # Push right first so left is processed first (LIFO)
        if node.right:
            stack.append(node.right)
        if node.left:
            stack.append(node.left)
    
    return result

def iterative_inorder(root):
    result = []
    stack = []
    curr = root
    
    while curr or stack:
        # Reach the leftmost node
        while curr:
            stack.append(curr)
            curr = curr.left
        
        # Process current node
        curr = stack.pop()
        result.append(curr.val)
        
        # Move to right subtree
        curr = curr.right
    
    return result

def iterative_postorder(root):
    if not root:
        return []
    
    result = []
    stack = [(root, False)]  # (node, visited_right_child)
    
    while stack:
        node, visited_right = stack.pop()
        
        if visited_right:
            # Both children have been processed
            result.append(node.val)
        else:
            # Process this node after its children
            stack.append((node, True))
            
            # Push right then left (so left is processed first)
            if node.right:
                stack.append((node.right, False))
            if node.left:
                stack.append((node.left, False))
    
    return result

def level_order_traversal(root):
    if not root:
        return []
    
    result = []
    queue = [root]
    
    while queue:
        level_size = len(queue)
        current_level = []
        
        for _ in range(level_size):
            node = queue.pop(0)
            current_level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(current_level)
    
    return result

# Morris Traversal (O(1) space inorder traversal)
def morris_inorder(root):
    result = []
    curr = root
    
    while curr:
        if not curr.left:
            # No left child, process current and move right
            result.append(curr.val)
            curr = curr.right
        else:
            # Find the inorder predecessor
            pre = curr.left
            while pre.right and pre.right != curr:
                pre = pre.right
            
            if not pre.right:
                # Make current as right child of its inorder predecessor
                pre.right = curr
                curr = curr.left
            else:
                # Restore the tree (remove the temporary link)
                pre.right = None
                result.append(curr.val)
                curr = curr.right
    
    return result

# BST Operations
def bst_search(root, target):
    if not root:
        return None
    
    if root.val == target:
        return root
    elif root.val > target:
        return bst_search(root.left, target)
    else:
        return bst_search(root.right, target)

def bst_insert(root, val):
    if not root:
        return TreeNode(val)
    
    if val < root.val:
        root.left = bst_insert(root.left, val)
    elif val > root.val:
        root.right = bst_insert(root.right, val)
    
    return root

def bst_delete(root, key):
    if not root:
        return None
    
    if key < root.val:
        root.left = bst_delete(root.left, key)
    elif key > root.val:
        root.right = bst_delete(root.right, key)
    else:
        # Node with only one child or no child
        if not root.left:
            return root.right
        elif not root.right:
            return root.left
        
        # Node with two children
        # Get the inorder successor (smallest in right subtree)
        successor = find_min(root.right)
        root.val = successor.val
        
        # Delete the inorder successor
        root.right = bst_delete(root.right, successor.val)
    
    return root

def find_min(node):
    current = node
    while current.left:
        current = current.left
    return current

# Range Query in BST
def range_query(root, low, high):
    result = []
    
    def dfs(node):
        if not node:
            return
        
        # Pruning: if current node is smaller than low, skip left subtree
        if node.val >= low:
            dfs(node.left)
        
        # Process node if it's in range
        if low <= node.val <= high:
            result.append(node.val)
        
        # Pruning: if current node is larger than high, skip right subtree
        if node.val <= high:
            dfs(node.right)
    
    dfs(root)
    return result

# Convert Sorted Array to Balanced BST
def sorted_array_to_bst(nums):
    if not nums:
        return None
    
    mid = len(nums) // 2
    
    # Create root with middle element
    root = TreeNode(nums[mid])
    
    # Recursively build left and right subtrees
    root.left = sorted_array_to_bst(nums[:mid])
    root.right = sorted_array_to_bst(nums[mid+1:])
    
    return root

# Check if a binary tree is a valid BST
def is_valid_bst(root):
    def validate(node, lower=float('-inf'), upper=float('inf')):
        if not node:
            return True
        
        if node.val <= lower or node.val >= upper:
            return False
        
        # Check if left subtree nodes are < current
        # Check if right subtree nodes are > current
        return (validate(node.left, lower, node.val) and
                validate(node.right, node.val, upper))
    
    return validate(root)
```

## Example LeetCode Hard Problems
- LC 99: Recover Binary Search Tree
- LC 124: Binary Tree Maximum Path Sum
- LC 297: Serialize and Deserialize Binary Tree
- LC 685: Redundant Connection II
- LC 968: Binary Tree Cameras

## Optimization Tips
- **Traversal selection**:
  - Use pre-order for creating a copy or prefix notation
  - Use in-order for BST to get elements in sorted order
  - Use post-order for deleting a tree or postfix notation
  - Use level-order for breadth-first applications
- **Space optimization**:
  - Apply Morris traversal for O(1) space requirement
  - Use iterative approaches with explicit stack control
  - Implement parent pointers for threaded traversals
- **BST-specific optimizations**:
  - Leverage ordering property for efficient searches
  - Use self-balancing techniques for degenerate cases
  - Apply range-based pruning for partial tree traversals
- **Problem-specific optimizations**:
  - Combine DFS with memoization for repeated subproblems
  - Use specialized tree structures (segment trees, interval trees)
  - Apply lazy propagation for range updates

## Common Pitfalls
- **Not considering all node types** (leaf, internal, root) in processing
- **Incorrect recursion base cases** leading to null pointer exceptions
- **Inefficient traversal choice** for the specific problem
- **Not leveraging BST properties** when applicable
- **Incorrect handling of duplicate values** in BST operations
- **Not accounting for tree balancing** in performance-critical applications
- **Stack overflow** with recursion on very deep trees
