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

**Code Solution:**

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
