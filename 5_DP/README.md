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

### 5. Decode Ways

**Problem:**
A string consisting of uppercase english characters can be encoded to a number using the following mapping:

```
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```

To decode a message, digits must be grouped and then mapped back into letters using the reverse of the mapping above. There may be multiple ways to decode a message. For example, "1012" can be mapped into:

```
"JAB" with the grouping (10 1 2)
"JL" with the grouping (10 12)
```

The grouping `(1 01 2)` is invalid because `01` cannot be mapped into a letter since it contains a leading zero.

Given a string s containing only digits, return the number of ways to decode it. You can assume that the answer fits in a 32-bit integer.

**Example:**

```
Input: s = "12"
Output: 2
Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).
```

**Strategy:**

```
1. init dp = [0] * (n + 1) to track all states up to idx i (accum so far)
2. Find Recurrence relation:
    - At idx i, only 2 actions possible:
        1. Single Digit Valid -> is s[i - 1] valid '1-9'?
            - yes -> += dp[i-1] (number of ways valid accum for all single digits)
            - no -> add nothing
        2. Double digit valid -> is s[i-2:i] valid '10-26'?
            - yes -> += dp[i-2] (number of ways valid accum for all double digits)
            - no -> add nothing
3. Populate base cases
    - dp[0] = 1 (Empty string = 1 way -> to prep for 2 digit)
        - for intuition, picture i=2 for '12' (valid). if dp[i-2] where i=2 is 0, no summing occurs
    - dp[1] = 1 if s[0] != '0' else 0
4. Iterate range(2, n+1)
    - Perform Single Digit Check -> += dp[i-1] (valid single digit accum so far)
    - Perform Double Digit check -> += dp[i-2] (valid double digit accum so far)
```

**Code:**

```
    def numDecodings(self, s: str) -> int:
        n = len(s)
        dp = [0] * (n + 1)

        # Base cases:
        dp[0] = 1
        dp[1] = 1 if s[0] != '0' else 0

        for i in range(2, n+1):

            # Valid Single Digit check
            if s[i-1] != '0':
                dp[i] += dp[i-1]

            # Valid Double Digit check
            double_digit = int(s[i-2:i])
            if double_digit >= 10 and double_digit <= 26:
                dp[i] += dp[i-2]

        return dp[n]
```

### 6. Coin Change

**Problem:**
You are given an integer array `coins` representing coins of different denominations (e.g. 1 dollar, 5 dollars, etc) and an integer `amount` representing a target amount of money.

Return the fewest number of coins that you need to make up the exact target amount. If it is impossible to make up the amount, return `-1`.

You may assume that you have an **unlimited number of each coin**.

**Example:**

```
Input: coins = [1,5,10], amount = 12
Output: 3
Explanation: 12 = 10 + 1 + 1. Note that we do not have to use every kind coin available.
```

**Strategy:**

```
1. init amounts bucket representing number of coins needed for that amount
    - amounts = [amount + 1] * (amount + 1)
    - e.g. amount = 5 -> [6, 6, 6, 6, 6, 6]
    - each idx means -> idx 0 = $0, idx 1 = $1
2. Base case -> amounts[0] = 0 (0 coins for 0$)
3. Iterate each amount in amounts
    - Iterate each coin in coins (e.g. {1, 2, 5} in coins)
    - amounts[i] = min(amounts[i], amounts[i-c] + 1)
        - amounts[i] <- accumulated coins from previous coin iteration
        - amounts[i-c] + 1 <- new denomination minimum (eg amt 5$ - coin 5$)
- why amounts[i-c]?
    - At i = $5 for c=$1, (i-c) = $4
    - amounts[$4] means min number of coins accumulated at $4
    - amounts[$4] + 1 ($1 coin)
    ---
    - At i = $5 for c=$5, (i-c) = $0
    - amount[$0] means min number at $0 which is 0
    - min(amounts[$5], amounts[$0] + 1) = min(5, 1) = 1
4. When returning, remember to check if > amount (means not possible)
    - return -1
else return amounts[amount]
```

**Code:**

```
    def coinChange(self, coins: List[int], amount: int) -> int:
        # bucket -> [13 13 13 ... 13]
        # each idx represent amount to "fill"
        amounts = [amount + 1] * (amount + 1)

        # Base case - 0 coins to make up $0
        amounts[0] = 0

        for amt in range(1, len(amounts)):
            # amt 1 means $1

            for coin in coins:
                if coin <= amt:
                    amounts[amt] = min(amounts[amt], amounts[amt-coin] + 1)

        if amounts[amount] > amount:
            return -1
        return amounts[amount]
```

## B. Grid DP (2D)

