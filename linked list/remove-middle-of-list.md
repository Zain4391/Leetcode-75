# Problem Statement

[LeetCode - Remove Middle of Linked List](https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/description/?envType=study-plan-v2&envId=leetcode-75)

You are given the head of a linked list. Delete the middle node, and return the head of the modified linked list.

The middle node of a linked list of size **n** is the **⌊n / 2⌋th** node from the start using 0-based indexing, where ⌊x⌋ denotes the largest integer less than or equal to x.

For n = 1, 2, 3, 4, and 5, the middle nodes are 0, 1, 1, 2, and 2, respectively.

## Examples

### Example 1

**Input**: head = [1,3,4,7,1,2,6]
**Output**: [1,3,4,1,2,6]
**Explanation**:
The above figure represents the given linked list. The indices of the nodes are written below.
Since n = 7, node 3 with value 7 is the middle node, which is marked in red.
We return the new list after removing this node.

## Observations

We can use fast and slow pointer approach where the fast pointer moves one step ahead of slow pointer. The idea is that when the fast pointer has traversed the list, our slow pointer will be at the middle of the list.
One edge case is that the head maybe empty or can have only one element. For that we would have to return null.

## Algorithm

1. Init fast = head.next.next and slow = head
2. Loop until fast is not null and fast.next is not null. The second check is due to the fact that fast pointer moves one step ahead.
3. Move pointers.
4. Upon exiting the loop, adjust the pointers fo middle element.

## Complexity

- **Time**: _O(n)_
- **Space**: _O(1)_

## Code

```python

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def deleteMiddle(self, head: Optional[ListNode]) -> Optional[ListNode]:

        if not head or not head.next:
            return None

        slow = head
        fast = head.next.next

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

        slow.next = slow.next.next
        return head

```

## Mistakes / Gotchas

1. Initializing fast with head.next
2. Not moving the either pointer inside the loop.
3. returning the pointers itself instead of the head.
