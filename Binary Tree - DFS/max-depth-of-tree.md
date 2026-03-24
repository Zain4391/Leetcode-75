# Problem Statement

[LeetCode - Max depth of binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/?envType=study-plan-v2&envId=leetcode-75)

Given the root of a binary tree, return its maximum depth.

A binary tree's maximum depth is the **number of nodes** along the longest path from the **root node down to the farthest leaf node**.

## Examples

### Example 1

**Input**: root = [3,9,20,null,null,15,7]
**Output**: 3

## Observations

Two things:

1. Number of nodes from root = Number of nodes in the path. The root can be a part subtree as well.
2. Farthest node = This means that we have to search the deepest point and then backtrack, esentially perform DFS.

## Algorithm

1. Base case, if root is null means no node, thus return 0.
2. Return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right))

## Complexity

- **Time**: _O(N)_
- **Space**: _O(N)_

## Code

```python

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right))

```

## Mistakes / Gotchas

1. Return 1 instead of 0 in the base case.
2. Not using + 1 to account for the actual root node itself.
