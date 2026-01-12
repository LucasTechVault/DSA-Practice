# Trees

- graph with no cycles
- always (N - 1) edges

## A. Top-Down DFS (Pre-order)

**Intuition:** Traveller going down tree, info from parent to children
**Mechanics:** Pass params into recursive fn
**When to use:**

- path sum (tracking sum going down)
- printing values / serialization
- checking if path exist

### A1. Binary Tree Preorder Traversal

**Problem:** You are given the `root` of a binary tree, return the preorder traversal of its nodes' values.

**Strategy:**

```
1. init result arr to track results
2. identify pre-order: process -> left -> right
3. define recursive traverse(node)
    - base case -> return None
```

**Code:**

```
def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
    result = []
    def traverse(node):
        # base case
        if node is None: # reached deepest, return
            return

        # processing case
        # pre means root, left, right
        # so we process first
        result.append(node.val)

        # recursive case
        traverse(node.left)
        traverse(node.right)

    traverse(root)
    return result
```

### A2. Invert Binary Tree

**Problem:** You are given the `root` of a binary tree root. Invert the binary tree and return its root.

**Example:**

```
Input: root = [1,2,3,4,5,6,7]
Output: [1,3,2,7,6,5,4]
```

**Strategy:**

```
1. Perform Top-down traversal (process, left, right)
    - invert then traverse
2. define recursive invert_and_traverse(node)
3. invert by using node.l, node.r = node.r, node.l
```

**Code:**

```
def invertTree(root):
    def invert_and_traverse(node):
        # base case
        if not node:
            return None

        # process first
        node.left, node.right = node.right, node.left

        # traverse
        invert_and_traverse(node.left)
        invert_and_traverse(node.right)

    invert_and_traverse(root)
    return root
```

### A3. Same Binary Tree

**Problem:**
Given the roots of two binary trees `p` and `q`, return `true` if the trees are equivalent, otherwise return `false`.

Two binary trees are considered equivalent if they share the exact same structure and the nodes have the same values.

**Example:**

```
Input: p = [1,2,3], q = [1,2,3]
Output: true
```

**Strategy:**

```
- traverse 2 trees top-down instead of 1
    - top-down means process -> traverse

- recursive fn must take 2 roots
1. define traverse_and_check_same(tree1, tree2)
2. 3 scenarios to process (check)
    1. both node at null - valid
    2. either node at null - invalid
    3. node val mismatch - invalid
3. traverse & check return (both.left) and (both.right)
```

**Code:**

```
def isSameTree(p, q):
    def traverse_and_check(node1, node2):
        # base case 1 -> both empty
        if not node1 and not node2:
            return True

        # base case 2 -> either 1 empty first
        if not node1 or not node2:
            return False

        # base case 3 -> val mismatch
        if node1.val != node2.val:
            return False

        # Traverse & check
        return (
            traverse_and_check(node1.left, node2.left)
            and
            traverse_and_check(node1.right, node2.right)
        )
    return traverse_and_check(p, q)
```

### A4. Subtree of Another Tree

**Problem:**
Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of `subRoot` and `false` otherwise.

A subtree of a binary tree tree is a tree that consists of a node in tree and all of this node's descendants. The tree tree could also be considered as a subtree of itself.

**Example:**

```
Input: root = [1,2,3,4,5], subRoot = [2,4,5]
Output: true
```

**Strategy:**

```
1. Top-down (process -> traverse)
2. Process phase -> 2 checks
    1. if subRoot is None -> valid (empty subTree is part of Tree)
    2. if root is None -> invalid (empty tree cannot be duplicated)
3. def isSameTree(p, q) recursive helper
4. process & traverse at same time
```

**Code:**

```
def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
    # edge case 1 -> empty subRoot is part of Tree
    if subRoot is None:
        return True

    # edge case 2 -> empty Root cannot be contained
    if root is None:
        return False

    # Helper to process
    def isSameTree(p, q):
        if not p and not q:
            return True

        if not p or not q:
            return False

        if p.val != q.val:
            return False

        return(
            isSameTree(p.left, q.left) and
            isSameTree(p.right, q.right)
        )

    # traverse & process
    # isSameTree initial check + recursive traverse & process
    return (
        isSameTree(root, subRoot) or
        self.isSubtree(root.left, subRoot) or
        self.isSubtree(root.right, subRoot)
    )
```

