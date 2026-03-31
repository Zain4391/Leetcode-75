# Problem Statement

[LeetCode - Leaf-Similar Trees](https://leetcode.com/problems/leaf-similar-trees/description/?envType=study-plan-v2&envId=leetcode-75)

Consider all the leaves of a binary tree, from left to right order, the values of those leaves form a leaf value sequence.
Two binary trees are considered leaf-similar if their leaf value sequence is the same.

Return **true** if and only if the two given trees with head nodes root1 and root2 are leaf-similar.

## Examples

### Example 1

**Input**: root1 = [3,5,1,6,2,9,8,null,null,7,4], root2 = [3,5,1,6,7,4,2,null,null,null,null,null,null,9,8]
**Output**: true

## Observations

This is a DFS question again. We have to find the deepest node which has no children and include it in our sequence. BFS will **fail** to preserve the order of the child nodes thus it is invalid here.

## Algorithm

1. Initialize two lists, a1 and a2.
2. Perform DFS with the base case; if not root we return, if root has not left or right child, include it in sequence.
3. DFS to the left
4. DFS to the right
5. Call dfs on root1, self.a1
6. Call dfs on root2, self.a2
7. Return self.a1 == self.a2

## Complexity

- **Time**: _O(N + M), where N and M are number of nodes in root1 and root2_
- **Space**: _O(N + M)_

## Code

```python

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def leafSimilar(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> bool:
        self.a1 = []
        self.a2 = []

        def dfs(root, lst):
            if not root:
                return
            if not root.left and not root.right:
                lst.append(root.val)
                return
            dfs(root.left, lst)
            dfs(root.right, lst)

        dfs(root1, self.a1)
        dfs(root2, self.a2)

        return self.a1 == self.a2

```

## Mistakes / Gotchas

1. Doing BFS instead of DFS.
2. Not passing the lists when calling DFS.
