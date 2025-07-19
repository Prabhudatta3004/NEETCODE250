# What is a Binary Tree?

A non linear data structure which resembles a Linked list in many ways, in terms of having connected objects. In a Binary tree, each object can either have 0,1,2 connected objects(nodes).

For leetcode purposes we need to know some types of Binary trees:

1. Full Binary Tree/ Strict Binary Tree: A tree in which each node either has 0 children or 2 but never 1. (My idea to remember this is a strict Indian family which goes for hum do hamare do or nothing).
2. Complete Binary Tree: All the nodes are completely filled in each level except the last level which is  being filled from left to right.
3. Perfect Binary Tree: All internal nodes have two children and all the nodes are in the same level.
4. Balanced Tree: Height of left subtree and right subtree has a difference of at most 1.

# Implementation of Tree (Python):

```python

```

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

```python

```

**INORDER:**

My way of remembering : In order is self explainable first the parent then comes the two children 

( NODE→LEFT CHILD→ RIGHT CHILD)

Now how to code Inorder:

We can do it recursively, check the figure below:

![image.png](attachment:15860e8e-dc62-4abc-b69a-1f3d5c92dd80:7782da19-5a83-495e-8b34-b4a9edfa162a.png)

```python

```

**POSTORDER:**

My way of remembering : Post order means whatever came after (post the) parent. 

( CHILD→ RIGHT CHILD→NODELEFT)

Now How to code Postorder:

We can do it recursively, check the figure below:

![image.png](attachment:15860e8e-dc62-4abc-b69a-1f3d5c92dd80:7782da19-5a83-495e-8b34-b4a9edfa162a.png)

```python

```

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
