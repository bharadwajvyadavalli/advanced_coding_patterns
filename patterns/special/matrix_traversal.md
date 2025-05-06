# Matrix Traversal / Flood Fill

## Category
Special Technique

## Difficulty Level
✅ **LeetCode Hard**

## When to Use
- Processing 2D grids or matrices element by element
- Finding connected components in a grid
- Exploring paths through mazes or grid-based problems
- Implementing region filling or coloring algorithms
- Hard-level applications typically involve:
  - Multiple types of movements or complex movement rules
  - Grid transformation with changing states
  - Multi-stage traversals with interdependent paths
  - Optimization across multiple traversal patterns
  - Combining with other algorithms (DP, Union-Find, etc.)

## Core Idea / Intuition
Matrix Traversal techniques systematically visit cells in a 2D grid, often using Depth-First Search (DFS) or Breadth-First Search (BFS) to explore connected regions. Flood Fill is a specialized application that finds and transforms contiguous regions meeting specific criteria.

The key insight is that 2D grids can be treated as implicit graphs, where each cell is a node and adjacent cells are connected by edges. This enables applying graph algorithms to solve complex spatial relationship problems efficiently.

For hard problems, matrix traversal often requires combining multiple techniques, handling complex state transitions, or optimizing for large grids with specific constraints.

## Step-by-Step Approach
1. **Define traversal rules**: Establish which movements are allowed (4-directional, 8-directional, etc.)
2. **Choose traversal method**: Select DFS, BFS, or other specialized approaches
3. **Track visited cells**: Implement a mechanism to avoid revisiting cells
4. **Handle boundaries**: Ensure traversal stays within the grid
5. **Implement state changes**: Apply transformations to cells as needed
6. **Process connected components**: Handle region-based operations
7. **Optimize approach**: Reduce space usage or traversal complexity if possible

