# Sweep Line Algorithm

## Category
Special Technique

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Geometric problems involving intersections, overlaps, or areas
- Interval-based problems with start and end points
- Problems requiring processing events in sorted order
- When naive approaches would require checking all pairs (O(n²))
- Hard-level applications typically involve:
  - Multiple types of events or complex event handling
  - Dynamic data structures during the sweep
  - Handling edge cases at event boundaries
  - Combining with other algorithms for complex problems
  - Multi-dimensional sweep line problems

## Core Idea / Intuition
The Sweep Line Algorithm processes geometric problems by simulating a line sweeping across the plane, handling events (like line segment endpoints or intersections) as they are encountered. By sorting all events and processing them in order, the algorithm transforms a two-dimensional problem into a sequence of one-dimensional problems, often reducing time complexity from O(n²) to O(n log n).

The key insight is that many geometric relationships only need to be considered at specific events, and maintaining an appropriate data structure during the sweep allows for efficient processing without checking all possible combinations.

For hard problems, the sweep line algorithm often needs to be combined with other data structures and algorithms to handle complex scenarios and edge cases.

## Step-by-Step Approach
1. **Identify events**: Determine what constitutes an event in the problem
2. **Define event processing**: Establish how to handle each type of event
3. **Sort events**: Arrange events in the order they will be encountered
4. **Initialize data structures**: Set up necessary structures for the sweep
5. **Process events sequentially**: Handle each event according to its type
6. **Update sweep line state**: Modify data structures as the line moves
7. **Extract results**: Collect information during or after the sweep

