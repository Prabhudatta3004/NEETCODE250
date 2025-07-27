# What is a Binary Tree?

A non linear data structure which resembles a Linked list in many ways, in terms of having connected objects. In a Binary tree, each object of class Node can either have 0,1,2 connected objects(nodes).

For leetcode purposes we need to know some types of Binary trees:

1. Full Binary Tree/ Strict Binary Tree: A tree in which each node either has 0 children or 2 but never 1. (My idea to remember this is a strict Indian family which goes for hum do hamare do or nothing).
2. Complete Binary Tree: All the nodes are completely filled in each level except the last level which is  being filled from left to right.
3. Perfect Binary Tree: All internal nodes have two children and all the leaf nodes are in the same level.
4. Balanced Tree: Height of left subtree and right subtree has a difference of at most 1.

# Implementation of Tree (Python):

At first we need a CreateNode class type which has 3 things the value of the node we have, the left child and the right child. Now we can create any tree dynamically by checking which child is None( that means it is not exisiting). I will be using Level order traversal to insert new nodes into the correct places.

```python
import collections

class CreateNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class BinaryTree:
    def __init__(self):
        self.root = None

    def add_nodes(self, val):
        node = CreateNode(val)

        if self.root is None:
            self.root = node
            return

        q = collections.deque([self.root])
        while q:
            current = q.popleft()

            if not current.left:
                current.left = node
                return
            else:
                q.append(current.left)

            if not current.right:
                current.right = node
                return
            else:
                q.append(current.right)

    def level_order(self):
        if not self.root:
            print("Tree is empty")
            return

        q = collections.deque([self.root])
        while q:
            node = q.popleft()
            print(node.val, end=' ')
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
```

How level order works in the above case is  we can try and think how we can insert new values in a tree. When we do it using a pen we go level by level to fill a tree. At first I created a node class which helps creating the tree object then I created a binary tree class which initialized a root (None). After that we can take In the values one by one and place it level by level. In add_nodes I have used level order traversal where the base case checks if the root is empty then the current val goes in there if not, then we push the root to a deque and then loop through that queue until we fill the value in. the checker checks if the left or right is empty if either of them are empty the current val can be placed there if both are not empty we can push them into the queue.

Mostly I believe, tree data structure is very visual, we need to draw the trees with an example to solve any question (so is the case for recursion!!)

# Most Important Concepts:

### TRAVERSALS:

Trees can be traversed in many ways:

1. Inorder
2. Preorder
3. postorder
4. level order

**PREORDER:**

My way of remembering : Imaging you are reading a sentence from left to right ( LEFT CHILD → NODE → RIGHT CHILD)

Now how to code Preorder:

We can do it recursively. check the figure shown below:

![image.png](attachment:15860e8e-dc62-4abc-b69a-1f3d5c92dd80:7782da19-5a83-495e-8b34-b4a9edfa162a.png)

In preorder traversal we need to print the left child first and then the node value and then the right child. this is the case for all the nodes. I am thinking of using recursion to do this. 

```python

def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        def dfs(root,res):
            if not root:
                return []
            res.append(root.val)
            dfs(root.left,res)
            dfs(root.right,res)
        result = []
        dfs(root, result)
        return result

```

**INORDER:**

My way of remembering : In order is self explainable first the parent then comes the two children 

( NODE→LEFT CHILD→ RIGHT CHILD)

Now how to code Inorder:

We can do it recursively, check the figure below:

![image.png](attachment:15860e8e-dc62-4abc-b69a-1f3d5c92dd80:7782da19-5a83-495e-8b34-b4a9edfa162a.png)

```python
 def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        def dfs(root,res):
            if root is None:

                return 
            dfs(root.left,res)
            res.append(root.val)
            dfs(root.right,res)
        res = []
        dfs(root, res)
        return res
```

**POSTORDER:**

My way of remembering : Post order means whatever came after (post the) parent. 

