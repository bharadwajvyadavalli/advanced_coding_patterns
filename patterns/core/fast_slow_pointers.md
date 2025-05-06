# Fast and Slow Pointers (Cycle Detection)

## Category
Core Pattern

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Problems involving linked lists or sequences where cycle detection is needed
- Finding the middle element of a linked list in one pass
- Determining if a sequence has a cycle and finding the start of the cycle
- Problems requiring detection of a loop or determining loop length
- Hard-level applications typically involve:
  - Multiple cycles or complex cycle structures
  - Finding specific nodes within cycles
  - Optimizing memory usage for large sequences
  - Combined with other techniques (e.g., tortoise and hare algorithm)

## Core Idea / Intuition
The Fast and Slow Pointers pattern (also known as Floyd's Cycle-Finding Algorithm or the "Tortoise and Hare" algorithm) uses two pointers moving at different speeds through a sequence. The slow pointer moves one step at a time, while the fast pointer moves two steps. If there's a cycle, the fast pointer will eventually catch up to the slow pointer. Once they meet, we can find the start of the cycle by resetting one pointer to the beginning and moving both pointers at the same speed.

For hard problems, we often need to extend this basic idea with additional logic to detect multiple cycles, find specific nodes within cycles, or combine it with other algorithms for optimal solutions.

## Step-by-Step Approach
1. **Initialize pointers**: Set slow and fast pointers to the start of the sequence
2. **Move pointers**: Advance slow pointer by one step and fast pointer by two steps
3. **Check for meeting**: If pointers meet, a cycle exists
4. **Find cycle start** (optional): Reset one pointer to the beginning, move both at same speed until they meet
5. **Calculate cycle length** (optional): After detecting the cycle, count steps for one pointer to return to the meeting point
6. **Handle edge cases**: Empty lists, single-node lists, no cycles

## Python-style Pseudocode Template
```python
def detect_cycle(head):
    # Edge cases
    if not head or not head.next:
        return None  # No cycle possible
    
    # Step 1: Initialize pointers
    slow = head
    fast = head
    
    # Step 2: Move pointers until they meet or fast reaches end
    while fast and fast.next:
        slow = slow.next           # Move slow one step
        fast = fast.next.next      # Move fast two steps
        
        # Step 3: Check if pointers meet (cycle detected)
        if slow == fast:
            # Step 4: Find the start of the cycle
            entry_pointer = head
            while entry_pointer != slow:
                entry_pointer = entry_pointer.next
                slow = slow.next
            
            return entry_pointer  # Return start of cycle
    
    return None  # No cycle found

def find_cycle_length(head):
    # First detect cycle using fast and slow
    slow = fast = head
    has_cycle = False
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:
            has_cycle = True
            break
    
    if not has_cycle:
        return 0
    
    # Step 5: Calculate cycle length
    cycle_length = 1
    current = slow.next
    
    while current != slow:
        cycle_length += 1
        current = current.next
    
    return cycle_length
```

## Example LeetCode Hard Problems
- LC 287: Find the Duplicate Number
- LC 142: Linked List Cycle II
- LC 457: Circular Array Loop
- LC 708: Insert into a Sorted Circular Linked List
- LC 2095: Delete the Middle Node of a Linked List

## Optimization Tips
- **Avoid additional data structures**: The beauty of this algorithm is its O(1) space complexity
- **Early termination checks**: 
  - Detect impossible cases (e.g., empty lists, lists with < 3 nodes)
  - Handle special cases separately
- **Extending for multiple cycles**:
  - Mark visited nodes in a separate pass
  - Use additional pointers to track different cycle properties
- **Combining with other techniques**:
  - Hash tables for complex node properties
  - Reversal techniques for specific node operations

## Common Pitfalls
- **Off-by-one errors** when calculating cycle length or finding cycle start
- **Not handling edge cases** properly (empty lists, single-node lists, no cycles)
- **Infinite loops** due to incorrect pointer movement
- **Incorrect application** of the cycle detection algorithm to problem variations
- **Modifying the original list** when not permitted
- **Failing to reset pointers correctly** when finding the cycle start
- **Over-complicating the solution** by adding unnecessary steps or data structures
