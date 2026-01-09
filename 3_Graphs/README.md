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
