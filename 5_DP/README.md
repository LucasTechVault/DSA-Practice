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

### 7. Maximum Product Subarray

**Problem:**
Given an integer array `nums`, find a subarray that has the largest product within the array and return it.
A subarray is a contiguous non-empty sequence of elements within an array.
You can assume the output will fit into a 32-bit integer.
**Example:**

```
Input: nums = [1,2,-3,4]
Output: 4
```

**Strategy:**

```
- The key focus is not on finding a subarray, but on finding the maximum
- Something like Kadane's algorithm
- In normal addition (like Kadane's), negatives are bad
- In product, 2 negatives could becomes the hero
- Soln -> Double Tracking
    - cur_max -> largest product ending at cur_pos
    - cur_min -> smallest (most negative) product ending at cur_pos

1. Init Base Variables
    - cur_min = cur_max = nums[0]
    - result = nums[0] # tracks the final result
2. Loop i from range(1, len(nums))
    - max_product = nums[i] * cur_max
    - min_product = nums[i] * cur_min

3. Update values (3 possible values)
    1) cur_number (like Kadane's, cur_num could also be contender)
    2) max_product (can turn to min if +ve * -ve)
    3) min_product (can turn to max if -ve * -ve)

4. Update result max(cur_max, result)
```

**Code:**

```
def maxProduct(self, nums: List[int]) -> int:
    cur_min = cur_max = nums[0]
    max_tracker = nums[0]

    for i in range(1, len(nums)):
        cur_num = nums[i]
        max_product = cur_num * cur_max
        min_product = cur_num * cur_min

        cur_max = max(cur_num, max_product, min_product)
        cur_min = min(cur_num, max_product, min_product)

        max_tracker = max(max_tracker, cur_max)

    return max_tracker
```

### 8. Word Break

**Problem:**
Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of dictionary words.

You are allowed to reuse words in the dictionary an unlimited number of times. You may assume all dictionary words are unique.
**Example:**

```
Input: s = "catsincars", wordDict = ["cats","cat","sin","in","car"]
Output: false
```

**Strategy:**

```
1. Convert wordDict to set -> check word in set is O(1)
2. init valid_checkpoints of size (n + 1) with False
    - valid_checkpoints[i] represents prefix of length i in wordDict
3. Base case -> dp[0] = True (valid start point)
4. Recurrence case
    1. Outer loop i in range(1, n+1) -> goal is n
    2. Inner loop j in range(i)
        - start from last checkpoint dp[j] = True
            - will always start dp[0] since that is True
        - from last checkpoint to i, is it valid string? s[j:i] in wordDict?
        Logic: if dp[j] and dp[j:i] in wordDict
            - dp[i] = True # set new checkpoint
Trace:
dp = [T, F, F, T, T, F, F] for s='catsin', {cat, cats}
i = 5, j = 0 -> dp[0] True -> substring = 'catsin' -> False
i = 5, j skips [1, 2] because F. j = 3 -> substring = 'sin' -> False
i = 5, j = 4 -> substring = 'in' -> False
dp[5] = False
return dp[5]
```

**Code:**

```
def wordBreak(self, s: str, wordDict: List[str]) -> bool:
    wordDict = set(wordDict)
    n = len(s)

    valid_checkpoints = [False] * (n + 1)

    # Base case:
    valid_checkpoints[0] = True # valid starting checkpoint

    # Recursive exploration
    for i in range(1, n+1):


        for j in range(i):
            # start from valid checkpoints
            if valid_checkpoints[j]:

                # anything up to j already checked
                substring_to_check = s[j:i]

                if substring_to_check in wordDict:
                    valid_checkpoints[i] = True # update new checkpoint
                    break # break out of j loop

    return valid_checkpoints[n]

```

### 9. Longest Increasing Subsequence

**Problem:**
Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A subsequence is a sequence that can be derived from the given sequence by deleting some or no elements without changing the relative order of the remaining characters.

For example, "cat" is a subsequence of "crabt".

**Example:**

```
Input: nums = [9,1,4,2,3,3,7]
Output: 4
```

**Strategy:**

```
- imagine nums are heights of islands (left to right)
    - can only jump from left to right
    - can only jump from lower islands to higher islands
    - goal: max number of jumps before stopping

1. init dp = [1] * len(nums)
    - tracks the max record for each island
    - at dp[i], it records the max hops before ending
2. iterate i in range(n) <- exploring all islands possible
    - cur_island = nums[i]
3. iterate from j in range(i) <- check all valid island before
    - prev_island = nums[j]
    - condition to check: if cur_island > prev_island # can hope
        - dp[i] = max(dp[i], dp[j] + 1)
            - dp[i] - maximum recorded step
            - dp[j] + 1 - whereever we came from + 1
4. return max(dp) - return the max step recorded
```

**Code:**