---

## C. Knapsack (Choice)

---

## D. String / Subsequence (2D Dual Sequence)

---

## E. Interval DP (combining)

```
1. To solve for [i, j], must first solve smaller interval
2. dp[i][j] depends on:
    - dp[i+1][j-1] (shrinking from both sides)
    - dp[i][k] + dp[k+1][j] (split interval)
3. Cannot iterate for i in range(n)
    - must iterate by length (size 1, size 2 etc..)
```

### 1. Longest Palindromic Substring

**Problem:**
Given a string `s`, return the longest substring of `s` that is a palindrome.

A palindrome is a string that reads the same forward and backward.

If there are multiple palindromic substrings that have the same length, **return any one of them.**

**Example:**

```
Input: s = "ababd"
Output: "bab"
```

**Intuition:**

```
1. Initialize a 2D grid dp [[False] * n for _ in range(n)]
    * only need to focus on upper triangular portion
     a     b     a     b     a
  a  False False False False False
  b        False False False False
  a              False False False
  b                    False False
  a                          False

2. Palindrome is just a smaller palindrome within
3. Diagonal 0 (Len 1) ('a', 'b', 'c') = True
    - (0,0) (1,1) (2,2) (3,3)
- Diagonal 1 (len 2) ("ab", "ba", "ab", "bd")
    - (0, 1), (1, 2), (2, 3), (3, 4)
- Diagonal 2 (len 3) ("aba", "bab", "abd")
    - (0, 2), (1, 3), (2, 4)
- Diagonal 3 (len 4) ("abab", "babd")
    - (0, 3), (1, 4)
- Diagonal 4 (len 5) ( "ababd")

To find len 5 -> we must "look back"
    - dp[i+1][j-1]
    e.g. len 5 -> i = 0, j = 4
        - i + 1 = 1
        - j - 1 = 3
        - dp[1][3] = "bab" <- inner string
        - dp[0][4] = "a bab a" <- original
    - To find len 3 "bab", we must "look back"
        - i + 1 = 2
        - j - 1 = 2
        - dp[2][2] = 'a'<- inner string (base case)
        - dp[1][3] = "bab" <- original
```

**Code:**

```
def longestPalindrome(self, s: str) -> str:
    n = len(s)
    if n == 1:
        return s

    max_len_so_far = 1 # default, single char is palindrome
    start_idx_of_palindrome = 0

    # 1. Init dp table
    dp = [[False] * n for _ in range(n)]

    # 2. Populate base case - single char
    for i in range(n):
        dp[i][i] = True

    # 3. Iterate length from 2 to n
    for length in range(2, n + 1):

        # 4. Iterate start index (end at n)
        for i in range(n - length + 1):
            j = i + length - 1 # end index

            # 5. Check if outer char match
            if s[i] == s[j]:

                # 5.1. if len 2, then naturally is palindrome
                if length == 2:
                    dp[i][j] = True

                # 5.2. else, check inner string is valid palindrome
                else:
                    dp[i][j] = dp[i+1][j-1]

            # 6. If Valid palindrome, update start_idx & longest length
            if dp[i][j]:
                max_len_so_far = max(max_len_so_far, length)
                start_idx_of_palindrome = i

    return s[start_idx_of_palindrome: start_idx_of_palindrome+max_len_so_far]
```

### 2. Palindromic Substrings

**Problem:**
Given a string `s`, return the number of substrings within `s` that are palindromes.

A palindrome is a string that reads the same forward and backward.
**Example:**

```
Input: s = "abc"
Output: 3
Explanation: "a", "b", "c".
```

**Strategy:**

```
- Same concept as Palindromic substring
- main difference is to count num palindromes
- instead of start_idx & len_of_longest

1. init dp table [[False] * n for _ in range(n)]
2. Init num_palindrome = 0
3. Base case dp[i][i] = True + num_palindrome++
4. Iterate length from 2 to n
    - Iterate i to (n - length + 1)
5. if palindrome, increment count
```

**Code:**

```
def countSubstrings(self, s: str) -> int:
    n = len(s)
    if n == 1:
        return 1

    num_palindromes = 0
    dp = [[False] * n for _ in range(n)]

    for i in range(n):
        dp[i][i] = True
        num_palindromes += 1

    for length in range(2, n+1):
        for i in range(n - length + 1):
            j = i + length - 1

            if s[i] == s[j]:
                # if len 2, must be palindrome
                if length == 2:
                    dp[i][j] = True
                else:
                    dp[i][j] = dp[i+1][j-1]

            if dp[i][j]:
                num_palindromes += 1
    return num_palindromes
```

---

## F. State Machine DP

```

```
