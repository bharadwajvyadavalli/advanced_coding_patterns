# Linked List Manipulation

## Category
Special Technique

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving singly or doubly linked lists
- Operations requiring pointer manipulation and node reordering
- When constant space complexity is required
- Problems with complex node relationships or patterns
- Hard-level applications typically involve:
  - Multi-level pointer manipulation
  - Merge or partition operations
  - Cycle detection and resolution
  - Complex reordering or restructuring
  - Optimizing for both time and space constraints

## Core Idea / Intuition
Linked list manipulation techniques focus on efficiently rearranging, modifying, or analyzing linked structures by directly manipulating node pointers. Unlike arrays, linked lists allow for constant-time insertion and deletion at any position given a pointer to that position, but lack random access capabilities.

The key insight for hard problems is developing a clear mental model of pointer relationships and carefully managing reference updates to avoid creating invalid structures or losing portions of the list during modifications.

For hard problems, these techniques often require multiple passes, carefully coordinated pointer updates, or auxiliary data structures to handle complex transformations efficiently.

## Step-by-Step Approach
1. **Analyze problem requirements**: Determine the type of linked list operations needed
2. **Choose traversal pattern**: Decide on single/multiple passes and traversal approach
3. **Plan pointer updates**: Establish the sequence of pointer modifications
4. **Handle edge cases**: Consider empty lists, single nodes, and boundary conditions
5. **Implement core algorithm**: Execute the node manipulations in the correct order
6. **Verify list integrity**: Ensure no nodes are lost or erroneously connected
7. **Optimize approach**: Reduce space usage or traversal passes if possible

## Python-style Pseudocode Template
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

# Reverse a linked list
def reverse_linked_list(head):
    prev = None
    curr = head
    
    while curr:
        next_temp = curr.next  # Store next node
        curr.next = prev       # Reverse the link
        prev = curr            # Move prev forward
        curr = next_temp       # Move curr forward
    
    return prev  # New head

# Find the middle of a linked list
def find_middle(head):
    if not head or not head.next:
        return head
    
    slow = head
    fast = head
    
    # Fast pointer moves twice as fast as slow pointer
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    return slow  # Middle node

# Detect cycle in a linked list
def detect_cycle(head):
    if not head or not head.next:
        return False
    
    slow = head
    fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:  # Cycle detected
            return True
    
    return False

# Find the start of the cycle
def find_cycle_start(head):
    if not head or not head.next:
        return None
    
    # Detect cycle
    slow = head
    fast = head
    has_cycle = False
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:
            has_cycle = True
            break
    
    if not has_cycle:
        return None
    
    # Find cycle start
    slow = head
    while slow != fast:
        slow = slow.next
        fast = fast.next
    
    return slow  # Start of cycle

# Merge two sorted linked lists
def merge_sorted_lists(l1, l2):
    dummy = ListNode(0)  # Dummy head
    curr = dummy
    
    while l1 and l2:
        if l1.val <= l2.val:
            curr.next = l1
            l1 = l1.next
        else:
            curr.next = l2
            l2 = l2.next
        
        curr = curr.next
    
    # Attach remaining nodes
    curr.next = l1 if l1 else l2
    
    return dummy.next

# Merge k sorted linked lists
def merge_k_sorted_lists(lists):
    # Helper function to merge two lists
    def merge_two_lists(l1, l2):
        dummy = ListNode(0)
        curr = dummy
        
        while l1 and l2:
            if l1.val <= l2.val:
                curr.next = l1
                l1 = l1.next
            else:
                curr.next = l2
                l2 = l2.next
            
            curr = curr.next
        
        curr.next = l1 if l1 else l2
        return dummy.next
    
    if not lists:
        return None
    
    # Merge lists using divide-and-conquer
    while len(lists) > 1:
        merged_lists = []
        
        for i in range(0, len(lists), 2):
            l1 = lists[i]
            l2 = lists[i + 1] if i + 1 < len(lists) else None
            
            merged_lists.append(merge_two_lists(l1, l2))
        
        lists = merged_lists
    
    return lists[0] if lists else None

# Reverse nodes in k-group
def reverse_k_group(head, k):
    # Check if there are at least k nodes
    def has_k_nodes(node, k):
        count = 0
        while node and count < k:
            node = node.next
            count += 1
        return count == k
    
    # Function to reverse k nodes starting from head
    def reverse_k_nodes(head, k):
        prev = None
        curr = head
        for _ in range(k):
            next_temp = curr.next
            curr.next = prev
            prev = curr
            curr = next_temp
        return prev, head, curr  # new_head, old_head (now tail), next_group_head
    
    dummy = ListNode(0)
    dummy.next = head
    prev_group_tail = dummy
    curr = head
    
    while curr:
        group_head = curr
        
        # Check if there are k nodes left
        if not has_k_nodes(curr, k):
            break
        
        # Reverse k nodes
        new_head, new_tail, next_group_head = reverse_k_nodes(curr, k)
        
        # Connect with previous group
        prev_group_tail.next = new_head
        
        # Connect with next group
        new_tail.next = next_group_head
        
        # Update variables for next iteration
        prev_group_tail = new_tail
        curr = next_group_head
    
    return dummy.next

