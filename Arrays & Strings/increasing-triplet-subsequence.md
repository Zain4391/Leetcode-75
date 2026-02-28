# Problem Statement

[Leetcode - Increasing Treiplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/description/?envType=study-plan-v2&envId=leetcode-75)

Given an integer array nums, return true if there exists a **triple of indices (i, j, k)** such that **i < j < k and nums[i] < nums[j] < nums[k]**. If no such indices exists, return **false**.

## Examples

### Example 1

**Input**: nums = [1,2,3,4,5]
**Output**: true
**Explanation**: Any triplet where i < j < k is valid.

### Example 2

**Input**: nums = [5,4,3,2,1]
**Output**: false
**Explanation**: No triplet exists.

### Example 3

**Input**: nums = [2,1,5,0,4,6]
**Output**: true
**Explanation**: One of the valid triplet is (1, 4, 5), because nums[1] == 1 < nums[4] == 4 < nums[5] == 6.

## Observations

We have to find a *subsequence*. This means, the numbers won't necessarily be contigious. Thus we can track the shortest and middle numbers. If we find any other number bigger than those, we have found our subsequence.

We try to keep short as small as possible and middle as small as possible (but > short). Thus this is a greedy approach.

## Algorithm

1. Initialize short and middle variables to max value.
2. For each number, first compare if it is less that short. If so update short
3. For each number, secondly compare it to middle. If less, then update middle.
4. If both conditions are not satisfied, it means we have found the **third** biggest number and thus we return True.
5. If return inside the loop doesn't execute, it means we didn't find a valid solution, thus we return False.

## Complexity

- **Time**: *O(n)*
- **Space**: *O(1)*

## Code

```python

class Solution:
    def increasingTriplet(self, nums: List[int]) -> bool:
        short = float('inf')
        middle = float('inf')

        for curr in nums:
            if curr <= short:
                short = curr
            elif curr <= middle:
                middle = curr
            else:
                return True
        return False

```

## Mistakes / Gotcha

1. Using if for the second condition instead of elif.
2. Initializing short and middle without float typecasting
3. Initializing short and middle with -inf.
