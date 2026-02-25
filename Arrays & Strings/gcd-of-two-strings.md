# Problem Statement

For two strings **s** and **t**, we say "**t divides s**" if and only if **s = t + t + t + ... + t + t** (i.e., t is concatenated with itself one or more times).

Given two strings **str1** and **str2**, return the largest string **x** such that x *divides* both str1 and str2.

## Examples

### Example 1

**Input**: str1 = "ABCABC", str2 = "ABC"

**Output**: "ABC"

### Example 2

**Input**: str1 = "LEET", str2 = "CODE"

**Output**: ""

## Observations

We need to find a string x which can be repeated to re-construct both str1 and str2. This means str1 and str2 must have the same character structures.

How can we determine that both str1 and str2 have *similar* character structures?

``` bash
# Valid
str1 = "ABABAB"
str2 = "ABAB"

# Invalid
str1 = "AB"
str2 = "ABCABC"

```

- from the above snippet we can see that in valid case, if we do str1 + str2 and str2 + str1, we end up having the same character representaion.

Let's continue with the valid case.

When multiple strings are given, determining whether they share a common repeating pattern requires identifying the length of the repeating pattern for each string.

For example,

```bash

str1 = "ABCABC" → The repeating pattern is "ABC" with a length of 3.
str2 = "ABC" → The repeating pattern is "ABC" with a length of 3.

str1 = "ABABAB" → The repeating pattern is "AB" with a length of 2.
str2 = "ABAB" → The repeating pattern is "AB" with a length of 2.

```

**In this case, the length of the common repeating pattern X must divide both len(str1) and len(str2).**
Furthermore, since we need to return the largest string, **we must return the string with the maximum length that divides both strings, which corresponds to the greatest common divisor (GCD) of their lengths.**

## Algorithm

1. Check for character pattern st1 + str2 == str2 + str1
2. Return empty string if false
3. Now move to GCD calculation. This can be done using the min_value = min(len(str1), len(str2)) and looping backwards. Why loop backwards? Because we want to find the **Greatest** common divisor.
4. We check if there is a number between min_value and 0 which divides the length of both strings. If so we have found our GCD! If not then max length of repeating pattern is 1.
5. Return the max string now: str1[: GCD(len(str1), len(str2))]

## Complexity

- **Time**: *O(n + m)*
- **Space**: *O(n + m)*

## Code

```python

class Solution:
    def gcdOfStrings(self, str1: str, str2: str) -> str:
        if str1 + str2 != str2 + str1:
            return ""
        
        def GCD(l1, l2):
            min_val = min(l1, l2)
            for i in range(min_val, 0, -1):
                if l1 % i == 0 and  l2 % i == 0:
                    return i
            return 1
        
        return str1[:GCD(len(str1), len(str2))]

```

## Mistakes / Gotcha

- Compare the length % i to 0, not to each other.
- Do not forget the same character repetition pattern (str1 + str2 == str2 + str1) condition
