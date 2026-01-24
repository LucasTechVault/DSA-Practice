### 1. Longest Common Prefix
**Problem:**
You are given an array of strings strs. Return the longest common prefix of all the strings.

If there is no longest common prefix, return an empty string "".
**Example:**
```
Input: strs = ["bat","bag","bank","band"]
Output: "ba"
```
**Strategy:**
```
1. Choose first word as reference
2. Iterate all other words from 1 to end
3. write a 'startswith' method (reference, word)
    - if len(reference) > len(word) -> return False
    - iterate i in range(len(reference))
        - if ref[i] != word[i] -> return False
    - return True if all matches
4. write a while not startswith(reference, word):
    - reference = reference[:-1]
```
**Code:**
```
def longestCommonPrefix(strs: list[str]):
    def startsWith(prefix, word):
        if len(prefix) > len(word):
            return False
        
        for i in range(len(prefix)):
            if prefix[i] != word[i]

    reference = strs[0]
    for word in strs[1:]:


```

### 2. Group Anagrams
**Problem:**
Given an array of strings strs, group all anagrams together into sublists. You may return the output in any order.

An anagram is a string that contains the exact same characters as another string, but the order of the characters can be different.
**Example:**
```
Input: strs = ["act","pots","tops","cat","stop","hat"]

Output: [["hat"],["act", "cat"],["stop", "pots", "tops"]]
```
**Strategy:**
```
1. init anagramDict defaultDict(list)
2. iterate through each word 
    - sort each word because anagrams have same sort
        - act & cat -> a,c,t
3. anagramDict[key].append(word)
4. return list(anagramDict.values())
```

**Code:**
```
def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
    anagramDict = defaultdict(list)
    
    for word in strs:
        key = "".join(sorted(word))
        anagramDict[key].append(word)
    
    return list(anagramDict.values())
```

### 3. Remove Element
**Problem:**
You are given an integer array nums and an integer val. Your task is to remove all occurrences of val from nums in-place.

After removing all occurrences of val, return the number of remaining elements, say k, such that the first k elements of nums do not contain val.

Note:

The order of the elements which are not equal to val does not matter.
It is not necessary to consider elements beyond the first k positions of the array.
To be accepted, the first k elements of nums must contain only elements not equal to val.
Return k as the final result.
**Example:**
```
Input: nums = [1,1,2,3,4], val = 1
Output: [2,3,4]
```
**Strategy:**
```
- bouncer at the club, inspecting each element
- only allow write if nums[i] != val
1. init anchor = 0 -> write to array if valid
    - valid means nums[i] != val
2. iterate every element
    - if nums[i] != val
        - nums[anchor] = nums[i]
        - anchor += 1
3. return anchor -> no. valid elements
```
**Code:**
```
def removeElement(self, nums: List[int], val: int) -> int:
    anchor = 0

    for i in range(len(nums)):
        if nums[i] != val:
            nums[anchor] = nums[i]
            anchor += 1
    
    return anchor
```

### Majority Element
**Problem:**
Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times in the array. You may assume that the majority element always exists in the array.
**Example:**
```
Input: nums = [5,5,1,1,1,5,5]
Output: 5
```
**Strategy:**
```
- Boyer-Moore voting algorithm
    - if 2 different people meet, they knock each other out
    - eventually, majority will be the remaining
1. init count = 0, candidate = None
2. iterate each num in nums
    - if count == 0 -> candidate = num
    - if num == candidate -> count += 1 else count -= 1
3. return candidate
```
**Code:**
```
```
