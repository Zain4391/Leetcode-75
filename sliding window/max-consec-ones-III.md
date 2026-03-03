# Problem Statement

[Leetcode - Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/?envType=study-plan-v2&envId=leetcode-75)

Given a binary array **nums** and an integer **k**, return the maximum number of **consecutive** 1's in the array if you can flip at most k 0's.

## Examples

### Example 1

**Input**: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
**Output**: 6
**Explanation**: [1,1,1,0,0,1,1,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

### Example 2

**Input**: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
**Output**: 10
**Explanation**: [0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

## Observations

In such Qs like:

- Flip ones
- Remove/drop something

We usually do not do the said operation. It is a fancy way of saying: "check for a specific condition" and increment a counter or a value.
By goind with the same logic, flip means we don't actually convert 0 to 1, we just track number of flipped zeros. Thus we can make an observation that our flipped zeros **CANNOT** be > K. This would indicate that out window needs shrinking. If our flipped ones are <= K we can expand the window.

## Algorithm

1. Initialize flipped = 0, l = 0 and maxL = 0
2. Iterate over the list, if nums[r] == 0, we flip it (just increment the counter).
3. If flipped > k, we check if our nums[l] has a zero, which would've been flipped. Thus we decrement flipped as it is no longer part of our window.
4. Update max length by using max(maxL, r-l+1)
5. Return maxL

## Complexity

- **Time**: *O(n)*
- **Space**: *O(1)*

## Code

```python

class Solution:
    def longestOnes(self, nums: List[int], k: int) -> int:
        l = 0
        flipped = 0
        
        maxL = 0
        for r in range(len(nums)):
            if nums[r] == 0:
                flipped += 1
            
            if flipped > k:
                if nums[l] == 0:
                    flipped -= 1
                l += 1
            
            maxL = max(maxL, r-l+1)
        return maxL

```

## Mistakes / Gotchas

1. Moving l by more than 1, this results in discarding potential 1s or 0s which can make up the answer.
2. Not decrementing flipped if nums[l] == 0. This needs to happen as element at l is no longer a part of our window.
