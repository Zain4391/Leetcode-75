# Problem Statement

[Leetcode - Remove Stars from String](https://leetcode.com/problems/removing-stars-from-a-string/description/?envType=study-plan-v2&envId=leetcode-75)

You are given a string s, which contains stars *.

In one operation, you can:

- Choose a star in s.
- Remove the closest non-star character to its left, as well as remove the star itself.
- Return the string after all stars have been removed.

**Note:**

- The input will be generated such that the operation is always possible.
- It can be shown that the resulting string will always be unique.

## Examples

**Input**: s = ``` "leet**cod*e" ```
**Output**: "lecoe"
**Explanation**: Performing the removals from left to right:

- The closest character to the 1st star is 't' in "leet**cod*e". s becomes "lee*cod*e".
- The closest character to the 2nd star is 'e' in "lee*cod*e". s becomes "lecod*e".
- The closest character to the 3rd star is 'd' in "lecod*e". s becomes "lecoe".
There are no more stars, so we return "lecoe".

## Observations

So this is a basic stack problem. For each ``` * ``` we have to remove the closest element to it's left. We can say that element will exist at the top of our stack as it will be the latest one encountered. So by using this logic, each ``` * ``` will have a corresponding left element as the question states input will be provided in such a way that the said operations can be performed.

## Algorithm

1. Init a stack st = []
2. Loop over the input
3. If s[i] a ``` * ```, pop from stack as it is the left most element.
4. If not, then push the element in the stack.
5. Return the string with no *.

## Complexity

- **Time**: *O(n)*
- **Space**: *O(n)*

## Code

```python

class Solution:
    def removeStars(self, s: str) -> str:
        st = []

        for i in range(len(s)):
            if s[i] != "*":
                st.append(s[i])
            if s[i] == "*":
                st.pop()
        
        if len(st) == 0:
            return ""
        
        return "".join(st)

```

## Mistakes / Gotchas

1. Not popping from the stack once the ```*``` has been encountered.
2. Not appending to the stack
