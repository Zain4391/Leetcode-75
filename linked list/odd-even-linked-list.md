# Problem Statement

[Leetcode - Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/?envType=study-plan-v2&envId=leetcode-75)

Given the **head** of a singly linked list, **group** all the nodes with **odd** indices together followed by the nodes with **even** indices, and return the reordered list.

The **first node** is considered odd, and the **second node** is even, and so on.

Note that the relative order inside both the even and odd groups should remain as it was in the input.

You must solve the problem in O(1) extra space complexity and O(n) time complexity.

## Examples

### Example 1

Input: head = [1,2,3,4,5]
Output: [1,3,5,2,4]

## Observations

This may seem daunting at first, but it can be solved by first solving it using extra space and then noticing that we alternatively modify the next pointers of odd and even nodes.
TO determine **when** we need to change odd node's next or even node's next we will use a flag, where 0 will mean we are at the odd node and 1 will mean we are at the even node. Then at the end we will have to join the the list's final odd with the list's first even.
There is an edge case where in case of an even list, odd can be None. Thus we track the last odd as well which will be used to connect with the first even.

## Algorithm

1. Initialize dummy node, for safe return.
2. Set odd = head and even = head.next and flag = 0 (starting index is odd)
3. Set dummy.next = odd
4. Initialize LO (last odd) and FE (first even) with odd and even respectively.
5. Loop until both pointers reach None.
6. If flag = 0, change odd.next and move odd. If odd exists we update last odd.
7. Else do the same but for even.
8. Outside the loop we join the last odd with next even
9. Return dummy.next (points to modified list's head)

## Complexity

- **TIme**: _O(N)_
- **Space**: _O(1)_

## Code

```python

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def oddEvenList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head:
            return None

        dummy = ListNode()

        odd = head
        dummy.next = odd
        even = head.next
        FE = even
        LO = odd
        flag = 0 # 0 for odd, 1 for even

        while odd and even:
            if flag == 0:
                odd.next = even.next
                odd = even.next
                if odd:
                    LO = odd
                flag = 1
            else:
                even.next = odd.next
                even = odd.next
                flag = 0


        LO.next = FE
        return dummy.next


```

## Mistakes / Gotchas

1. Not checking for odd being None.
2. Not maintaining first even and last odd pointers.
