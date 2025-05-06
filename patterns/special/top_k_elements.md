# Top-K Elements / Bucket Sort / Counting Sort

## Category
Special Technique

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Finding k largest/smallest elements in a collection
- Sorting arrays with a limited range of values
- Frequency-based problems with bounded frequencies
- When O(n log n) comparison-based sorting is not optimal
- Hard-level applications typically involve:
  - Dynamic updates to the top-k elements
  - Complex scoring or ranking functions
  - Multi-criteria top-k selection
  - Streaming or online algorithms
  - Optimizing for space and time constraints simultaneously

## Core Idea / Intuition
The Top-K Elements pattern efficiently finds or maintains the k largest/smallest elements in a collection, typically using a heap data structure which provides O(log k) operations. For sorting arrays with limited value ranges, Bucket Sort and Counting Sort provide linear time O(n) solutions by exploiting the distribution of values rather than relying on element comparisons.

For hard problems, these techniques often need modifications to handle complex scoring mechanisms, dynamic updates, or multiple constraints. The key is selecting the right approach based on the problem's specific requirements and constraints.

## Step-by-Step Approach
1. **Analyze value distribution**: Determine if limited range or frequency-based approach is applicable
2. **Choose appropriate technique**:
   - Heap for general top-k problems (O(n log k))
   - Quick select for one-time kth element selection (O(n) average)
   - Counting sort for small integer ranges (O(n + range))
   - Bucket sort for uniformly distributed values (O(n) average)
3. **Implement core algorithm**: Set up data structures and main processing
4. **Handle edge cases**: Consider empty collections, duplicates, ties
5. **Optimize for constraints**: Tune for time/space requirements
6. **Process results**: Extract and format the final answer

## Python-style Pseudocode Template
```python
# Top-K Elements using Min-Heap (for largest elements)
def find_top_k_largest(nums, k):
    # Use a min-heap of size k
    min_heap = []
    
    for num in nums:
        if len(min_heap) < k:
            # Heap not full, add element
            heapq.heappush(min_heap, num)
        elif num > min_heap[0]:
            # Replace smallest element in heap
            heapq.heappushpop(min_heap, num)
    
    # Return the k largest elements
    return min_heap

# Top-K Elements using Max-Heap (for smallest elements)
def find_top_k_smallest(nums, k):
    # Use a max-heap of size k (negate values for max-heap)
    max_heap = []
    
    for num in nums:
        if len(max_heap) < k:
            # Heap not full, add element
            heapq.heappush(max_heap, -num)
        elif -num > max_heap[0]:
            # Replace largest element in heap
            heapq.heappushpop(max_heap, -num)
    
    # Return the k smallest elements
    return [-x for x in max_heap]

# Quick Select Algorithm for K-th largest element
def quick_select(nums, k):
    # Convert to 0-indexed k (kth largest = (n-k)th smallest)
    k = len(nums) - k
    
    def partition(left, right, pivot_index):
        pivot = nums[pivot_index]
        # Move pivot to the end
        nums[pivot_index], nums[right] = nums[right], nums[pivot_index]
        
        # Move elements smaller than pivot to the left
        store_index = left
        for i in range(left, right):
            if nums[i] <= pivot:
                nums[store_index], nums[i] = nums[i], nums[store_index]
                store_index += 1
        
        # Move pivot to its final place
        nums[store_index], nums[right] = nums[right], nums[store_index]
        
        return store_index
    
    def select(left, right, k_smallest):
        # Base case: array contains only one element
        if left == right:
            return nums[left]
        
        # Select a random pivot
        pivot_index = random.randint(left, right)
        
        # Partition around the pivot
        pivot_index = partition(left, right, pivot_index)
        
        # The pivot is in its final sorted position
        if k_smallest == pivot_index:
            return nums[k_smallest]
        elif k_smallest < pivot_index:
            # Search in the left subarray
            return select(left, pivot_index - 1, k_smallest)
        else:
            # Search in the right subarray
            return select(pivot_index + 1, right, k_smallest)
    
    return select(0, len(nums) - 1, k)

# Counting Sort for arrays with small integer range
def counting_sort(arr):
    # Determine range of elements
    min_val, max_val = min(arr), max(arr)
    range_size = max_val - min_val + 1
    
    # Initialize counting array
    count = [0] * range_size
    
    # Count occurrences of each element
    for num in arr:
        count[num - min_val] += 1
    
    # Reconstruct sorted array
    sorted_arr = []
    for i in range(range_size):
        sorted_arr.extend([i + min_val] * count[i])
    
    return sorted_arr

# Bucket Sort for uniformly distributed values
def bucket_sort(arr, num_buckets=10):
    if not arr:
        return arr
    
    # Find min and max values
    min_val, max_val = min(arr), max(arr)
    
    # Create empty buckets
    bucket_range = (max_val - min_val) / num_buckets
    buckets = [[] for _ in range(num_buckets)]
    
    # Distribute elements into buckets
    for num in arr:
        # Handle edge case for max value
        bucket_index = min(int((num - min_val) / bucket_range), num_buckets - 1)
        buckets[bucket_index].append(num)
    
    # Sort individual buckets (typically using insertion sort for small buckets)
    for i in range(num_buckets):
        buckets[i].sort()
    
    # Concatenate buckets
    result = []
    for bucket in buckets:
        result.extend(bucket)
    
    return result

# Top-K Frequent Elements
def top_k_frequent(nums, k):
    # Count frequencies
    frequency = Counter(nums)
    
    # Bucket sort based on frequencies
    # bucket[i] contains all elements with frequency i
    bucket = [[] for _ in range(len(nums) + 1)]
    
    for num, freq in frequency.items():
        bucket[freq].append(num)
    
    # Extract top k frequent elements
    result = []
    for i in range(len(nums), 0, -1):
        for num in bucket[i]:
            result.append(num)
            if len(result) == k:
                return result
    
    return result
```

## Example LeetCode Hard Problems
- LC 215: Kth Largest Element in an Array
- LC 295: Find Median from Data Stream
- LC 347: Top K Frequent Elements
- LC 632: Smallest Range Covering Elements from K Lists
- LC 895: Maximum Frequency Stack

## Optimization Tips
- **Heap optimizations**:
  - Use a min-heap for top-k largest elements (size k)
  - Use a max-heap for top-k smallest elements (size k)
  - Replace the heap's top element only when necessary
- **Quick select optimizations**:
  - Use random pivots to avoid worst-case performance
  - Apply median-of-medians for guaranteed O(n) time
  - Early pruning when possible
- **Counting/bucket sort optimizations**:
  - Use sparse representations for large ranges
  - Adjust bucket size based on data distribution
  - Apply counting sort within buckets for hybrid approaches
- **Problem-specific optimizations**:
  - Maintain running top-k for streaming data
  - Use efficient data structures for frequency tracking
  - Exploit problem constraints for memory optimization

## Common Pitfalls
- **Using inappropriate techniques** for the given value range or distribution
- **Inefficient heap operations** that degrade performance
- **Not handling duplicates or ties** correctly
- **Memory overflow** with counting sort for large ranges
- **Uneven bucket distribution** leading to worst-case performance
- **Incorrect k-to-index mapping** in selection algorithms
- **Not recognizing when simpler alternatives** work better (e.g., partial sort)