( CHILD→ RIGHT CHILD→NODELEFT)

Now How to code Postorder:

We can do it recursively, check the figure below:

![image.png](attachment:15860e8e-dc62-4abc-b69a-1f3d5c92dd80:7782da19-5a83-495e-8b34-b4a9edfa162a.png)

```python
def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        def dfs(root, res):
            if root is None:
                return
            dfs(root.left,res)
            dfs(root.right,res)
            res.append(root.val)

        res = []
        dfs(root,res)
        return res
```

---

# DFS AND BFS (THE ONLY THING THAT MATTERS)

HOW TO KNOW DFS?

Well basically when we have to traverse through all the nodes in a branch, backtrack and enter another branch and do the same, we use DFS. This is useful when the answer is far away from the root node.

DFS using stack internally via recursion or we can take up a stack and build the DFS logic (iterative).

 

Great for:

- Exploring all paths (path sum)
- Tree-based recursion problems
- Subtree checks, depth calculations

# TEMPLATE FOR DFS

DFS RECURSIVE:

```python
def dfs(node):
    if not node:
        return
    # preorder logic
    dfs(node.left)
    # inorder logic
    dfs(node.right)
    # postorder logic

```

DFS ITERATIVE:

```python
def iterative_dfs(root):
    if not root:
        return
    stack = [root]
    while stack:
        node = stack.pop()
        # Process node
        if node.right:
            stack.append(node.right)
        if node.left:
            stack.append(node.left)

```

HOW TO KNOW BFS?

when the answer lies closer to the root node. When we feel like we dont need to go through all the nodes, when level order traversals seems much more sensible.

BFS uses double ended queue which helps us to do level order traversal.

# BFS TEMPLATE (LEVEL ORDER)

```python
from collections import deque

def bfs(root):
    if not root:
        return []

    res = []
    q = deque()
    q.append(root)

    while q:
        level = []             # For storing the current level's nodes
        qlen = len(q)          # Number of nodes in the current level

        for _ in range(qlen):
            node = q.popleft()

            # Process current node
            level.append(node.val)

            # Push left and right children
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)

        res.append(level)      # After finishing this level
    return res

```

# QUESTIONS (NEETCODE 150)

**Invert Binary Tree**Solved

**You are given the root of a binary tree `root`. Invert the binary tree and return its root.**

**Example 1:**

[](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/ac124ee6-207f-41f6-3aaa-dfb35815f200/public)

`Input: root = [1,2,3,4,5,6,7]

Output: [1,3,2,7,6,5,4]`

MY INTUITION:

This problem involves visiting every node in the binary tree and swapping its left and right children, which clearly calls for a traversal over all nodes—making DFS a natural choice. The operation is simple: for each node, swap the left and right subtrees, then recursively apply the same to its children. While it might feel like an inorder traversal due to visiting nodes one by one, the specific order doesn't matter here; both preorder and postorder DFS are valid, as long as the swap occurs and all nodes are visited. DFS is ideal because it provides a clean, recursive way to handle this node-by-node transformation.

CODE:

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        
        ## This feels like we are traversing each child and reversing
        ## This needs to be done for all nodes, DFS comes to mind
        ## Now tha I look into the graphic I see that the left subtree and 
        ## right subtree are swapped and their children are also swapped

        ## The perfect traversal for such case feels like inorder where we 
        ## visit a node , swap the left child and the right child 
        ## then we move ahead 

        def dfs(node):
            if not node:
                return 
            node.left, node.right = node.right, node.left
            dfs(node.left)
            dfs(node.right)
            return root
        return dfs(root)
```

TC : O(N)

SC: O(H) : O(N)

# MAXIMUM DEPTH OF A BINARY TREE

**Given the `root` of a binary tree, return its depth.**

**The depth of a binary tree is defined as the number of nodes along the longest path from the root node down to the farthest leaf node.**

**Example 1:**

[](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/5ea6da77-7e43-43e0-dd9d-e879ca0b1600/public)

`Input: root = [1,2,3,null,null,4]

