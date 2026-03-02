# Problem Statement

You are given an integer array nums consisting of n elements, and an integer k.

Find a contiguous **subarray** whose length is equal to k that has the maximum average value and return this value. Any answer with a calculation error less than $$ 10^5 $$ will be accepted.

## Examples

### Example 1

**Input**: nums = [1,12,-5,-6,50,3], k = 4
**Output**: 12.75000
**Explanation**: Maximum average is (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75

### Example 2

**Input**: nums = [5], k = 1
**Output**: 5.00000

## Observations

This is a *fixed* length sliding window problem. In such cases we can iterate over the window length which is k in this case. We have to find maximum average of a subarray. we can start by first calculating the average of the initial window. Then to calculate avrage of the next k length window we:

- In the cumulative sum, add nums[i]
- Then to drop previous number we can use nums[i-k]. Say i = 4 and k = 4. This drops the value at 0th index, which we do not need as it is not a part of the next window.

Using the above method we can calculate the avg of a window and keep tracking the maximum.

## Algorithm

1. Initialize cs = 0
2. Loop from 0 - k to find cumulative sum for first window
3. Find ma = cs / k.
4. Start looping from k all the way till len(nums).
5. Update ma using ma = max (ma, (cs / k)).
6. Return ma

## Complexity

- **Time**: *O(n)*
- **Space**: *O(1)*

## Code

```python

class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        cs = 0
        for i in range(k):
            cs += nums[i]
        
        ma = cs / k

        for i in range(k, len(nums)):
            cs += nums[i]
            cs -= nums[i-k]

            avg = cs / k
            ma = max(avg, ma)
        
        return ma

```

## Mistakes / Gotcha

1. Dividing with incorrect value.
2. Not updating the cs inside the second for loop, specifically by cs -= nums[i-k].
3. Not tracking max average.
