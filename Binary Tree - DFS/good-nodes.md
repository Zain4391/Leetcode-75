# Problem Statement

[LeetCode - Number of Good Nodes](https://leetcode.com/problems/count-good-nodes-in-binary-tree/description/?envType=study-plan-v2&envId=leetcode-75)

Given a binary tree **root**, a node X in the tree is named **good** if in the path from root to X there are no nodes with a value greater than X.

Return the number of good nodes in the binary tree.

## Examples

### Example 1

**Input**: root = [3,1,4,3,null,1,5]
**Output**: 4
**Explanation**: Nodes in blue are good.
Root Node (3) is always a good node.
Node 4 -> (3,4) is the maximum value in the path starting from the root.
Node 5 -> (3,4,5) is the maximum value in the path
Node 3 -> (3,1,3) is the maximum value in the path.

## Observations

We would have to maintain the running maximum as we perform DFS. If current node X.val >= max so far then X is a good node. So it is a simple DFS with an additional check for good value condition.

## Algorithm

1. Init self.count = 0
2. Define dfs function with the conditions
   - IF not node, return
   - IF X.val >= MAX_SO_FAR, self.count += 1

3. Call the dfs inside the main function with root and root.val as arguments
4. Return self.count

## Complexity

- **Time**: _O(N)_
- **Space**: _O(log n) for balanced*. *O(N) for skwed_

## Code

```python

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def goodNodes(self, root: TreeNode) -> int:
        self.count = 0

        def DFS(root, max_so_far):
            if not root:
                return
            if root.val >= max_so_far:
                self.count += 1
            new_max = max(root.val, max_so_far)
            DFS(root.left, new_max)
            DFS(root.right, new_max)
        DFS(root, root.val)
        return self.count

```

## Mistakes / Gotchas

1. Initializing self.count = 1.
2. Not calling DFS with the right arguments