Output: 3`

INTUITION:

To find the maximum depth of a binary tree, we need to **explore all paths from the root to the leaf nodes**. This makes DFS a natural choice, as we recursively go down each subtree and calculate the depth along the way. At each node, we add `1` (for the current node) to the maximum depth of its left and right subtrees. When we hit a `None` (leaf’s child), we return 0 — indicating no depth beyond that point. Recursively combining these values as we return back up gives us the maximum depth.

CODE:

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        ## Since we are exploring the depths of the tree
        ## we need to move till the end leaf nodes.
        ## we can do that by going through each subtree
        ## and while we do so we can keep on adding +1 for the
        ## current visited node , this will give us the 
        ## depth of the tree
        ## when we reach the leaf node, it wont have any depth
        ## the value will be 0 ## we can return 1+ max(dfs(left), dfs(tright))

        ## By doing so for each node we can get the max depth and recursively
        ## we can calculate that and backtrack to the root node to
        ## get the final max depth of the tree

        def dfs(node):
            if not node:
                return 0
            left = dfs(node.left)
            right = dfs(node.right)
            return 1 + max(left, right)
        return dfs(root)
```

# DIAMETER OF A TREE

**The diameter of a binary tree is defined as the length of the longest path between *any two nodes within the tree*. The path does not necessarily have to pass through the root.**

**The length of a path between two nodes in a binary tree is the number of edges between the nodes.**

**Given the root of a binary tree `root`, return the diameter of the tree.**

**Example 1:**

[](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/90e1d7a0-4322-4c5d-c59b-dde2bf92bb00/public)

`Input: root = [1,null,2,3,4,5]

Output: 3`

MY INTUITION:

To find the diameter of a binary tree—the longest path between any two nodes—we need to explore all nodes and compute the depth of their left and right subtrees. At each node, the longest path that passes through it is the sum of the depths of its left and right children. This makes DFS the ideal approach, as we can recursively compute subtree depths and track the maximum `left + right` value across all nodes. The final result is the largest such value found during traversal. Since the diameter is measured in terms of **edges**, we don’t add 1 when summing `left + right`—only when returning depth upward in the recursive stack.

CODE:

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        ## The question asks for the diameter of the binary tree
        ## Which gives the longest path, since we have to go through
        ## All the paths, we can go for a DFS, now for the diameter
        ## I was thinking of using dfs to get the maximum depth
        ## then we can use and keep on adding the left subtree and 
        ## right subtree depths and store it in the result

        res = 0
        def dfs(node):
            if not node:
                return 0
            nonlocal res
            left = dfs(node.left)
            right = dfs(node.right)
            res = max(res, left+right) 
            # Adding the left and right subtree length
            return 1 + max(left,right)

        dfs(root)
        return res

```

# BALANCED BINARY TREE

**Given a binary tree, return `true` if it is height-balanced and `false`otherwise.**

**A height-balanced binary tree is defined as a binary tree in which the left and right subtrees of every node differ in height by no more than 1.**

**Example 1:**

[](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/c19c3727-ea28-416c-3873-79ee75f2b400/public)

`Input: root = [1,2,3,null,null,4]

Output: true`

MY INTUITION:

# Same TREE

**Same Binary Tree**Solved

**Given the roots of two binary trees `p` and `q`, return `true` if the trees are equivalent, otherwise return `false`.**

**Two binary trees are considered equivalent if they share the exact same structure and the nodes have the same values.**

**Example 1:**

[](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/e78fc10c-4692-471f-5261-61e9be4f3a00/public)

`Input: p = [1,2,3], q = [1,2,3]

