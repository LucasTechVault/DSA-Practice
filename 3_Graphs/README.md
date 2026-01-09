# Graph Algorithms

## Grid Linear Scan Patterns

### 1. Island Perimeter

**Problem:**
You are given a row x col grid representing a map where `grid[i][j] = 1` represents land and `grid[i][j] = 0` represents water.

Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).

The island doesn't have "lakes", meaning the water inside isn't connected to the water around the island. One cell is a square with side length 1.

Return the perimeter of the island.

Example:

```
Input: grid = [
    [1,1,0,0],
    [1,0,0,0],
    [1,1,1,0],
    [0,0,1,1]
]

Output: 18
2
```

**Intuition:**

```
- Just need to iterate every cell once
- if landcell -> perimeter += 4
- Perform 2 further checks after adding 4
    - top (boundary + land check)
    - left (boundary + land check)
    why top & left?
        - because eventually end btm right

```

**Code:**

```
# O(M*N) Time | O(1) Space
for r in range(rows):
    for c in range(cols):
        if grid[r][c] == 1:
            perimeter += 4
            # If land is above, they share an edge. Remove 2 (1 from each).
            if r > 0 and grid[r-1][c] == 1:
                perimeter -= 2
            # If land is to the left, they share an edge. Remove 2.
            if c > 0 and grid[r][c-1] == 1:
                perimeter -= 2
```
