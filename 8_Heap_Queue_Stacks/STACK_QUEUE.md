# Stacks & Queues
    - Processing tools
    - about ordering & flow (LIFO, FIFO)

## Stack Pattern
    - Like a time machine that tracks occurrence of events

### A - Matching Pair
    - ensure things open / close correctly
    - push openers onto stack
    - pop from stack if closer and compare

### B Monotonic Stack (next greater element)
    - "Nobody smaller than me can stand behind me"
    - pop elements off stack until rule satisfied, then push current element

### C Stack Calculator
    - stack to hold numbers or operators
    - used to handle nested parenthesis operations


### C1 - Crawler Log
**Problem:**
There is a file system that keeps a log each time some user performs a change folder operation.

The operations are described below:

"../" : Move to the parent folder of the current folder. (If you are already in the main folder, remain in the same folder).
"./" : Remain in the same folder.
"x/" : Move to the child folder named x (This folder is guaranteed to always exist).
You are given a list of strings logs where logs[i] is the operation performed by the user at the ith step.

The file system starts in the main folder, then the operations in logs are performed.

Return the minimum number of operations needed to go back to the main folder after the change folder operations.

**Example:**
```
Input: logs = ["d1/","d2/","../","d21/","./"]
Output: 2
```
**Strategy:**
```
1. Simply use stack to append child folders /x
2. if detect '../':
    - check if stack not empty -> pop
3. elif not './' then append
4. else continue ('./' -> do nothing)
5. return len(stack) which returns the min number of layers to get back
```

**Code:**
```
def minOperations(self, logs: List[str]) -> int:
    s = deque()

    for log in logs:
        if log == "../":
            if len(s) > 0:
                s.pop()
        
        elif log != './':
            s.append(log)
    
        else:
            continue
    
    return len(s)
```

### C2. Baseball Game
**Problem:**
You are keeping the scores for a baseball game with strange rules. At the beginning of the game, you start with an empty record.

Given a list of strings operations, where operations[i] is the ith operation you must apply to the record and is one of the following:

An integer x: Record a new score of x.
'+': Record a new score that is the sum of the previous two scores.
'D': Record a new score that is the double of the previous score.
'C': Invalidate the previous score, removing it from the record.
Return the sum of all the scores on the record after applying all the operations.

Note: The test cases are generated such that the answer and all intermediate calculations fit in a 32-bit integer and that all operations are valid.

**Example:**
```
Input: ops = ["1","2","+","C","5","D"]
Output: 18
```

**Strategy:**
```
1. check '+', 'D', 'C' & apply correspond operations
    - '+' -> s[-1] + s[-2]
    - 'D' -> s[-1] * 2
    - 'C' -> s.pop()
2. else s.append(int(item))
    - assumed that there are no other chars
```
**Code:**
```
def calPoints(self, operations: List[str]) -> int:
    s = []
    for ops in operations:
        if ops == '+':
            s.append(s[-1] + s[-2])
        elif ops == 'D':
            s.append(s[-1] * 2)
        elif ops == 'C':
            s.pop() 
        else:
            s.append(int(ops))   
    
    return sum(s)
```

