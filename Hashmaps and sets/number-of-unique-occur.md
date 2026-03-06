# Problem Statement

[Leetcode - Unique Number of Occurrences](https://leetcode.com/problems/unique-number-of-occurrences/description/?envType=study-plan-v2&envId=leetcode-75)

Given an array of integers arr, return **true** if the number of **occurrences** of each value in the array is **unique** or false otherwise.

## Examples

### Example 1

**Input**: arr = [1,2,2,1,1,3]
**Output**: true
**Explanation**: The value 1 has 3 occurrences, 2 has 2 and 3 has 1. No two values have the same number of occurrences.

## Observations

This is another hashmap + hashset in one problem:

1. The hashmap will be a frequency counter
2. Hashset will be used to detect duplicates.

## Algorithm

1. Init freq = {} and hashset.
2. Loop through the array and add elements to the hashmap.
3. Now loop through the hashmap's key and values. Check if a value is in the hashset, if it is, then return False.
4. If we do not return from loop, it means we had no duplicate occurences, thus return True

## Complexity

- **Time**: *O(n)*
- **Space**: *O(N)* - Where N = number of elements in hashmap + number of elements in hashset.

## Code

```python

class Solution:
    def uniqueOccurrences(self, arr: List[int]) -> bool:
        freq = {}

        for num in arr:
            freq[num] = freq.get(num, 0) + 1
        
        s = set()
        for key, value in freq.items():
            if freq[key] not in s:
                s.add(freq[key])
            else:
                return False
        
        return True

```

## Mistakes / Gotchas

1. Not using hashset for checking duplicates.
2. Not adding elements to hashmap properly.