```
def lengthOfLIS(self, nums: List[int]) -> int:
    n = len(nums)
    dp = [1] * n

    # loop every element in nums
    for i in range(n):

        cur_num = nums[i]

        # loop every element before i to check if increasing
        for j in range(i):
            prev_num = nums[j]

            # valid hop (lower -> higher island)
            if prev_num < cur_num:

                # update checkpoint
                dp[i] = max(dp[i], dp[j] + 1)
                # dp[i] tracks current number of hops
                # dp[j] + 1 (hop from that island to next island)

    return max(dp)
```

### 10. Partition Equal Subset Sum

**Problem:**
You are given an array of positive integers `nums`.

Return true if you can partition the array into two subsets, subset1 and subset2 where sum(subset1) == sum(subset2). Otherwise, return false.

**Example:**

```
Input: nums = [1,2,3,4]
Output: true
- {1, 4} and {2, 3}
```

**Strategy:**

```
1. Get sum(nums) and check if divisible by 2
2. if possible, set target = total // 2
3. init dp table = [False] * (target + 1)
    - each idx (1 -> target) represent if possible to land on target
    - if possible to land on target, the remaining half will be possible too
4. To populate dp table:
    4.1 iterate num in nums (our bullets)
    4.2 iterate downwards from target to num
        - if dp[i-num] is True, means possible to +num and arrive at i
        - set dp[i] = True (since we are iterating downwards for each num)
        - if dp[target] is True, return True (optimize check)
5. return dp[target]
```

**Code:**

```
def canPartition(self, nums: List[int]) -> bool:
    total = sum(nums)
    # total not divisible by 2, cannot be split
    if total % 2 != 0:
        return False

    # else, total is divisible by 2
    target = total // 2

    # Each idx 1 -> target represents if possible to reach this number
    dp = [False] * (target + 1)
    dp[0] = True # base case, where num 0 requires no value to sum

    # Iterate each value in nums
    for num in nums:

        # Iterate down from target to num
        for i in range(target, num-1, -1):

            # if we take `num` step back and its true, we can take `num` step fwd and be true
            if dp[i - num] == True:
                dp[i] = True

            if dp[target]:
                return True

    return dp[target]
```

### 11. Triangle

**Problem:**
You are given a `triangle` array, return the minimum path sum from top to bottom.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index `i` on the current row, you may move to either index `i` or index `i + 1` on the next row.

**Example:**

```
Input: triangle = [
    [2],
   [3,4],
  [6,5,7],
 [4,1,8,3]
]

Output: 11
```

**Strategy:**

```
1. Idea is we have to process current row i and next row (i+1)
2. Instead of top-down, we view it bottom-up
3. We init a 2D dp table similar to triangle but all values 0
4. Base case is dp[-1] = triangle[-1] -> last row has no next row, so is itself
5. We iterate bottom up from 2nd last row
    - dp[i][j] = triangle[i][j] + min(dp[i+1][j], dp[i+1][j+1])
        - take the min of the left and right child
6. At dp[0][0] -> it will be the min sum
- Why it works? -> Triangle takes binary tree structure
    - this is essentially min path sum
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

### F1. Paint House

**Problem:**
There is a row of `n` houses, where each house can be painted one of three colors: red, blue, or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by an n x 3 cost matrix `costs`.

For example, `costs[0][0]` is the cost of painting house 0 with the color red; `costs[1][2]` is the cost of painting house 1 with color green, and so on...

Return the minimum cost to paint all houses.

**Example:**

```
Input: costs = [[17,2,17],[16,16,5],[14,3,19]]
Output: 10
```

**Strategy:**

```
- n = len(costs) - number of houses to paint
1. Init red_dp, blue_dp, green_dp = [0] * n
    - n and not (n+1) because dependent only on houses before (i-1)
2. Each element in dp is the accumulated cost if we took that path
3. Base case -> red_dp[0] = costs[0][0] | blue_dp[0] = costs[0][1] ...
4. Iterate from i = 1 to n:
    - red_dp[i] = cost[i][0] + min(blue_dp[i-1], green_dp[i-1])
        - cost[i][0] - cost of red at this cur step
        - min(blue_dp[i-1], green_dp[i-1]): prev can only be blue or green (accum)
5. return min(red_dp[-1], blue_dp[-1], green_dp[-1])
    - the ending is the accumulated cost at step n
```

**Code:**

```
def minCost(self, costs: List[List[int]]) -> int:
    n = len(costs)
    if n == 0:
        return 0

    red_dp = [0] * n
    blue_dp = [0] * n
    green_dp = [0] * n

    # Base cases:
    red_dp[0] = costs[0][0]
    blue_dp[0] = costs[0][1]
    green_dp[0] = costs[0][2]

    # Iterative case:
    # each element in dp is the accumulated cost
    for i in range(1, n):
        red_dp[i] = costs[i][0] + min(blue_dp[i-1], green_dp[i-1])
        blue_dp[i] = costs[i][1] + min(red_dp[i-1], green_dp[i-1])
        green_dp[i] = costs[i][2] + min(red_dp[i-1], blue_dp[i-1])

    # After iteration, dp[-1] contains the final accumulated cost
    return min(
        red_dp[-1], blue_dp[-1], green_dp[-1]
    )
```
