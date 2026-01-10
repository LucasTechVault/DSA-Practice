# Graph Algorithms

## Grid Linear Scan Pattern

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

### 2. Flood Fill

**Problem:**
You are given an image represented by an `m x n` grid of integers image, where `image[i][j]` represents the pixel value of the image. You are also given three integers `sr, sc, and color`. Your task is to perform a flood fill on the image starting from the pixel `image[sr][sc]`.

To perform a flood fill:

Begin with the starting pixel and change its color to color.
Perform the same process for each pixel that is directly adjacent (pixels that share a side with the original pixel, either horizontally or vertically) and shares the same color as the starting pixel.
Keep repeating this process by checking neighboring pixels of the updated pixels and `modifying their color` if it matches the original color of the starting pixel.
The process stops when there are no more adjacent pixels of the original color to update.
Return the modified image after performing the flood fill.

**Example:**

```
Input: image = [[1,1,1],[1,1,0],[1,0,1]]
sr = 1, sc = 1, color = 2

Output: [[2,2,2],[2,2,0],[2,0,1]]
```

**Strategy**:

```
1. check if image[sr][sc] == color
    - no change needed, return image
    - else to change color = image[sr][sc]

2. Perform BFS from image[sr][sc]
    - init deque([(sr, sc)])
    - init directions array
    - find nr, nc
    - perform boundary check + color check
    - if img[nr][nc] == to change color
        - img[nr][nc] = color
        - append ((nr, nc)) to queue
```

**Code:**

```
    def floodFill(self, image: List[List[int]], sr: int, sc: int, color: int) -> List[List[int]]:
        reference_color = image[sr][sc]

        # No change needed
        if reference_color == color:
            return image

        rows, cols = len(image), len(image[0])

        # init & change color immediately
        queue = deque([(sr, sc)])
        image[sr][sc] = color

        directions = [
            (1, 0), (-1, 0),
            (0, 1), (0 , -1)
        ]

        while queue:
            r, c = queue.popleft()
            for dr, dc in directions:
                nr, nc = r + dr, c + dc

                if (
                    nr >= 0 and nr < rows and
                    nc >= 0 and nc < cols and
                    image[nr][nc] == reference_color
                ):
                    image[nr][nc] = color
                    queue.append((nr, nc))

        return image

```

### 3. Count Servers that communicate

**Problem:**
You are given a map of a server center, represented as a `m * n` integer matrix grid, where 1 means that on that cell there is a server and 0 means that it is no server. Two servers are said to communicate if they are on the same row or on the same column.

Return the number of servers that communicate with any other server.

**Example:**

```
Input: grid = [
    [1,1,0,0],
    [0,0,1,0],
    [0,0,1,0],
    [0,0,0,1]
]

Output: 4
```

**Strategy:**

```
Main Idea -> track num systems in rows & cols
- init row_census arr [0] * rows
- init col_census arr [0] * cols

1st loop -> populate census arrays
2nd loop -> comm_device += 1 if:
    - row_census[r] > 1 or col_census[c] > 1
```

**Code:**

```
    def countServers(self, grid: List[List[int]]) -> int:
        rows, cols = len(grid), len(grid[0])

        # Init Census array
        row_counts = [0] * rows
        col_counts = [0] * cols

        # First pass to track num systems per row & col
        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == 1:
                    row_counts[r] += 1
                    col_counts[c] += 1

        communicating_device = 0

        # Second pass to see if device is communicating
        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == 1 and (row_counts[r] > 1 or col_counts[c] > 1):
                    communicating_device += 1

        return communicating_device
```

### 4. Number of Islands

**Problem:**
Given a 2D grid `grid` where '1' represents land and '0' represents water, count and return the number of islands.

An island is formed by connecting adjacent lands horizontally or vertically and is surrounded by water. You may assume water is surrounding the grid (i.e., all the edges are water).

**Example:**

```
Input: grid = [
    ["0","1","1","1","0"],
    ["0","1","0","1","0"],
    ["1","1","0","0","0"],
    ["0","0","0","0","0"]
  ]
Output: 1
```