Output: true`

MY INTUITION:
Since we are comparing all the nodes of each tree, we are recursively comparing the subtrees till the end. The comparison is simple node to node , left to left child and right to right child comparison. So I can easily do it via DFS

TC : O(N)

SC: O(H)

```python
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        def dfs(p, q):
            if not p and not q:
                return True
            if not p or not q:
                return False
            if p.val != q.val:
                return False
            return dfs(p.left, q.left) and dfs(p.right, q.right)
        
        return dfs(p, q)

```

# Subtree of another Tree

**Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of `subRoot`and `false` otherwise.**

**A subtree of a binary tree `tree` is a tree that consists of a node in `tree`and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.**

**Example 1:**

[](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/2991a77a-9664-46ed-528d-019e392f7400/public)

`Input: root = [1,2,3,4,5], subRoot = [2,4,5]

Output: true`

MY INTUITION:

to determine if one tree is a subtree of another, we perform a standard DFS traversal on the main tree (`root`) and, at each node, check if the subtree starting from that node matches the `subRoot`. This is done using a helper function that recursively compares the current node and all of its children for both trees. If we find any node in the main tree where the structure and values of the subtree exactly match `subRoot`, we return `True`. This approach works because DFS allows us to explore every possible starting point in the main tree where a match might occur.

CODE:

```python
class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        # Helper function to check if two trees are exactly the same
        def issame(t1, t2):
            if not t1 and not t2:
                return True
            if not t1 or not t2:
                return False
            if t1.val != t2.val:
                return False
            return issame(t1.left, t2.left) and issame(t1.right, t2.right)
        
        # Main DFS to traverse the root tree
        def dfs(node):
            if not node:
                return False
            if issame(node, subRoot):
                return True
            return dfs(node.left) or dfs(node.right)
        
        return dfs(root)
```

# LCA of BST

**Given a binary search tree (BST) where all node values are *unique*, and two nodes from the tree `p` and `q`, return the lowest common ancestor (LCA) of the two nodes.**

**The lowest common ancestor between two nodes `p` and `q` is the lowest node in a tree `T` such that both `p` and `q` as descendants. The ancestor is allowed to be a descendant of itself.**

**Example 1:**

[](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/2080ee6a-3d27-4cd5-0db2-07672ead8200/public)

`Input: root = [5,3,8,1,4,7,9,null,2], p = 3, q = 8

Output: 5`

MY intuition

In a BST, we know that the lower values will always lie to the left subtree and the higher values

to the right subtree. So by checking the max values between the two node values if It is less then the root we go to the left subtree and min of two nodes if > than the root then recurse in the right subtree.

CODE:

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def lowestCommonAncestor(self, root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
        if(max(p.val,q.val) < root.val):
            return self.lowestCommonAncestor(root.left, p , q)
        elif(min(p.val,q.val)>root.val):
            return self.lowestCommonAncestor(root.right, p , q)
        else:
            return root

```

# BT LEVEL ORDER:

**Given a binary tree `root`, return the level order traversal of it as a nested list, where each sublist contains the values of nodes at a particular level in the tree, from left to right.**

**Example 1:**

[](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/a4639809-0754-4eda-221f-a4cd58bd9c00/public)

`Input: root = [1,2,3,4,5,6,7]

Output: [[1],[2,3],[4,5,6,7]]`

My intuition 

My intuition is simple , I will go for a breadth first search. I will go level order BFS , I will use a dequeue and pop and check for the left and right child.

CODE:

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        queue = collections.deque()
        queue.append(root)
        res = []
        while queue:
            queue_length = len(queue)
            level = []
            for i in range(queue_length):
                node = queue.popleft()
                if not node:
                    return
                level.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            res.append(level)
        return res
```

# Binary Tree Right View:

**You are given the `root` of a binary tree. Return only the values of the nodes that are visible from the right side of the tree, ordered from top to bottom.**

**Example 1:**

[](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/d348893a-8917-456c-9599-c405cfc4e000/public)

`Input: root = [1,2,3]

