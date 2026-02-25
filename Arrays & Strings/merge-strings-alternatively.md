# Problem Statement

[LeetCode — Merge Strings Alternately](https://leetcode.com/problems/merge-strings-alternately/description/?envType=study-plan-v2&envId=leetcode-75)

You are given two strings **word1** and **word2**. Merge the strings by adding letters in alternating order, starting with word1. If a string is longer than the other, append the additional letters onto the end of the merged string.

Return the *merged* string.

## Examples

### Example 1

Input: word1 = "abc", word2 = "pqr"
Output: "apbqcr"
Explanation: The merged string will be merged as so:
word1:  a   b   c
word2:    p   q   r
merged: a p b q c r

### Example 2

**Input**: word1 = "ab", word2 = "pqrs"
**Output**: "apbqrs"
**Explanation**: Notice that as word2 is longer, "rs" is appended to the end.
word1:  a   b
word2:    p   q   r   s
merged: a p b q   r   s

## Observations

word1 and word2 are not of the same length. The problem is a modified form of **Merge** function of merge sort.

## Algorithm

We will solve this using two pointer approach

1. Initialize i and j to be 0. They will be used to index the word strings
2. Use *len()* function to obtain each word's length
3. Conditional Loop which termnates if i and j go out of bounds
4. As we have stated before, words can be of different length, so we add the remaining word's characters at the end using Conditional Loops

## Complexity

- **Time**: As we traverse the word1 and word2 strings only once, we have a Time Complexity of *O(n)*
- **Space**: Only variables are initialized, thus constant space *O(1)*

## Code

```python

class Solution:
    def mergeAlternately(self, word1: str, word2: str) -> str:
        i = 0
        j = 0
        n = len(word1)
        m = len(word2)
        ans = ''

        while i < n and j < m:
            ans += word1[i]
            i += 1
            ans += word2[j]
            j += 1
        
        while i < n:
            ans += word1[i]
            i += 1
        
        while j < m:
            ans += word2[j]
            j += 1
        
        return ans

```

## Mistakes / Gotchas

- Invalid while loop conditions
- Use proper length check (i < n for word1 and j < m for word2) **I made this MISTAKE!**
- Increment i and j after appending to ans.
