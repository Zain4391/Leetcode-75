# Problem Statement

[Leetcode - Longest Subarray of 1's After Deleting One Element](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/description/?envType=study-plan-v2&envId=leetcode-75)

Given a binary array nums, you should delete one element from it.

Return the size of the longest non-empty subarray containing only 1's in the resulting array. Return 0 if there is no such subarray.

## Examples

### Example 1

**Input**: nums = [1,1,0,1]
**Output**: 3
**Explanation**: After deleting the number in position 2, [1,1,1] contains 3 numbers with value of 1's.

### Example 2

**Input**: nums = [0,1,1,1,0,1,1,0,1]
**Output**: 5
**Explanation**: After deleting the number in position 4, [0,1,1,1,1,1,0,1] longest subarray with value of 1's is [1,1,1,1,1].

## Observations

Again, removing means we do not do the said operation, we track it. This is almost the same problem as max ones. Instead of variable k, the k here is one. Plus as we have to remove one one element at any case, our answer will be maxL - 1.

## Algorithm

1. Initialize removed = 0, l = 0 and maxL = 0
2. Iterate over the list, if nums[r] == 0, we flip it (just increment the counter).
3. If removed > 1, we check if our nums[l] has a zero, which would've been removed. Thus we decrement removed as it is no longer part of our window.
4. Update max length by using max(maxL, r-l+1)
5. Return maxL - 1

## Code

```python

class Solution:
    def longestSubarray(self, nums: List[int]) -> int:
        l = 0
        maxL = 0
        removed = 0

        for r in range(len(nums)):
            if nums[r] == 0:
                removed += 1
            
            if removed > 1:
                if nums[l] == 0:
                    removed -= 1
                l += 1
            
            maxL = max(maxL, r-l+1)
        return maxL-1

```

## Mistakes / Gotcha

1. Not returning maxL - 1
2. Not decrementing removed if nums[l] == 0. This needs to happen as element at l is no longer a part of our window.
