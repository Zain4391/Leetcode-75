# Problem Statement

[Leetcode - Is Subsequence](https://leetcode.com/problems/is-subsequence/description/?envType=study-plan-v2&envId=leetcode-75)

Given two strings **s** and **t**, return **true** if s is a subsequence of t, or **false** otherwise.

A **subsequence** of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not).

## Examples

### Example 1

**Input**: s = "abc", t = "ahbgdc"
**Output**: true

### Example 2

**Input**: s = "axc", t = "ahbgdc"
**Output**: false

## Observations

We have to match characters of s with t. Thus we go for a two pointer approach where *i* indexes s and *j* indexes t. For each matching character we will increment i, j increments nontheless as in-case of match we have to proceed and in-case of no match we have to proceed.
Nowo to detect if out subsequence has been formed, we employ the following logic:

- A subsequence can be formed iff i is equal to s' length. This means when i reaches len(s), an occurence of s has been found in t. Thus we return i == len(s) as our output.

## Algorithm

1. Initalize i = 0, j = 0
2. Set n = len(n) and m = len(t)
3. Loop until i < n and j < n
4. If match, increment i
5. Increment j unconditionally.
6. Return i == len(s)

## Complexity

- **Time**: *O(n)*
- **Space**: *O(1)*

## Code

```python

class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        n = len(s)
        m = len(t)

        i = j = 0

        while i < n and j < m:
            if s[i] == t[j]:
                i += 1
            j += 1
        
        return i == n

```

## Mistakes / Gotcha

1. Comparing i and j with eachother's lengths.
2. Incremnting j inside the if as well as outside, causing j to go up by 2.
3. Using or isntead of and in while's condition.
