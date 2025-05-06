# Reservoir Sampling

## Category
Special Technique

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Selecting a random sample from a data stream of unknown size
- When memory constraints prevent storing the entire dataset
- Choosing k elements from n items with equal probability (1/n) for each item
- Processing large datasets with limited memory
- Hard-level applications typically involve:
  - Weighted reservoir sampling
  - Distributed reservoir sampling
  - Sampling with additional constraints
  - Combining with other algorithms for complex problems
  - Optimizing for specific probability distributions

## Core Idea / Intuition
Reservoir Sampling is an algorithm for randomly selecting k samples from a stream of data with unknown or infinite size, ensuring each element has an equal probability of being selected. The algorithm maintains a "reservoir" of k elements and, as new elements arrive, probabilistically decides whether to replace existing elements based on carefully calculated probabilities.

The key insight is that by decreasing the probability of replacement as more elements are processed, the algorithm can maintain uniform sampling without knowing the total number of elements in advance.

For hard problems, reservoir sampling often needs to be extended to handle weights, distributed data, or combined with other algorithms to meet complex requirements.

## Step-by-Step Approach
1. **Initialize reservoir**: Fill the first k elements into the reservoir
2. **Process stream**: For each subsequent element, decide whether to include it
3. **Calculate probabilities**: Determine replacement probability for each new element
4. **Apply replacements**: Replace reservoir elements according to calculated probabilities
5. **Maintain invariants**: Ensure the sampling properties are preserved
6. **Handle special cases**: Account for edge cases and constraints
7. **Return sample**: Provide the final reservoir as the result

## Python-style Pseudocode Template
```python
import random

# Standard Reservoir Sampling (select k elements from a stream)
def reservoir_sampling(stream, k):
    # Initialize reservoir with first k elements
    reservoir = []
    for i in range(k):
        if stream.has_next():
            reservoir.append(stream.next())
        else:
            break
    
    # Process remaining elements
    i = k
    while stream.has_next():
        item = stream.next()
        i += 1
        
        # Randomly decide whether to include current element
        j = random.randint(0, i - 1)
        if j < k:
            reservoir[j] = item
    
    return reservoir

# Weighted Reservoir Sampling (Algorithm A-Res)
def weighted_reservoir_sampling(stream, weights, k):
    # Initialize reservoir and weights
    reservoir = []
    weights_keys = []
    
    # Process stream
    i = 0
    while stream.has_next():
        item = stream.next()
        weight = weights[i]
        
        # Calculate key = random() ^ (1/weight)
        key = random.random() ** (1.0 / weight)
        
        # Initial filling of the reservoir
        if i < k:
            reservoir.append(item)
            weights_keys.append((key, i))
        else:
            # Find minimum key in reservoir
            min_key, min_idx = min(weights_keys)
            
            # Replace if current key is larger
            if key > min_key:
                reservoir[min_idx] = item
                weights_keys[min_idx] = (key, i)
                
                # Re-sort the keys (use a heap in practice)
                weights_keys.sort()
        
        i += 1
    
    return reservoir

# Reservoir Sampling for Linked Lists
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def linked_list_sampling(head, k):
    # Initialize reservoir
    reservoir = [0] * k
    curr = head
    
    # Fill reservoir with first k elements
    for i in range(k):
        if curr:
            reservoir[i] = curr.val
            curr = curr.next
        else:
            break
    
    # Process remaining elements
    i = k
    while curr:
        # Randomly decide whether to include current element
        j = random.randint(0, i)
        if j < k:
            reservoir[j] = curr.val
        
        i += 1
        curr = curr.next
    
    return reservoir

# Distributed Reservoir Sampling
def distributed_reservoir_sampling(streams, k):
    # Assume streams is a list of data streams
    n_streams = len(streams)
    
    # Initialize reservoirs for each stream
    local_reservoirs = []
    local_counts = []
    
    for i in range(n_streams):
        # Sample from each stream
        stream = streams[i]
        local_reservoir = reservoir_sampling(stream, k)
        local_count = stream.count()  # Number of elements in stream
        
        local_reservoirs.append(local_reservoir)
        local_counts.append(local_count)
    
    # Combine local reservoirs
    total_count = sum(local_counts)
    final_reservoir = []
    
    # Sample from local reservoirs with appropriate weights
    for i in range(n_streams):
        stream_weight = local_counts[i] / total_count
        samples_to_take = int(k * stream_weight)
        
        # Randomly select samples_to_take elements from local_reservoirs[i]
        selected_indices = random.sample(range(k), samples_to_take)
        final_reservoir.extend([local_reservoirs[i][j] for j in selected_indices])
    
    # Adjust if we didn't get exactly k elements
    while len(final_reservoir) > k:
        final_reservoir.pop()
    
    while len(final_reservoir) < k:
        # Add more elements randomly from local reservoirs
        stream_idx = random.randint(0, n_streams - 1)
        item_idx = random.randint(0, k - 1)
        final_reservoir.append(local_reservoirs[stream_idx][item_idx])
    
    return final_reservoir

# Algorithm L: Efficient Sample of k Items from n Items
def algorithm_l(n, k):
    # Sample k items from range(n) without replacement
    result = []
    for i in range(n):
        # Probability that i is selected
        if random.random() < k / (n - i):
            result.append(i)
            k -= 1
        
        if k == 0:
            break
    
    return result
```

## Example LeetCode Hard Problems
- LC 382: Linked List Random Node
- LC 398: Random Pick Index
- LC 519: Random Flip Matrix
- LC 528: Random Pick with Weight
- LC 710: Random Pick with Blacklist

## Optimization Tips
- **Efficient algorithm selection**:
  - Use Algorithm R for standard reservoir sampling
  - Apply Algorithm A-Res for weighted sampling
  - Use Algorithm L for efficient sampling from a known range
- **Data structure optimizations**:
  - Use min-heap for maintaining weighted keys
  - Apply bit manipulation for compact representation
  - Implement efficient stream iterators
- **Probability optimizations**:
  - Precompute or cache random values when applicable
  - Use alias method for weighted sampling
  - Apply skip-ahead techniques to avoid processing all elements
- **Memory optimizations**:
  - Store indices instead of actual elements when possible
  - Use in-place modifications when appropriate
  - Implement batch processing for efficiency

## Common Pitfalls
- **Incorrect probability calculations** leading to biased sampling
- **Off-by-one errors** in element counting or array indexing
- **Poor random number generator quality** affecting sampling distribution
- **Memory overflow** for large reservoirs
- **Inefficient stream processing** causing performance issues
- **Numerical stability issues** in weighted sampling
- **Not handling edge cases** (empty streams, k larger than stream size)
