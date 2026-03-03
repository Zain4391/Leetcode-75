# Problem Statement

[Leetcode - Maximum Number of Vowels in a Substring of Given Length](https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/description/?envType=study-plan-v2&envId=leetcode-75)

Given a string **s** and an integer **k**, return the **maximum** number of **vowel** letters in any substring of *s* with length *k*.
Vowel letters in English are 'a', 'e', 'i', 'o', and 'u'

## Examples

### Example 1

**Input**: s = "abciiidef", k = 3
**Output**: 3
**Explanation**: The substring "iii" contains 3 vowel letters.

### Example 2

**Input**: s = "aeiou", k = 2
**Output**: 2
**Explanation**: Any substring of length 2 contains 2 vowels.

### Example 3

**Input**: s = "leetcode", k = 3
**Output**: 2
**Explanation**: "lee", "eet" and "ode" contain 2 vowels

## Observations

Like max average subarry, this is a rolling fixed length sliding window problem. We have to count vowels in the window and upon moving forward start dropping vowels from the left side (means decrease count).
The dropping logic is simply s[i - k] in v, then drop. This means the character (which will be a vowel) at i-k is not a part of our window now.

## Algorithm

1. Init v = set('aeiou')
2. Calculaate vowels for first window k.
3. Then from k till len(s), add vowel, remove vowel.
4. Update maximum.
5. Return maximum.

## Complexity

- **Time**: *O(n)*
- **Space**: *O(m)* - where m is the size of vowel set.

## Code

```python

class Solution:
    def maxVowels(self, s: str, k: int) -> int:
        v = ("aeiou")
        vc = 0
        maxV = 0

        for c in s[:k]:
            if c in v:
                vc += 1
        
        maxV = vc

        for i in range(k, len(s)):
            if s[i] in v:
                vc += 1
            if s[i - k] in v:
                vc -= 1
            maxV = max(maxV, vc)
        
        return maxV

```

## Mistakes / Gotcha

1. Do not complicate the Qs by using left and right pointer. Try looping till k and start the next one from k in such questions.
2. not doing i-k for dropping vowel
3. Not updating max.
