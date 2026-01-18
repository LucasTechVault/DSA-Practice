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
