# Problem Statement

[Leetcode - Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)

Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the **root node reference** (possibly updated) of the BST.

The deletion has two stages:

1. Search for the node to remove.
2. If the node is found, delete it.

## Examples

**Input**: root = [5,3,6,2,4,null,7], key = 3
**Output**: [5,4,6,2,null,null,7]
**Explanation**:

- The node with value 3 is found and deleted.
- It has two children (2 and 4), so it is replaced by its inorder successor (4).
- [5,2,6,null,4,null,7] is also a valid answer.

**Input**: root = [5,3,6,2,4,null,7], key = 0
**Output**: [5,3,6,2,4,null,7]
**Explanation**: The tree does not contain a node with value 0, so it is returned unchanged.

**Input**: root = [], key = 0
**Output**: []

## Observations

Deleting a node from a BST has three cases, depending on how many children it has. The BST property (left < node < right) must still hold afterwards.

1. **Leaf node**: nothing hangs off it, so just remove it.
2. **One child**: the child (and its whole subtree) takes the node's place. The subtree is already on the correct side of the parent.
3. **Two children**: neither child can replace it directly without losing the other subtree. Replace the node's value with its **inorder successor** (smallest value in the right subtree), then delete that successor from the right subtree. The successor has no left child, so its deletion falls into case 1 or 2. The inorder predecessor (largest value in the left subtree) works equally well.

Each recursive call returns the new root of its subtree, and the caller must store it back into `root.left` or `root.right`. That reassignment is how the parent's pointer gets updated.

Cases 1 and 2 can be merged: `if not root.left: return root.right` returns `None` for a leaf.

## Algorithm

1. If `root` is `None`, return `None` (key not found)
2. If `key < root.val`, set `root.left = deleteNode(root.left, key)`
3. If `key > root.val`, set `root.right = deleteNode(root.right, key)`
4. Otherwise the node is found:
   - If it has no left child, return `root.right`
   - If it has no right child, return `root.left`
   - Otherwise find the successor (go right once, then left as far as possible)
   - Copy the successor's value into `root.val`
   - Set `root.right = deleteNode(root.right, successor.val)`
5. Return `root`

## Complexity

- **Time**: _O(h)_, where h is the height of the tree (O(log N) balanced, O(N) worst case)
- **Space**: _O(h)_ for the recursion stack

## Code

```python

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def deleteNode(self, root: TreeNode | None, key: int) -> TreeNode | None:
        if not root:
            return None

        if root.val > key:
            root.left = self.deleteNode(root.left, key)
        elif root.val < key:
            root.right = self.deleteNode(root.right, key)
        else:
            if root.left is None and root.right is None:
                return None
            if root.left is None:
                return root.right
            if root.right is None:
                return root.left

            succ = root.right
            while succ.left:
                succ = succ.left
            root.val = succ.val
            root.right = self.deleteNode(root.right, succ.val)

        return root

```

## Mistakes / Gotchas

1. Writing `return self.deleteNode(root.left, key)` in the search branches. This returns the subtree as the whole tree and drops the current node. It must be `root.left = self.deleteNode(root.left, key)`, then fall through to `return root`
2. In the two-children case, copying the successor's value but not deleting the successor node from the right subtree. This leaves a duplicate value in the tree
3. Looking for the successor in the left subtree. The inorder successor is the smallest node in the **right** subtree (the predecessor is the largest in the left)
4. Forgetting that the key may not exist. The recursion must bottom out at `None` and leave the tree unchanged
