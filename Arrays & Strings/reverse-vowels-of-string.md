# Problem Statement

[Leetcode - Resverse vowels of a string](https://leetcode.com/problems/reverse-vowels-of-a-string/?envType=study-plan-v2&envId=leetcode-75)

Given a string **s**, reverse **only** the *vowels* in s while keeping the other characters in their **original** positions.

## Examples

### Example 1

**Input**: "hello"
**Output**: "holle"

### Example 2

**Input**: "leetcode"
**Output**: "leotcede"

## Observation

- At a glance we can clearly see a two pointer approach works here. We start the pointers at opposite ends and swap only if we encounter a vowel.
- For checking vowels we can make a set of vowels and check the character at s[i] or s[j] if it is in the vowel or not
- We would also need to convert the string into a list as strings are **immutable** in python.

## Algorithm

1. Initialize l = 0, r = len(s) - 1
2. Initialize a set of vowels = set('aeiouAEIOU') and typecast s into list.
3. Loop until the condition l < r becomes false.
4. If the current character from the left is not a vowel, increment l
5. If the current character from the right is not a vowel, decrement r
6. If it is, swap and increment l, decrement r
7. Return ''.join(s_list)

## Complexity

- **Time**: *O(n)*
- **Space**: *O(n)*

## Code

```python

def reverse_vowels(s: str) -> str:
    vowels = set("aeiouAEIOU")
    s_list = list(s)
    left, right = 0, len(s) - 1

    while left < right:
        while left < right and s_list[left] not in vowels:
            left += 1
        while left < right and s_list[right] not in vowels:
            right -= 1
        if left < right:
            s_list[left], s_list[right] = s_list[right], s_list[left]
            left += 1
            right -= 1

    return "".join(s_list)

```

## Mistakes / Gotcha

- **ALWAYS** typecast to a list
- no left < right check in the inner while loops
- Forgetting to increment and decrement pointers