### A5. Merge Two Binary Trees

**Problem:**
You are given two binary trees `root1` and `root2`.

Imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not. You need to merge the two trees into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of the new tree.

Return the merged tree.

Note: The merging process must start from the root nodes of both trees.
**Example:**

```
Input: root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
Output: [3,4,5,5,4,null,7]
```

**Strategy:**

```
1. Top-down (process then traverse) -> define traverse_and_merge(node1, node2)
2. Need a reference tree to save value, use tree1
3. Processing case
    1. node1 exist, node2 doesnt -> return node1 (doesnt matter, using node1)
    2. node2 exist, node1 doesnt -> return node2 (use node2 as node1)
    3. both nodes exist -> node1.val += node2.val
    * we need to return, cannot modify in-place due to recursive stack
        - if use node1 = TreeNode(node2.val) -> no effect (at stack level)
4. traverse & process, merge into tree1
    - node1.left = traverse_and_merge(node1.left, node2.left)
    - node1.right = traverse_and_merge(node1.right, node2.right)
```

**Code:**

```
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        def traverse_and_merge(node1, node2):
            # base case 1 - node1 empty
            if node1 is None:
                return node2 # use node2

            # base case 2 - node2 empty
            if node2 is None:
                return node1 # just use node1

            # base case 3 - both nodes exist
            node1.val += node2.val

            # Traverse & merge into node1.left
            node1.left = traverse_and_merge(node1.left, node2.left)

            # Traverse & merge into node1.right
            node1.right = traverse_and_merge(node1.right, node2.right)

            # return merged node
            return node1

        # traverse & merge
        return traverse_and_merge(root1, root2)
```

## B. Bottom-up DFS (Post-order)

**Intuition:** Manager waiting for reports. Cannot make decision until subordinate return results
**Mechanics:** using return value of recursive fn
**When to use:**

- Calculate height / depth (Height of me = max(l, r) + 1)
- Diameter of tree (pivot)
- subtree sum problems
- balanced tree

### B1. Binary Tree Postorder Traversal

**Problem:** You are given the `root` of a binary tree, return the postorder traversal of its nodes' values.
**Strategy:**

```
1. init result arr to track resultfs
2. identify post-order: left -> right -> process
3. define recursive traverse(node)
    - base case -> return None
```

**Code:**

```
def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
    result = []

    def traverse(node):
        # Base Case -> reach end
        if node is None:
            return

        # Processing Case
        # PostOrder is left, right, root (process)
        # So we traverse first
        traverse(node.left)
        traverse(node.right)

        # Process after traversing
        result.append(node.val)

    traverse(root)
    return result
```

### B2. N-ary PostOrder Traversal

**Problem:**
You are given the `root` of an n-ary tree, return the postorder traversal of its nodes' values.

Nary-Tree input serialization is represented in their level order traversal. Each group of children is separated by the null value (See examples)
**Example:**

```
Input: root = [1,null,3,2,4,null,5,6]
Output: [5,6,3,2,4,1]
```

**Strategy:**

```
1. Identify post-order (traverse -> process)
    - N-ary, not possible for In-order
2. init results arr -> store results
3. define recursive traverse(node)
    - Base case -> if not node, return None
4. traverse then process
```

**Code:**

```
def postorder(self, root: 'Node') -> List[int]:
    result = []

    def traverse(node):
        # base case
        if not node:
            return

        # traverse
        def traverse(node):
            for child in node.children:
                traverse(child)

        # process
        result.append(node.val) # eventually will be child

    traverse(root)
    return result
```

### B3. Maximum Depth of Binary Tree

**Problem:**
Given the `root` of a binary tree, return its depth.

The depth of a binary tree is defined as the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example:**

```
Input: root = [1,2,3,null,null,4]
Output: 3
```

**Intuition:**

- cannot solve height until have children's height

1. In order to get height, need child's height
   - in fact, need max(left_height, right_height)
2. define traverse_and_get_height(node)
   - Base case -> node is None -> return 0
     - 0 because None node is not a valid level
   - get left height
   - get right height
     return 1 + max(left, right)
     **Code:**

