# Problem Statement

[LeetCode - Decode String](https://leetcode.com/problems/decode-string/description/?envType=study-plan-v2&envId=leetcode-75)

Given an encoded string, return its decoded string.

The encoding rule is:**k[encoded_string]**, where the encoded*string inside the square brackets is being repeated exactly k times. Note that k is \_guaranteed* to be a positive integer.

You may assume that the input string is always valid; there are no extra white spaces, square brackets are well-formed, etc. Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there will not be input like 3a or 2[4].

The test cases are generated so that the length of the output will never exceed 105.

## Examples

### Example 1

**Input**: s = "3[a]2[bc]"
**Output**: "aaabcbc"

### Example 2

**Input**: s = "3[a2[c]]"
**Output**: "accaccacc"

## Observations

By observing the second example we can see we have to solve the inner brackets first. This means the outer ones can be solved only if the inner sub problem is solved. This pattern usually translates to recursion which uses the **call stack**. We can simulate the behaviour using a normal stack by following the below listed rules:

- Push into the stack everything except ]
- When ] is encountered we pop from the stack until it's corresponding [ is not found.
- Each popped character is stored in a substr.
- Then we pop for k.
- Then we append k \* substr into the stack.

## Algorithm

1. Init stack = []
2. Append in stack if not ]
3. If ] is found, start popping to first form the string (substr) within the `[]` and then for a integer k.
4. Append k \* substr in the stack.
5. Return the stack joined as string.

## Complexity

- **Time**: _O(n)_
- **Space**: _O(n)_

## Code

```python

class Solution:
    def decodeString(self, s: str) -> str:
        stack = []

        for i in range(len(s)):
            if s[i] != ']':
                stack.append(s[i])
            else:
                substr = ""
                while stack and stack[-1] != "[":
                    ch = stack.pop()
                    substr = ch + substr
                stack.pop() # POP the [

                k = ""
                while stack and stack[-1].isdigit():
                    k = stack.pop() + k
                stack.append(int(k) * substr)

        return "".join(stack)

```

## Mistakes / Gotchas

1. Not popping the closing ] after extracting the substr
2. Not converting k into an integer when multiplying.
