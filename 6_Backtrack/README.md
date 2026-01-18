# Backtracking

- "Stubborn" recursion - try everything until hit wall then undo last step
- **Core Mechanism:** State Tree (Traverse using DFS)

## A. "Start Index" Pattern

**Goal:** Find groups where order does not matter {1, 2} == {2, 1}
**Mechanism:** Pass `start_idx` to recursive fn so that recursion never looks back

- **Intuition:** Standing at idx i, pick any number from i to the right. Then pass the baton to (i+1)

```
def backtrack(start, path):
    # Base Case
    results.append(path[:]) # [:] to make a copy

    for i in range(start, len(nums)):
        path.append(nums[i]) # add to exploration
        backtrack(i + 1, path) # explore
        path.pop() # hit wall, turn back
```

### A1. Subsets

**Problem:**
Given an array nums of unique integers, return all possible subsets of nums.

The solution set must not contain duplicate subsets. You may return the solution in any order.
**Example:**

```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**Strategy:**

```
1. Identify that this qns, order does not matter [1,2] == [2,1]
2. Check for the 2 base cases where n == 0 and n == 1
    - n == 0 -> return [[]]
    - n == 1 -> return [[], nums[0]]
3. def a recursive explore_subset(start_idx, path)
    - start_idx: used to track the exploration (rightwards only)
    - path: track state of exploration
4. iterate i from start_idx to n
    4.1 add i to path to begin exploration
    4.2 explore neighbor of i explore_subset(i+1, path)
    4.3 pop from path after depth explored
5. call explore_subset(0, []) to popultae result
```

**Code:**

```
def subsets(self, nums: List[int]) -> List[List[int]]:
    n = len(nums)
    if n == 0:
        return []
    if n == 1:
        return [[], [nums[0]]]

    result = []
    def explore_subsets(start_idx, path):
        # Base Case
        result.append(path[:])

        # Recursive Case
        for i in range(start_idx, n):
            path.append(nums[i])
            explore_subsets(i+1, path)

            # backtrack
            path.pop()

    explore_subsets(0, [])
    return result
```

### A2. Combination Sum (Start idx with a twist)

**Problem:**
Given an array of distinct integers `candidates` and a target integer `target`, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

The **same number may be chosen** from candidates an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to target is less than 150 combinations for the given input.

**Example:**

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

**Strategy:**

```
1. init result [] to track combination sum result
2. sort candidates for pruning later
3. define a backtrack(start_idx, path, remaining_target)
    - key idea is using remaining_target to track validity
4. Define base case
    - if remaining_target == 0: append path[:]
5. otherwise, begin exploration
    - for i in range(start_idx, n)
6. extract cur_num = candidates[i]
    - if cur_num > remaining_sum -> break (essentially return up the recursive stack)
    - else, append cur_num into path
7. recurse backtrack(start_idx, path, remaining_sum - cur_num)
    - note -> start_idx is not incremented for re-use
    - decrement remaining sum
8. path.pop after backtracking exploration
```

**Code:**

```

```

## B. "Used Array" Pattern

**Goal:** Find groups where order matters {1, 2} != {2, 1}
**Mechanism:** Always loop from 0 to end. Use a set to skip elements in path

- **Intuition:** I can pick any card from deck so long as its not in my hand

```
def backtrack(path):
    if len(path) == len(nums):
        result.append(path[:])
        return

    for i in range(len(nums)):
        # currently within path but not ended
        if nums[i] in visited:
            continue

        # begin exploration
        visited.add(nums[i])
        path.append(nums[i])
        backtrack(path)

        # reach end, backtrack
        path.pop()
        visited.remove(nums[i])
```

###

## C. Board Constraint Pattern

**Goal:** Place items on constrained grid
**Mechanism:** At each cell, perform isValid(r, c) check before recursing. If invalid, skip

- **Intuition:** Can i place a queen here? Check row, cols and diagonals. If possible, place and move to next row. If stucked, return to placed Queen and try next square

```
def backtrack(row):
    if row == n:
        return True # success

    for col in range(n):
        if is_valid(row, col):
            board[row][col] = 'Q' # place queen
            if backtrack(row + 1):
                return True
            board[row][col] = '.' # remove (backtrack)

    return False
```

## D. Flood Fill Pattern

**Goal:** Find path or shape inside 2D grid.
**Mechanism:** Mark current cell as visited (often with special char like '#'), explore 4 directions, restore cell value

- **Intuition:** Leaving breadcrumbs during exploration. if deadend, pick up breadcrumbs during backtracking

```
def backtrack(r, c, idx):
    if board[c][c] != word[idx]:
        return False

    # mark visited
    temp, board[r][c] = board[r][c], '#'

    # explore neighbors
    found = (
        backtrack(r+1, c, i+1) or backtrack(r-1, c, i+1) or
        backtrack(r, c+1, i+1) or backtrack(r, c-1, i+1)
    )

    # unmark
    board[r][c] = temp
    return found
```

## E. Partition Pattern:

**Goal:** Distribute items into K buckets such that constraints are met
**Mechanism:** Iterate each bucket for current item: can item[i] fit in bucket[j]?

- **Inutuition:** I have a ball in hand, i try putting it in bin 1. Does it fit? if so, recurse to next ball. If fails, take it out of bin 1 and try bin 2

```
def backtrack(idx):
    if idx == len(nums): # explored all possible items
        return True

    for i in range(k): # try each bucket
        if buckets[i] + nums[idx] <= target:
            buckets[i] += nums[idx]
            if backtrack(idx + 1): # check if explored
                return True
            buckets[i] -= nums[idx] # remove from bucket for other exploration

        if buckets[i] == 0:
            break
```
