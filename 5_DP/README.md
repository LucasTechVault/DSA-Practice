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

### 3. House Robber

**Problem:**
You are given an integer array `nums` where `nums[i]` represents the amount of money the `ith` house has. The houses are arranged in a straight line, i.e. the `ith` house is the neighbor of the `(i-1)th` and `(i+1)th` house.

You are planning to rob money from the houses, but you **cannot rob two adjacent houses** because the security system will automatically alert the police if two adjacent houses were both broken into.

Return the maximum amount of money you can rob without alerting the police.

**Example:**

```
Input: nums = [2,9,8,3,6]
Output: 16
nums[0] + nums[2] + nums[4] = 2 + 8 + 6 = 16.
```

**Strategy:**

```
* Init accum_so_far = [0] * (n+1)
    - normalizes base case
        - dp[0]: profit from 0 house (always 0)
        - dp[1]: profit from 1 house (always nums[0])
1. Identify Recurrence (At house i):
    - "Do I rob this house or skip it?"
        - Choice A: Rob current + Accum money 2 houses ago
        - Choice B: Skip current + Accum money 1 house ago

    - Logic:
        rob_current = nums[i-1] + dp[i - 2]
            - becareful of off-by-1 idx
        skip_current = 0 + dp[i-1]

2. Identify Base Case (Require 2 houses before)
    - accumulated_so_far[0] = nums[0]
        - only 1 house, rob that
    - accumulated_so_far[1] = max(nums[0], nums[1])
        - maximum of 2 houses
```

**Code:**

````
def rob(self, nums: List[int]) -> int:
    n = len(nums)
    if n == 0:
        return 0

    # accum_so_far[i] means "max profit considering the first i houses"
    accum_so_far = [0] * (n + 1)

    # Base Cases (Zero houses = 0 profit)
    accum_so_far[0] = 0
    accum_so_far[1] = nums[0]

    # Loop from the 2nd house (index 1) up to the nth house
    for i in range(2, n + 1):
        # CAREFUL: "Current House" is nums[i-1]
        rob_current  = nums[i-1] + accum_so_far[i-2]
        skip_current = accum_so_far[i-1]

        accum_so_far[i] = max(rob_current, skip_current)

    return accum_so_far[n]```

````

### 4. House Robber II

**Problem:**
You are given an integer array `nums` where `nums[i]` represents the amount of money the `ith` house has. **The houses are arranged in a circle**, i.e. the first house and the last house are neighbors.

You are planning to rob money from the houses, but you **cannot rob two adjacent houses** because the security system will automatically alert the police if two adjacent houses were both broken into.

Return the maximum amount of money you can rob without alerting the police.

**Example:**

```
Input: nums = [2,9,8,3,6]
Output: 15
You cannot rob nums[0] + nums[2] + nums[4] = 16
- nums[0] and nums[4] are adjacent houses.
- The maximum you can rob is nums[1] + nums[4] = 15.
```

**Strategy:**

```
1. Define function -> robLinear(nums: list[int])
    - logic will be same as House Robber 1
2. Split sequence into 2
    2.1 -> rob first, skip last
        - robLinear(nums[0:n-2])
    2.2 -> skip first, rob last
        - robLinear(nums[1: n-1])
3. return max(robFirst, skipFirst)
```

**Code:**

```
def rob(self, nums: List[int]) -> int:
    if not nums:
        return 0
    if len(nums) == 1:
        return nums[0]

    def roblinear(houses: list[int]) -> int:
        if not houses:
            return 0
        if len(houses) == 1:
            return houses[0] # single house

        robValueAccum = [0] * (len(houses) + 1)

        # Base Cases
        robValueAccum[0] = 0 # no houses robbed prior
        robValueAccum[1] = houses[0] # 1 house prior

        # Recursive case
        for i in range(2, len(houses)+1):
            skip_current = 0 + robValueAccum[i-1]
            rob_current = houses[i-1] + robValueAccum[i-2] # adjacency rule
            robValueAccum[i] = max(skip_current, rob_current)

        return robValueAccum[len(houses)]

    # Divide into 2 scenarios
    n = len(nums)
    rob_first_skip_last_seq = nums[:n-1]
    skip_first_rob_last_seq = nums[1:]

    rob_first_res = roblinear(rob_first_skip_last_seq)
    skip_first_res = roblinear(skip_first_rob_last_seq)

    return max(rob_first_res, skip_first_res)

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

```

```
