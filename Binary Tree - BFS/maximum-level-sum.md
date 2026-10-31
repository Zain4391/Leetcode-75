# Problem Statement

[Leetcode - Maximum Level Sum of a Binary Tree](https://leetcode.com/problems/maximum-level-sum-of-a-binary-tree/description/?envType=study-plan-v2&envId=leetcode-75)

Given the root of a binary tree, the level of its root is 1, the level of its children is 2, and so on.
Return the **smallest** level x such that the sum of all the values of nodes at level x is maximal.

## Examples

**Input**: root = [1,7,0,7,-8,null,null]
**Output**: 2
**Explanation**:

- Level 1 sum = 1.
- Level 2 sum = 7 + 0 = 7.
- Level 3 sum = 7 + -8 = -1.

So we return the level with the maximum sum which is level 2.

## Observations

Return statement of the question can throw off people. It simply wants us to return the level which has the maximum sum. We have to trace sum at each level and when a new max sum is encountered, we update the sum and the level.

## Algorithm

1. Set max sum to a very large value
2. Initialize a queue as it is a BFS problem
3. Set q = [root]
4. Current level = 1, and answer level = -1
5. While q is not empty, track sum at each level
6. After sum for a level is calculated, update max sum and answer level
7. Return answer level

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
from collections import deque
class Solution:
    def maxLevelSum(self, root: TreeNode | None) -> int:
        max_sum = float('-inf')
        q = deque([root])
        ans_lvl = -1
        lvl = 1

        while q:
            s = 0
            for _ in range(len(q)):
                node = q.popleft()
                s += node.val

                if node.right:
                    q.append(node.right)
                if node.left:
                    q.append(node.left)
            if s > max_sum:
                max_sum = s
                ans_lvl = lvl
            lvl += 1

        return ans_lvl

```

## Mistakes / Gotchas

1. Returning the max sum. Question wants the level at which sum is max
2. Not re-setting sum = 0 at the start of new level. This causes overflow of previous level's values into new ones