Output: [1,3]`

My Intuition:

The first thing that I can think of is that when we check the right view, in best case we can have a perfect BT and we can go for just the right side child nodes of each node . But in worse cases I believe we need to think of a skewed tree with all left children which will require the entire change in logic. 

As we can see, when viewed from right side we are seeing all the end elements or nodes of each level. We can simply go for a BFS level order traversal and return the last element of level.

CODE:

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        queue = collections.deque()
        queue.append(root)
        res = []

        while queue:
            queue_length = len(queue)
            
            for level_nodes in range(queue_length):
                node = queue.popleft()
                if level_nodes == queue_length-1:
                    res.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        return res
```

# GOOD NODES IN BINARY TREE:

Given a binary tree `root`, a node *X* in the tree is named **good** if in the path from root to *X* there are no nodes with a value *greater than* X.

Return the number of **good** nodes in the binary tree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/04/02/test_sample_1.png)

```
Input: root = [3,1,4,3,null,1,5]
Output: 4
Explanation: Nodes in blue aregood.
Root Node (3) is always a good node.
Node 4 -> (3,4) is the maximum value in the path starting from the root.
Node 5 -> (3,4,5) is the maximum value in the path
Node 3 -> (3,1,3) is the maximum value in the path.
```

My Intuition:

Good nodes are those nodes which are greater than their predecessors. I like to go for a depth first traversal, notsince we are looking at the family structure and not level wise. But let me have a walkthrough at first I will start with the root node if the val at root is max then root is a good node, which it is now I can store the max_value and move to the next node of left subtree and check if the val of the visited node on left subtree is ≥ max_val then yes we increment the counter, we can go through left subtree and then right subtree and then we can end after we move out of the boundaries.

CODE:

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def goodNodes(self, root: TreeNode) -> int:
        counter = 0
        def dfs(node, max_val):
            nonlocal counter
            if not node:
                return None
            if node.val >= max_val:
                counter +=1
            max_val = max(max_val, node.val)
            dfs(node.left, max_val)
            dfs(node.right, max_val)
        
        dfs(root, float('-inf'))
        return counter
```

# Validate BST

**Given the `root` of a binary tree, return `true` if it is a valid binary search tree, otherwise return `false`.**

**A valid binary search tree satisfies the following constraints:**

**The left subtree of every node contains only nodes with keys less than the node's key.The right subtree of every node contains only nodes with keys greater than the node's key.Both the left and right subtrees are also binary search trees.**

- The left subtree of every node contains only nodes with keys **less than** the node's key.
- The right subtree of every node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees are also binary search trees.

**Example 1:**

[](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/18f9a316-8dc2-4e11-d304-51204454ac00/public)

`Input: root = [2,1,3]

Output: true`

My Intuition:

In a valid Binary Search Tree, all nodes in the left subtree must have values **less than** the current node, and all nodes in the right subtree must have values **greater than** the current node. This condition must hold **recursively** for every node in the tree. So, to validate a BST, I can use a DFS approach where I pass down the allowable `min` and `max` bounds for each node. For the left child, I update the upper bound (`max = current.val`), and for the right child, I update the lower bound (`min = current.val`). If at any point a node violates these constraints — i.e., its value is not within the valid `(min, max)`range — the tree is not a valid BST.

CODE:

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        def dfs(node,minimum, maximum):
            if not node:
                return True
            if not (minimum < node.val < maximum):
                return False
            return dfs(node.left, minimum, node.val) and dfs(node.right, node.val, maximum)

        return dfs(root, float('-inf'), float('inf'))

            
```

# Kth Smallest Element in BST

**Given the `root` of a binary search tree, and an integer `k`, return the `kth`smallest value (1-indexed) in the tree.**

**A binary search tree satisfies the following constraints:**

**The left subtree of every node contains only nodes with keys less than the node's key.The right subtree of every node contains only nodes with keys greater than the node's key.Both the left and right subtrees are also binary search trees.**