**Strategy:**

```
1. Grid Scan: Iterate through every cell (satellite)
2. Trigger: (when find '1')
    - execute sink_island(r, c) with BFS within
    - sink cell (1 to 0) to never visit again
    - using deque & directions
    - boundary & land cell check
```

**Code:**

```
 def numIslands(self, grid: List[List[str]]) -> int:
        rows, cols = len(grid), len(grid[0])
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

        def sink_island(sr, sc):
            grid[sr][sc] = '0'
            q = deque([(sr, sc)])

            while q:
                r, c = q.popleft()
                for dr, dc in directions:
                    nr, nc = r + dr, c + dc

                    if (
                        nr >= 0 and nr < rows and
                        nc >= 0 and nc < cols and
                        grid[nr][nc] == '1'
                    ):
                        grid[nr][nc] = '0'
                        q.append((nr, nc))

        num_islands = 0
        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == '1':
                    num_islands += 1
                    sink_island(r, c)

        return num_islands
```

### 5. Max Size of Island

**Problem:**
You are given a matrix grid where `grid[i]` is either a 0 (representing water) or 1 (representing land).

An island is defined as a group of 1's connected horizontally or vertically. You may assume all four edges of the grid are surrounded by water.

The area of an island is defined as the number of cells within the island.

Return the maximum area of an island in grid. If no island exists, return 0.

**Example:**

```
Input: grid = [
  [0,1,1,0,1],
  [1,0,1,0,1],
  [0,1,1,0,1],
  [0,1,0,0,1]
]

Output: 6

```

**Strategy:**

```
- Same idea as Num Islands
- init a sink_island(r, c)
    - grid[r][c] = 0 # sink
    - count += 1
    - return count
- init max_size_so_far = 0
- satellite loop:
    - count = sink_island(r, c)
    - max_size_so_far = max(max_size_so_far, count)
```

**Code:**

```
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        rows, cols = len(grid), len(grid[0])

        directions = [
            (1, 0), (-1, 0),
            (0, 1), (0, -1)
        ]

        def sink_and_get_size(sr, sc):
            grid[sr][sc] = 0
            cur_island_size = 1

            q = deque([(sr, sc)])

            while q:
                r, c = q.popleft()
                for dr, dc in directions:
                    nr, nc = r + dr, c + dc

                    if (
                        nr >= 0 and nr < rows and
                        nc >= 0 and nc < cols and
                        grid[nr][nc] == 1
                    ):
                        grid[nr][nc] = 0
                        cur_island_size += 1

                        q.append((nr, nc))

            return cur_island_size

        largest_island_size = 0
        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == 1:
                    size = sink_and_get_size(r, c)
                    largest_island_size = max(largest_island_size, size)

        return largest_island_size
```

---

## Directed Graph / Degree Counting Pattern

### 1. Town Judge

**Problem:**
In a town, there are n people labeled from `1 to n`. There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:

- The town judge trusts nobody.
- Everybody (except for the town judge) trusts the town judge.
- There is exactly one person that satisfies properties 1 and 2.

You are given an array trust where trust[i] = [ai, bi] representing that the person labeled ai trusts the person labeled bi.

Return the label of the town judge if the town judge exists and can be identified, or return -1 otherwise.

**Key Idea:**

```
- Indegree: People who trust me (+=1)
- Outdegree: People I trust (-=1)
- Judge Condition: Score = (n - 1)
- Create score array [0] * (n + 1)
- += 1 if receive trust, -=1 if send trust
```

**Code:**

```
# O(E) Time | O(N) Space
scores = [0] * (n + 1)

for t in trust:
    trust_sent, trust_received = t
    scores[trust_sent] -= 1      # Outdegree contributes -1
    scores[trust_received] += 1  # Indegree contributes +1

for i in range(1, n + 1):
    # The only way to reach N-1 is: (N-1 incoming) - (0 outgoing)
    if scores[i] == n - 1:
        return i

return -1
```
