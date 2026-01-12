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

### 1. Binary Tree Preorder Traversal

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

## B. Bottom-up DFS (Post-order)

**Intuition:** Manager waiting for reports. Cannot make decision until subordinate return results
**Mechanics:** using return value of recursive fn
**When to use:**

- Calculate height / depth (Height of me = max(l, r) + 1)
- Diameter of tree (pivot)
- subtree sum problems
- balanced tree

### 1. Binary Tree Postorder Traversal

**Problem:** You are given the `root` of a binary tree, return the postorder traversal of its nodes' values.

**Strategy:**

```
1. init result arr to track results
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

### 2. N-ary PostOrder Traversal

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

### 1. Binary Tree In-order Traversal

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