- The left subtree of every node contains only nodes with keys **less than** the node's key.
- The right subtree of every node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees are also binary search trees.

**Example 1:**

[](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/02eca3db-f72f-4277-7134-faec4f02e500/public)

`Input: root = [2,1,3], k = 1

Output: 1`

My Intuition:

Since we have the smaller elements in the left subtree, we can directly traverse to the the left child and keep on comparing with the current_low value and if the value of the child is lower we increment a counter , when the counter hits k we just return the result. But now that I can see the structure of the BST, if we traverse inorder we can easily get the sorted values and we can keep on visiting the sorted values upto k times. It can be done using DFS on each node.

CODE:

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        counter = 0
        res = root.val

        def dfs(node):
            nonlocal counter, res
            if not node:
                return 
            dfs(node.left)
            counter +=1
            if counter == k:
                res = node.val
                return
            dfs(node.right)
        dfs(root)
        return res
```

# Build BT from Preorder and inorder

**You are given two integer arrays `preorder` and `inorder`.**

**`preorder` is the preorder traversal of a binary tree`inorder` is the inorder traversal of the same treeBoth arrays are of the same size and consist of unique values.**

- `preorder` is the preorder traversal of a binary tree
- `inorder` is the inorder traversal of the same tree
- Both arrays are of the same size and consist of unique values.

**Rebuild the binary tree from the preorder and inorder traversals and return its root.**

**Example 1:**

[](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/938c14d3-6669-47ab-924b-a1a08640f200/public)

`Input: preorder = [1,2,3,4], inorder = [2,1,3,4]

Output: [1,2,3,null,null,null,4]`

My intuition:

# MAXIMUM PATH SUM

**Given the `root` of a *non-empty* binary tree, return the maximum path sumof any *non-empty* path.**

**A path in a binary tree is a sequence of nodes where each pair of adjacent nodes has an edge connecting them. A node can *not* appear in the sequence more than once. The path does *not* necessarily need to include the root.**

**The path sum of a path is the sum of the node's values in the path.**

**Example 1:**

[](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/9896b041-9021-44c2-ab3e-5cff76adf100/public)

`Input: root = [1,2,3]

Output: 6`

My intuition:

The problem wants me to find the maximum path sum for any path in the tree and return it. Since we need to traverse through all paths recursively, I think Depth first traversal is the best option for this problem. Now let me go node by node at the root node, I expect to get a maximum path sum from either left or right subtree and added to its val we can return the max_path_sum. The same thing can be done recursively for both the subtrees in order to find the max_path_sum for that particular subtree. My DFS function should be able to check whether a particular recursion/subtree has the max_sum if yes we store that value as max_sum (maybe a global variable) and then the dfs function will add the node.val + max of either left or right subtree. But if any path_sum gets me a negative value that won’t be useful since that will reduce my path sum, so I need to make sure that I take only the positive path_sum from the sub_trees.

CODE:

```python
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        max_sum = float('-inf')

        def dfs(node):
            nonlocal max_sum
            if not node:
                return 0
            left = max(dfs(node.left),0)
            right = max(dfs(node.right),0)
            max_sum = max(max_sum, node.val+left+right)

            return node.val + max(left, right)
        
        dfs(root)
        return max_sum

```

# Serialize and Deserialize a BT

**Implement an algorithm to serialize and deserialize a binary tree.**

**Serialization is the process of converting an in-memory structure into a sequence of bits so that it can be stored or sent across a network to be reconstructed later in another computer environment.**

**You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure. There is no additional restriction on how your serialization/deserialization algorithm should work.**

**Note: The input/output format in the examples is the same as how NeetCode serializes a binary tree. You do not necessarily need to follow this format.**

**Example 1:**

[](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/a9dfb17f-70e9-42a3-ba97-33cfd82f6100/public)

`Input: root = [1,2,3,null,null,4,5]

Output: [1,2,3,null,null,4,5]`

MY INTUITION:
