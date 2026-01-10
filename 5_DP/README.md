# Dynamic Programming

## A. Linear DP (1D)

### 1. Climbing Stairs

**Problem:**
You are given an integer n representing the number of steps to reach the top of a staircase. You can climb with either 1 or 2 steps at a time.

Return the number of distinct ways to climb to the top of the staircase.

**Example:**

```
Input: n = 3
Output: 3
1 + 1 + 1 = 3
1 + 2 = 3
2 + 1 = 3
```

**Strategy:**

```
1. Identify recurrence
    - if standing at step N
        - came from step (N - 1)
        - came from step (N - 2)
2. Ways(N) = Ways(N-1) + Ways(N-2)
3. Identify Base case (min needed for recursive calc)
    - step 0 -> 1 way
    - step 1 -> 1 way
4. step(2) = step(2-1) + step(2-2)
5. create dp table to store results
6. Space optimize if possible
```

**Code:**

```
def climbStairs(self, n: int) -> int:
    num_ways = [0] * (n + 1)

    num_ways[0] = 1
    num_ways[1] = 1
    for i in range(2, n+1):
        num_ways[i] = num_ways[i-1] + num_ways[i-2]

    return num_ways[n]
```

**Space Optimize:**

```
- Recurrence only depend on 2 past variables
- store initial variables as cur and pre & update per iteration
```

**Code (Space Optimized):**

```
def climbStairs(self, n: int) -> int:
    prev2 = 1
    prev1 = 1
    for i in range(2, n+1):
        cur = prev1 + prev2

        prev2 = prev1
        prev1 = cur

    return cur
```

### 2. Min Cost Climbing Stairs

**Problem:**
You are given an array of integers `cost` where `cost[i]` is the cost of taking a step from the `i-th` floor of a staircase. After paying the cost, you can step to either the `(i + 1)-th` floor or the `(i + 2)-th` floor.

You may choose to start at the index 0 or the index 1 floor.

Return the minimum cost to reach the top of the staircase, i.e. just past the last index in cost.
**Example:**

```
Input: cost = [1,2,1,2,1,1,1]
Output: 4
```

**Strategy:**

```
1. Identify Recurrence (At Step N):
    - paid at (N-1) and move 1-step
    - paid at (N-2) and move 2-step
2. Don't forget accumulated at that given step
    - paid at (N-1) & accumulated at (N-1)
    - paid at (N-2) & accumulated at (N-2)
3. Goal -> minimum of either
4. Identify Base Case (minimum for recurrence)
    - N = 0 -> 0 (can start from idx 0)
    - N = 1 -> 0 (can start from idx 1)
```

**Code:**

```
def minCostClimbingStairs(self, cost: List[int]) -> int:
    n = len(cost)
    cost_table = [0] * (n + 1)
    cost_table[0] = 0
    cost_table[1] = 0

    for i in range(2, n+1):
        cost_table[i] = min(
            cost[i-1] + cost_table[i-1],
            cost[i-2] + cost_table[i-2]
        )

    return cost_table[n]
```

**Space Optimization: O(1)**

```
- only need cost_table[i-1] & cost_table[i-2]
- initialize prev_cost1 & prev_cost2
```

**Code (Space Optimized):**

```
def minCostClimbingStairs(self, cost: List[int]) -> int:
    n = len(cost)
    prev_cost2 = 0
    prev_cost1 = 0

    for i in range(2, n+1):
        cur_cost = min(
            cost[i-1] + prev_cost1,
            cost[i-2] + prev_cost2
        )
        prev_cost2 = prev_cost1
        prev_cost1 = cur_cost
    return cur_cost
```

---

## B. Grid DP (2D)

---

## C. Knapsack (Choice)

---

## D. String / Subsequence (2D Dual Sequence)

---

## E. Interval DP (combining)

---

## F. State Machine DP
