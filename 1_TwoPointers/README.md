# Two-Pointers (5 flavours)

## A. Collider (Opposite Ends)

**Scenario:** Usually sorted array, find pair or reverse something or check symmetry
**Mechanism:** l, r = 0, n-1 | while l < r | l++, r--

### A1. Reverse String

**Problem:**
You are given an array of characters which represents a string `s`. Write a function which reverses a string.

You must do this by modifying the input array in-place with `O(1)` extra memory.
**Example:**

```
Input: s = ["n","e","e","t"]
Output: ["t","e","e","n"]
```

**Strategy:**

```
- tuple unpacking
a, b = b, a in pythn
```
**Code:**
```
l, r = 0, len(s) - 1
while l < r:
    s[l], s[r] = s[r], s[l]
    l += 1
    r -= 1

```

### A2. Valid Word Abbreviation
**Problem:**
A string can be shortened by replacing any number of non-adjacent, non-empty substrings with their lengths (without leading zeros).

For example, the string "implementation" can be abbreviated in several ways, such as:

"i12n" -> ("i mplementatio n")
"imp4n5n" -> ("imp leme n tatio n")
"14" -> ("implementation")
"implemetation" -> (no substrings replaced)
Invalid abbreviations include:

"i57n" -> (i mplem entatio n, adjacent substrings are replaced.)
"i012n" -> (has leading zeros)
"i0mplementation" (replaces an empty substring)
You are given a string named word and an abbreviation named abbr, return true if abbr correctly abbreviates word, otherwise return false.

A substring is a contiguous non-empty sequence of characters within a string.

**Example:**
```
Input: word = "apple", abbr = "a3e"
Output: true
```

**Strategy:**
```
- explorer tracking 'word'
- navigator tracking 'abbr'
- also need word_len & abbr_len
1. while explorer < word_len and navigator < abbr_len
    - perform isdigit() check
        - if '0' -> return False (leading 0)
        - while navigator < adj_len & char.isdigit()
            - digitBuilder += char 
            - navigator += 1
```

### A3. Squares of Sorted Array
**Problem:**
You are given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

**Example:**
```
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
```
**Strategy:**
```
1. init results -> [0] * n
2. init l, r = 0, n-1
3. down-iterate (n-1, -1, -1)
    -> perform abs(l) > abs(r)
        - val = nums[l] & l += 1
        - else val = nums[r] & r-= 1
4. result[i] = val * val
```
**Code:**
```
def sortedSquares(self, nums: List[int]) -> List[int]:
    n = len(nums)
    result = [0] * n
    l, r = 0, n-1
    for i in range(n-1, -1, -1):
        if abs(nums[l]) > abs[num[r]]:
            val = nums[l]
            l += 1
        else:
            val = nums[r]
            r -= 1

        result[i] = val * val
    return result
```

## B. Hare & Tortoise (Fast Slow)

**Scenario:** Typically in LinkedList or Circular arrays
**Mechanism:** slow, fast = head, head

- slow = slow.next, fast = fast.next.next

## C. Merger (Pointers in separate structure)

**Scenario:** Two separate sorted arrays, need to merge
**Mechanism:** p1 for arrA, p2 for arrB. Move whichever processed

### C1. Merge Strings Alternately
**Problem:** 
You are given two strings, word1 and word2. Construct a new string by merging them in alternating order, starting with word1 — take one character from word1, then one from word2, and repeat this process.

If one string is longer than the other, append the remaining characters from the longer string to the end of the merged result.

Return the final merged string.
**Example:**
```
Input: word1 = "abc", word2 = "xyz"
Output: "axbycz"
```
**Strategy:**
```
1. init p1, p2 = 0, 0 & n1, n2 = len(word1), len(word2)
2. perform initial loop: p1 < n1 and p2 < n2
    - result += word1[p1]
    - result += word2[p2]
3. perform p1 check -> if p1 < n1:
    - result += word1[p1:]
4. perform p2 check -> if p2 < n2: 
    - result += word2[p2:]
```
**Code:**
```
def mergeAlternately(self, word1: str, word2: str) -> str:
    p1, p2 = 0, 0
    n1, n2 = len(word1), len(word2)

    result = ''
    while p1 < n1 and p2 < n2:
        result += word1[p1]
        result += word2[p2]

        p1 += 1
        p2 += 1
    
    if p1 < n1:
        result += word1[p1:]
    if p2 < n2:
        result += word2[p2:]
    
    return result        
```
### C2. Merge Sorted Array
**Problem:**
You are given two integer arrays nums1 and nums2, both sorted in non-decreasing order, along with two integers m and n, where:

m is the number of valid elements in nums1,
n is the number of elements in nums2.
The array nums1 has a total length of (m+n), with the first m elements containing the values to be merged, and the last n elements set to 0 as placeholders.