```
def maxDepth(root):
    def traverse_and_get_height(node):
        # base case
        if not node:
            return 0

        # traverse & get height first
        left_height = traverse_and_get_height(node.left)
        right_height = traverse_and_get_height(nodoe.right)

        # process height
        return 1 + max(left_height, right_height)

    return traverse_and_get_height(root)
```

### B4. Diameter of Binary Tree

**Problem:** The diameter of a binary tree is defined as the length of the longest path between any two nodes within the tree. The path does not necessarily have to pass through the root.

The length of a path between two nodes in a binary tree is the number of edges between the nodes. Note that the path can not include the same node twice.

Given the root of a binary tree `root`, return the diameter of the tree.

**Example:**

```
Input: root = [1,null,2,3,4,5]
Output: 3
```

**Strategy:**

```
1. init self.max_diameter = 0 to keep track of largest diameter seen so far
2. Identify largest diameter occurs at pivot point
3. pivot point = left_height + right_height
4. init recursive traverse_and_find_diameter(node)
    4.1 base case -> if not node, return None
    4.2 recurse for left_height, right_height
    4.3 update diameter max(self.max_diam, lh + rh)
    4.4 return 1 + max(lh, rh) for recursive height imbued
```

**Code:**

```
def diameterOfBinaryTree(root):
    self.max_diameter = 0
    def traverse_and_find_height(node):
        # base case
        if not node:
            return None

        # traverse
        left_height = traverse_and_find_height(node.left)
        right_height = traverse_and_find_height(node.right)

        # process
        self.max_diameter = max(self.max_diameter, left_height + right_height)

        # return for recursion
        return 1 + max(left_height, right_height)
```

### B5. Balanced Binary Tree

**Problem:**
Given a binary tree, return `true` if it is height-balanced and `false` otherwise.

A height-balanced binary tree is defined as a binary tree in which the left and right subtrees of every node differ in height by no more than 1.

**Example:**

```
Input: root = [1,2,3,null,null,4]
Output: true
```

**Strategy:**

```
1. Same idea as finding max_depth, we need left and right height (post-order -> traverse then process)
2. Problem: qns wants us to return boolean
    - finding height, requires returning int (height)
    - soln: use -1 to indicate 'breach'
3. As usual, def recurse traverse_and_find_height(node)
    - extra check -> if height == -1, immediately return (optimization)
    - else, process after traverse as required
4. check balanced height abs(left-right) <= 1
```

**Code:**

```
def isBalanced(root):
    def traverse_and_find_height(node):
        # base case
        if not node:
            return 0

        # traverse
        left_height = traverse_and_find_height(node.left)
        if left_height == -1:
            return -1

        right_height = traverse_and_find_height(node.right)
        if right_height == -1:
            return -1

        # process - check balanced
        if abs(left_height - right_height) > 1:
            return -1

        # recursive return for height
        return 1 + max(left_height, right_height)
    return traverse_and_find_height(root) != -1
```

## C. Scanner Pattern (Level-order BFS)

**Intuition:** Scanning a document, level by level
**Mechanics:** Iterative using queue (deque)
**When to use:**

- right-side / left-side view
- find largest value in each row
- shortest path (min-depth) - sometimes faster than DFS

## D. In-Order Traversal (BST Special)

**Intuition:** Flattening tree into line
**Mechanic:** Plain old in-order traversal
**When to use:**

- exclusively for BST
- validating BST
- Finding Kth smallest element in BST

### D1. Binary Tree In-order Traversal

**Problem:**
You are given the root of a binary tree, return the inorder traversal of its nodes' values.

**Example:**

```
Input: root = [1,2,3,4,5,6,7]
Output: [4,2,5,1,6,3,7]
```

**Strategy:**

```
1. Identify In-order = left -> root -> right
2. init result arr to track values
3. define traverse(node) dfs recursion
    - Base case -> if not node -> return None
    - recurse (left, process, right)
```

**Code:**

```
def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
    result = []
    def traverse(node):
        # Base case -> reached end return
        if not node:
            return

        # Processing case
        # In-order means left -> root -> right
        traverse(node.left) # no return value needed, just traverse to left-most
        result.append(node.val)
        traverse(node.right)

    traverse(root)
    return result
```
