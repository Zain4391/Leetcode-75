# Problem Statement

[Leetcode - Move Zeros](https://leetcode.com/problems/move-zeroes/description/?envType=study-plan-v2&envId=leetcode-75)

Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Note that you must do this **in-place** without making a copy of the array.

## Examples

### Example 1

**Input**: nums = [0,1,0,3,12]
**Output**: [1,3,12,0,0]

### Example 2

**Input**: nums = [0]
**Output**: [0]

## Observations

As we have to solve this in-place, we can use two pointers approach. One will be the read pointer which scans the array and a write pointer which is used to write the number in place of zero and increment. By writing, I mean it will swap it with the number on the nums at read.

## Algorithm

1. Initialize w = 0
2. Loop for r from 1 to len(nums)
3. If nums[w] == 0 and nums[r] != 0, means we need to *write* the number at r. So we swap and increment w
4. Now there maybe a case where nums[w] != 0, for it we just increment w.
5. Return the modified list

## Complexity

- **Time**: *O(n)*
- **Space**: *O(1)*

## Code

```python

class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        w = 0
        for r in range(1, len(nums)):
            if nums[w] == 0 and nums[r] != 0:
                nums[r], nums[w] = nums[w], nums[r]
                w += 1
            if nums[w] != 0:
                w += 1
        return nums
        
```

## Mistakes / Gotcha

1. Initializing w with 1.
2. Not considering the case nums[w] != 0.