# Reorder linked list: L0→L1→...→Ln-1→Ln to L0→Ln→L1→Ln-1→...
def reorder_list(head):
    if not head or not head.next:
        return head
    
    # Find the middle of the linked list
    slow = head
    fast = head
    while fast.next and fast.next.next:
        slow = slow.next
        fast = fast.next.next
    
    # Split the list into two halves
    second_half = slow.next
    slow.next = None  # Terminate first half
    
    # Reverse the second half
    prev = None
    curr = second_half
    while curr:
        next_temp = curr.next
        curr.next = prev
        prev = curr
        curr = next_temp
    second_half = prev
    
    # Merge the two halves
    first = head
    second = second_half
    
    while second:
        temp1 = first.next
        temp2 = second.next
        
        first.next = second
        second.next = temp1
        
        first = temp1
        second = temp2
    
    return head

# Remove duplicates from sorted list II (remove all nodes with duplicate values)
def delete_duplicates(head):
    dummy = ListNode(0)
    dummy.next = head
    prev = dummy
    curr = head
    
    while curr:
        # Check for duplicates
        is_duplicate = False
        while curr.next and curr.val == curr.next.val:
            curr = curr.next
            is_duplicate = True
        
        if is_duplicate:
            # Skip all duplicates
            prev.next = curr.next
        else:
            # Move prev forward
            prev = curr
        
        # Move curr forward
        curr = curr.next
    
    return dummy.next

# LRU Cache implementation using doubly linked list
class DoubleListNode:
    def __init__(self, key=0, val=0):
        self.key = key
        self.val = val
        self.prev = None
        self.next = None

class LRUCache:
    def __init__(self, capacity):
        self.capacity = capacity
        self.cache = {}  # key -> node
        
        # Initialize dummy head and tail
        self.head = DoubleListNode()
        self.tail = DoubleListNode()
        self.head.next = self.tail
        self.tail.prev = self.head
    
    def _add_node(self, node):
        # Always add node right after head
        node.prev = self.head
        node.next = self.head.next
        
        self.head.next.prev = node
        self.head.next = node
    
    def _remove_node(self, node):
        # Remove an existing node
        prev = node.prev
        new = node.next
        
        prev.next = new
        new.prev = prev
    
    def _move_to_head(self, node):
        # Move a node to the head
        self._remove_node(node)
        self._add_node(node)
    
    def _pop_tail(self):
        # Remove and return the tail node
        node = self.tail.prev
        self._remove_node(node)
        return node
    
    def get(self, key):
        if key not in self.cache:
            return -1
        
        # Update recently used
        node = self.cache[key]
        self._move_to_head(node)
        
        return node.val
    
    def put(self, key, value):
        if key in self.cache:
            # Update existing key
            node = self.cache[key]
            node.val = value
            self._move_to_head(node)
        else:
            # Add new key
            node = DoubleListNode(key, value)
            self.cache[key] = node
            self._add_node(node)
            
            # Check capacity
            if len(self.cache) > self.capacity:
                # Remove least recently used
                tail = self._pop_tail()
                del self.cache[tail.key]
```

## Example LeetCode Hard Problems
- LC 23: Merge k Sorted Lists
- LC 25: Reverse Nodes in k-Group
- LC 146: LRU Cache
- LC 460: LFU Cache
- LC 1206: Design Skiplist

## Optimization Tips
- **Pointer manipulation**:
  - Use dummy nodes to simplify edge cases
  - Implement in-place operations to minimize space usage
  - Apply multiple pointers for complex operations
- **Traversal optimizations**:
  - Use slow and fast pointers for efficient middle finding
  - Implement one-pass algorithms when possible
  - Apply tortoise and hare technique for cycle detection
- **Memory management**:
  - Reuse nodes instead of creating new ones
  - Implement constant space solutions
  - Consider custom node structures for specific problems
- **Problem-specific optimizations**:
  - Apply divide-and-conquer for large list operations
  - Use auxiliary data structures for complex transformations
  - Leverage sorting or hashing for duplicate detection

## Common Pitfalls
- **Losing references** to portions of the list during manipulation
- **Not handling edge cases** (empty list, single node, last node)
- **Infinite loops** in cyclic lists or incorrect pointer updates
- **Off-by-one errors** in counting nodes or position tracking
- **Memory leaks** in languages with manual memory management
- **Incorrect order of pointer updates** leading to premature link breaking
- **Not visualizing the list** structure before implementing changes