## Python-style Pseudocode Template
```python
def sweep_line(segments):
    # Step 1: Create events
    events = []
    
    for i, segment in enumerate(segments):
        # Store start and end events with appropriate flags
        # Format: (x-coordinate, event_type, segment_id, y-coordinate)
        start_x, start_y, end_x, end_y = segment
        
        # Ensure start is before end in x-direction
        if start_x > end_x:
            start_x, start_y, end_x, end_y = end_x, end_y, start_x, start_y
        
        events.append((start_x, 0, i, start_y))  # 0 for start event
        events.append((end_x, 1, i, end_y))      # 1 for end event
    
    # Step 2: Sort events by x-coordinate, then by event type
    events.sort()
    
    # Step 3: Initialize data structures
    active_segments = set()  # Track segments intersecting the sweep line
    result = []  # Store results (depends on problem)
    
    # Step 4: Process events
    for event in events:
        x, event_type, segment_id, y = event
        
        if event_type == 0:  # Start event
            # Check for intersections with active segments
            for active_id in active_segments:
                if segments_intersect(segments[segment_id], segments[active_id]):
                    result.append((segment_id, active_id))
            
            # Add current segment to active set
            active_segments.add(segment_id)
        
        else:  # End event
            # Remove segment from active set
            active_segments.remove(segment_id)
    
    return result

# Helper function to check if two line segments intersect
def segments_intersect(segment1, segment2):
    # Implementation depends on problem specifics
    pass

# Example: Find all intersections between line segments
def find_intersections(segments):
    # Sort endpoints by x-coordinate
    events = []
    
    for i, (x1, y1, x2, y2) in enumerate(segments):
        events.append((min(x1, x2), 0, i))  # Start event
        events.append((max(x1, x2), 1, i))  # End event
    
    events.sort()
    
    active = set()
    intersections = set()
    
    for x, event_type, segment_id in events:
        if event_type == 0:  # Start event
            # Check for intersections with active segments
            for active_id in active:
                seg1 = segments[segment_id]
                seg2 = segments[active_id]
                
                if do_segments_intersect(seg1, seg2):
                    # Find intersection point
                    intersection = compute_intersection(seg1, seg2)
                    intersections.add(intersection)
            
            active.add(segment_id)
        else:  # End event
            active.remove(segment_id)
    
    return intersections

# Example: Calculate area covered by rectangles
def rectangle_area(rectangles):
    # Extract vertical edges
    events = []
    
    for i, (x1, y1, x2, y2) in enumerate(rectangles):
        events.append((x1, 0, y1, y2))  # Start edge
        events.append((x2, 1, y1, y2))  # End edge
    
    events.sort()
    
    # Initialize sweep line status
    active_ranges = []
    prev_x = events[0][0]
    total_area = 0
    
    for x, event_type, y1, y2 in events:
        # Calculate area between prev_x and current x
        width = x - prev_x
        if width > 0:
            # Calculate height covered by active ranges
            height = calculate_covered_height(active_ranges)
            total_area += width * height
        
        # Update active ranges
        if event_type == 0:  # Start edge
            active_ranges.append((y1, y2))
        else:  # End edge
            active_ranges.remove((y1, y2))
        
        prev_x = x
    
    return total_area

def calculate_covered_height(ranges):
    if not ranges:
        return 0
    
    # Sort by lower bound
    points = []
    for y1, y2 in ranges:
        points.append((y1, 0))  # Start point
        points.append((y2, 1))  # End point
    
    points.sort()
    
    # Scan through points to find covered length
    count = 0
    covered = 0
    prev_y = points[0][0]
    
    for y, point_type in points:
        # If count > 0, the range [prev_y, y] is covered
        if count > 0:
            covered += y - prev_y
        
        # Update counter and prev_y
        if point_type == 0:  # Start point
            count += 1
        else:  # End point
            count -= 1
        
        prev_y = y
    
    return covered

# Example: Skyline problem
def get_skyline(buildings):
    # Extract critical points (start and end of buildings)
    events = []
    
    for left, right, height in buildings:
        events.append((left, -height, right))  # Negative height for start event
        events.append((right, 0, 0))          # Zero height for end event
    
    events.sort()
    
    # Initialize result and max heap for heights
    result = []
    heights = [0]  # Ground level
    prev_max_height = 0
    
    for x, height, right in events:
        if height < 0:  # Start event
            # Add building height to max heap
            heapq.heappush(heights, height)
        else:  # End event
            # Remove corresponding building height
            # This requires finding height in the heap, which is inefficient
            # In practice, use a more efficient data structure or lazy removal
            heights.remove(-right)
            heapq.heapify(heights)
        
        # Check if maximum height changed
        current_max_height = -heights[0]
        if current_max_height != prev_max_height:
            result.append([x, current_max_height])
            prev_max_height = current_max_height
    
    return result
```

## Example LeetCode Hard Problems
- LC 218: The Skyline Problem
- LC 391: Perfect Rectangle
- LC 850: Rectangle Area II
- LC 1851: Minimum Interval to Include Each Query
- LC 2158: Amount of New Area Painted Each Day

## Optimization Tips
- **Efficient event handling**:
  - Use appropriate event types and sorting criteria
  - Process start events before end events at the same position
  - Handle special cases (vertical lines, overlapping endpoints)
- **Data structure selection**:
  - Use self-balancing binary search trees for dynamic range queries
  - Apply segment trees or BITs for range updates
  - Implement lazy propagation for deferred operations
- **Computational geometry optimizations**:
  - Use robust geometric primitives for intersection tests
  - Apply coordinate compression for large coordinate ranges
  - Implement efficient line segment intersection algorithms
- **Problem-specific optimizations**:
  - Exploit monotonicity when possible
  - Apply divide-and-conquer for recursive sweep line
  - Use plane sweep in multiple directions for complex problems

## Common Pitfalls
- **Incorrect event ordering** leading to processing errors
- **Handling edge cases** at event boundaries (e.g., coincident endpoints)
- **Floating-point precision issues** in geometric calculations
- **Inefficient active segment representation** causing slow updates
- **Missing event types** that affect the sweep line state
- **Improper data structure selection** for maintaining the sweep line
- **Overlooking degenerate cases** (vertical lines, point segments, etc.)