## Python-style Pseudocode Template
```python
# Common directions for 4-directional movement (up, right, down, left)
directions_4 = [(-1, 0), (0, 1), (1, 0), (0, -1)]

# Common directions for 8-directional movement (including diagonals)
directions_8 = [(-1, 0), (-1, 1), (0, 1), (1, 1), (1, 0), (1, -1), (0, -1), (-1, -1)]

# DFS matrix traversal
def dfs_matrix(matrix, row, col, visited=None):
    if visited is None:
        visited = set()
    
    rows, cols = len(matrix), len(matrix[0])
    
    # Check if current cell is valid and not visited
    if (row < 0 or row >= rows or col < 0 or col >= cols or
            (row, col) in visited or not is_valid(matrix[row][col])):
        return
    
    # Mark current cell as visited
    visited.add((row, col))
    
    # Process current cell
    process_cell(matrix, row, col)
    
    # Explore all four directions
    for dr, dc in directions_4:
        dfs_matrix(matrix, row + dr, col + dc, visited)

# BFS matrix traversal
def bfs_matrix(matrix, start_row, start_col):
    rows, cols = len(matrix), len(matrix[0])
    visited = set([(start_row, start_col)])
    queue = [(start_row, start_col)]
    
    while queue:
        row, col = queue.pop(0)
        
        # Process current cell
        process_cell(matrix, row, col)
        
        # Explore all four directions
        for dr, dc in directions_4:
            new_row, new_col = row + dr, col + dc
            
            # Check if the new cell is valid and not visited
            if (0 <= new_row < rows and 0 <= new_col < cols and
                    (new_row, new_col) not in visited and
                    is_valid(matrix[new_row][new_col])):
                visited.add((new_row, new_col))
                queue.append((new_row, new_col))

# Flood fill algorithm
def flood_fill(image, sr, sc, new_color):
    rows, cols = len(image), len(image[0])
    start_color = image[sr][sc]
    
    # If starting color is already the new color, no need to fill
    if start_color == new_color:
        return image
    
    def dfs(r, c):
        if (r < 0 or r >= rows or c < 0 or c >= cols or 
                image[r][c] != start_color):
            return
        
        # Change the color
        image[r][c] = new_color
        
        # Explore all four directions
        for dr, dc in directions_4:
            dfs(r + dr, c + dc)
    
    dfs(sr, sc)
    return image

# Count connected components in a grid
def count_islands(grid):
    if not grid or not grid[0]:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    visited = set()
    count = 0
    
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1' and (r, c) not in visited:
                bfs_island(grid, r, c, visited)
                count += 1
    
    return count

def bfs_island(grid, row, col, visited):
    rows, cols = len(grid), len(grid[0])
    queue = [(row, col)]
    visited.add((row, col))
    
    while queue:
        r, c = queue.pop(0)
        
        for dr, dc in directions_4:
            nr, nc = r + dr, c + dc
            
            if (0 <= nr < rows and 0 <= nc < cols and 
                    grid[nr][nc] == '1' and (nr, nc) not in visited):
                queue.append((nr, nc))
                visited.add((nr, nc)))

# Find the shortest path in a maze
def shortest_path_maze(maze, start, destination):
    rows, cols = len(maze), len(maze[0])
    directions = [(-1, 0), (0, 1), (1, 0), (0, -1)]  # Up, right, down, left
    
    # Use BFS to find shortest path
    queue = [(start[0], start[1], 0)]  # (row, col, distance)
    visited = set([(start[0], start[1])])
    
    while queue:
        row, col, distance = queue.pop(0)
        
        # Check if destination is reached
        if row == destination[0] and col == destination[1]:
            return distance
        
        # Explore all four directions
        for dr, dc in directions:
            new_row, new_col = row + dr, col + dc
            
            # Continue moving in the current direction until hitting a wall or boundary
            steps = 0
            while (0 <= new_row < rows and 0 <= new_col < cols and 
                   maze[new_row][new_col] == 0):
                new_row += dr
                new_col += dc
                steps += 1
            
            # Move back one step from the wall or boundary
            new_row -= dr
            new_col -= dc
            
            if (new_row, new_col) not in visited:
                visited.add((new_row, new_col))
                queue.append((new_row, new_col, distance + steps))
    
    return -1  # No path found

# Pacific-Atlantic water flow problem
def pacific_atlantic(heights):
    if not heights or not heights[0]:
        return []
    
    rows, cols = len(heights), len(heights[0])
    pacific_reachable = set()
    atlantic_reachable = set()
    
    def dfs(r, c, reachable):
        reachable.add((r, c))
        
        for dr, dc in directions_4:
            nr, nc = r + dr, c + dc
            
            if (0 <= nr < rows and 0 <= nc < cols and 
                    (nr, nc) not in reachable and 
                    heights[nr][nc] >= heights[r][c]):
                dfs(nr, nc, reachable)
    
    # Start DFS from the Pacific borders
    for c in range(cols):
        dfs(0, c, pacific_reachable)  # Top row
    for r in range(rows):
        dfs(r, 0, pacific_reachable)  # Leftmost column
    
    # Start DFS from the Atlantic borders
    for c in range(cols):
        dfs(rows - 1, c, atlantic_reachable)  # Bottom row
    for r in range(rows):
        dfs(r, cols - 1, atlantic_reachable)  # Rightmost column
    
    # Find cells that can reach both oceans
    return list(pacific_reachable.intersection(atlantic_reachable))

# Multi-source BFS (shortest path from any source to any target)
def multi_source_bfs(grid, sources, targets):
    rows, cols = len(grid), len(grid[0])
    queue = [(r, c, 0) for r, c in sources]  # (row, col, distance)
    visited = set([(r, c) for r, c in sources])
    
    while queue:
        row, col, distance = queue.pop(0)
        
        # Check if target is reached
        if (row, col) in targets:
            return distance
        
        # Explore all four directions
        for dr, dc in directions_4:
            nr, nc = row + dr, col + dc
            
            if (0 <= nr < rows and 0 <= nc < cols and 
                    (nr, nc) not in visited and 
                    is_valid(grid[nr][nc])):
                visited.add((nr, nc))
                queue.append((nr, nc, distance + 1))
    
    return -1  # No path found

# Minimum spanning tree in a grid (Prim's algorithm)
def grid_mst(grid):
    rows, cols = len(grid), len(grid[0])
    visited = set()
    
    # Use priority queue for Prim's algorithm
    # (cost, row, col)
    pq = [(0, 0, 0)]  # Start from top-left cell
    total_cost = 0
    
    while pq and len(visited) < rows * cols:
        cost, row, col = heapq.heappop(pq)
        
        if (row, col) in visited:
            continue
        
        visited.add((row, col))
        total_cost += cost
        
        for dr, dc in directions_4:
            nr, nc = row + dr, col + dc
            
            if (0 <= nr < rows and 0 <= nc < cols and 
                    (nr, nc) not in visited):
                edge_cost = calculate_cost(grid, row, col, nr, nc)
                heapq.heappush(pq, (edge_cost, nr, nc))
    
    return total_cost
```

## Example LeetCode Hard Problems
- LC 37: Sudoku Solver
- LC 51: N-Queens
- LC 329: Longest Increasing Path in a Matrix
- LC 980: Unique Paths III
- LC 1293: Shortest Path in a Grid with Obstacles Elimination

## Optimization Tips
- **Traversal selection**:
  - Use DFS for exploring all possible paths or backtracking
  - Apply BFS for shortest path problems
  - Implement bidirectional BFS for faster path finding
  - Use A* search when heuristics are available
- **Space optimizations**:
  - Modify the original grid to mark visited cells when possible
  - Use bit manipulation for visited state in small grids
  - Apply run-length encoding for sparse grids
- **Algorithmic optimizations**:
  - Implement pruning to avoid unpromising paths
  - Use dynamic programming to avoid redundant traversals
  - Apply jump point search for uniform-cost grids
  - Use Union-Find for connected components in static grids
- **Problem-specific optimizations**:
  - Precompute reachable areas for multi-query problems
  - Apply coordinate compression for large sparse grids
  - Use monotonic queues for specific constraints

## Common Pitfalls
- **Off-by-one errors** in boundary checking
- **Incorrect visited cell tracking** leading to infinite loops
- **Stack overflow** with recursive DFS on large grids
- **Memory limit exceeded** with BFS on large grids
- **Inefficient queue implementations** slowing down BFS
- **Incorrect direction handling** for specific movement rules
- **Not considering diagonal movements** when they're allowed