Your task is to merge the two arrays such that the final merged array is also sorted in non-decreasing order and stored entirely within nums1.
You must modify nums1 in-place and do not return anything from the function.
**Example:**
```
Input: nums1 = [10,20,20,40,0,0], m = 4, nums2 = [1,2], n = 2
Output: [1,2,10,20,20,40]
```
**Strategy:**
```
- instead of front, check & shift from back
1. init 3 pointers p1, p2, p3 = m-1, n-1, m+n-1
2. iterate while p2 >= 0 (if p2 checked, p1 already in order)
    - if nums1[p1] > nums2[p2] -> nums1[p3] = nums1[p1]
        p1 -= 1
    - else -> nums1[p3] = nums2[p2]
        p2 -= 1
    - p3 -= 1
```
**Code:**
```
def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
    """
    Do not return anything, modify nums1 in-place instead.
    [10 20 10 20 20 40]   [1 2]
    """
    p1, p2, p3 = m-1, n-1, m+n-1
    
    # once p2 completes, p2 already in place
    while p2 >= 0: 
        if p1 >= 0 and nums1[p1] > nums2[p2]:
            nums1[p3] = nums1[p1]
            p1 -= 1
        
        else:
            nums1[p3] = nums2[p2]
            p2 -= 1

        p3 -= 1
```

### C3. Find Content Children:
**Problem:**
Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie.

Each child i has a greed factor g[i], which is the minimum size of a cookie that the child will be content with; and each cookie j has a size s[j]. If s[j] >= g[i], we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.

**Example:**
```
Input: g = [1,2], s = [1,2,3]
Output: 2
```
**Strategy"**
```
1. g.sort() & s.sort()
2. init buyer_idx, seller_idx = 0, 0
    - seller_idx -> g (value to meet to sell)
    - buyer_idx -> s (value avail to spend)
3. while buyer_idx < len(s) and seller_idx < (len(g))
4. check if s[buyer_idx] >= g[seller_idx]
    - seller_idx += 1 (if bought, increment this to next)
5. buyer_idx += 1
    - if no purchase -> move next
    - if purchase -> still move next
```
**Code:**
```
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort()
        s.sort()

        buyer_idx, seller_idx = 0, 0
        while buyer_idx < len(s) and seller_idx < len(g):
            if s[buyer_idx] >= g[seller_idx]:
                seller_idx += 1
        
        # else, buyer cannot afford (or already bought)
            buyer_idx += 1

        return seller_idx
```

## D. Caterpillar (Sliding Window)

**Scenario:** find subarray (contiguous) that meets criteria
**Mechanism:** l, r = 0, 0 or l, r = 0, l+1

- right expands, left shrinks

## E. Partition (In-place modifier)

**Scenario:** filter / sort colors / move elements to 1 side in-place
**Mechanism:** Pointer i explores, pointer p tracks boundary of "safe" zone

### E1. Move Zeroes
**Problem:**
You are given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Note that you must do this in-place without making a copy of the array.
**Example:**
```
Input: nums = [0,0,1,2,0,5]
Output: [1,2,5,0,0,0]
```
**Strategy:**
```
1. init anchor, explorer ptr = 0, 0
2. while explorer < len(nums)
    - if explorer != 0
        - swap l, r = r, l
        - anchor += 1
```
**Code:**
```
def moveZeroes(self, nums: List[int]) -> None:
    """
    Do not return anything, modify nums in-place instead.
    """
    anchor = explorer = 0
    while explorer < len(nums):
        if nums[explorer] != 0:
            nums[anchor], nums[explorer] = nums[explorer], nums[anchor]
            anchor += 1
        explorer += 1
```

### E2. Remove Duplicates From Sorted Array
**Problem:**
You are given an integer array nums sorted in non-decreasing order. Your task is to remove duplicates from nums in-place so that each element appears only once.

After removing the duplicates, return the number of unique elements, denoted as k, such that the first k elements of nums contain the unique elements.

Note:

The order of the unique elements should remain the same as in the original array.
It is not necessary to consider elements beyond the first k positions of the array.
To be accepted, the first k elements of nums must contain all the unique elements.
Return k as the final result.
**Example:**
```
Input: nums = [1,1,2,3,4]
Output: [1,2,3,4]
```
**Strategy:**
```
1. perform len 1 check -> return 1
2. set anchor = explorer = 1
3. iterate explorer < len(nums)
    - check explorer[i] != explorer[i-1]
        - nums[anchor] = nums[explorer]
        - anchor += 1
    - explorer += 1
4. return anchor
```
**Code:**
```
def removeDuplicates(self, nums: List[int]) -> int:
    if not nums:
        return 0
    
    explorer = anchor = 1
    while explorer < len(nums):
        if nums[explorer] != nums[explorer-1]:
            nums[anchor] = nums[explorer]
            anchor += 1
        explorer += 1
    
    return anchor
```

