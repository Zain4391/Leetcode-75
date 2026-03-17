# Problem Statement

[LeetCode - Reverse a LinkedList](https://leetcode.com/problems/reverse-linked-list/description/?envType=study-plan-v2&envId=leetcode-75)

Given the head of a singly linked list, **reverse** the list, and return the reversed list.

## Examples

### Example 1

**Input**: head = [1,2,3,4,5]
**Output**: [5,4,3,2,1]

## Observations

Simplest question in leetcode 75. We have to change direction of next pointers. One thing we have to consider is that once we do change the direction of pointer of a node say k, then we would have to track what is beyond the node k **before** changing it's direction. Thus we use temp to store what is at k.next before reversing k.next's direction.

## Algorithm

1. Check for empty list, return None.
2. Set curr = head, prev = None
3. While curr, we keep track of curr.next and set curr.next to previous, one after the other.
4. Update curr with t.
5. Return prev

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
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head:
            return None

        curr = head
        prev = None

        while curr:
            t = curr.next
            curr.next = prev
            prev = curr
            curr = t
        return prev

```

## Mistakes / Gotchas

1. Not doing curr = t, causing infinite loop.
2. Not doing t = curr.next **BEFORE** curr.next = prev.
