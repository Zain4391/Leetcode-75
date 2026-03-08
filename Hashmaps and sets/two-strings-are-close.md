# Problem Statement

[Leetcode - Determine if Two Strings Are Close](https://leetcode.com/problems/determine-if-two-strings-are-close/description/?envType=study-plan-v2&envId=leetcode-75)

Two strings are considered close if you can attain one from the other using the following operations:

- Operation 1: Swap any two existing characters.
For example, abcde -> aecdb
- Operation 2: Transform every occurrence of one existing character into another existing character, and do the same with the other character.
For example, aacabb -> bbcbaa (all a's turn into b's, and all b's turn into a's)
You can use the operations on either string as many times as necessary.

Given two strings, **word1** and **word2**, return true if **word1** and **word2** are close, and false otherwise.

## Examples

### Example 1

**Input**: word1 = "abc", word2 = "bca"
**Output**: true
**Explanation**: You can attain word2 from word1 in 2 operations.
Apply Operation 1: "abc" -> "acb"
Apply Operation 1: "acb" -> "bca"

### Example 2

**Input**: word1 = "a", word2 = "aa"
**Output**: false
**Explanation**: It is impossible to attain word2 from word1, or vice versa, in any number of operations.

### Example 3

**Input**: word1 = "cabbba", word2 = "abbccc"
**Output**: true
**Explanation**: You can attain word2 from word1 in 3 operations.
Apply Operation 1: "cabbba" -> "caabbb"
Apply Operation 2: "caabbb" -> "baaccc"
Apply Operation 2: "baaccc" -> "abbccc"

## Observations

This is a tricky question and the explaination does not make it any easier. We have to esentially look for the following:

1. If two words' character sets are equal.
2. If the frequencies of their respective characters are equal. To check sort and compare the key's values.

If the above two conditions suffice, the string is close.

## Algorithm

1. Init two hashmaps, have and need.
2. Fill them in using word2 and word1 respectively [examples show that word1 is being transformed into word2]
3. Check if both hashmaps have same keys
4. Check if both hashmaps have same values for their keys.
5. Return True otherwise false.

## Complexity

- **Time**: *O(n)* - you might think the sorted() on .values() dominates, but again, at most 26 values, so that sort is O(26 log 26) = O(1). The bottleneck is just iterating through the strings, so it's O(n).
- **Space**: *O(1)* - the hashmaps can only ever have at most 26 keys (lowercase English letters), so it's constant space regardless of input size

## Code

```python

class Solution:
    def closeStrings(self, word1: str, word2: str) -> bool:
        have = {}
        for ch in word2:
            have[ch] = have.get(ch, 0) + 1
        
        need = {}
        for ch in word1:
            need[ch] = need.get(ch, 0) + 1
            
        cond1 = set(have) == set(need)
        cond2 = sorted(have.values()) == sorted(need.values())

        return cond1 and cond2

```

## Mistakes / Gotchas

1. Conditionally building the second hashamp leading to incorrect hashmap forming or the entire word1 not being traversed.
2. Comparing dicts using *>=* which is **INVALID** in python.
