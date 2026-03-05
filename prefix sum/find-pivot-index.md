# Problem Statement

[Leetcode - Find pivot index](https://leetcode.com/problems/find-pivot-index/description/?envType=study-plan-v2&envId=leetcode-75)

Given an array of integers **nums**, calculate the pivot index of this array.

The **pivot index** is the index where the **sum** of all the numbers *strictly* to the **left** of the index is equal to the sum of all the numbers *strictly* to the index's **right**.

If the index is on the left edge of the array, then **the left sum is 0** because there are no elements to the left. This also applies to the **right** edge of the array.

Return the leftmost pivot index. If no such index exists, return **-1**.

## Examples

### Example 1

**Input**: nums = [1,7,3,6,5,6]
**Output**: 3
**Explanation**:
The pivot index is 3.
Left sum = nums[0] + nums[1] + nums[2] = 1 + 7 + 3 = 11
Right sum = nums[4] + nums[5] = 5 + 6 = 11

### Example 2

Input: nums = [1,2,3]
Output: -1
Explanation:
There is no index that satisfies the conditions in the problem statement.

## Observations

This is a pre-req type problem for product of array except self. We can make 2 arrays, L and R. Then we:

- compute sums to the left of i and place them in L
- For sum to th right we can place them in R.

Then for each element of L and R we check

- **L[i] == R[i].**

If satisfied, we return i.

But we can solve it without requiring extra memory. They key to understand it is the fact that

- **Total = left_sum + nums[i] + right_sum**, where nums[i] is a *candidate* for a pivot index.

Thus by observing this we realize we can just keep a running sum, left_sum, and compute right sum with it. Then if sums at any point become equal we return that index.

## Algorithm

### Algorithm 1

1. Init L and R with len(n) and 0s.
2. Compute L with formula L[j] = nums[i - 1] + L[j - 1].
3. Compute R with formula R[j] = nums[i + 1], R[j + 1]. Before it set j and i to be n - 2.
4. Loop over L and R, check for pivot condition and return index.
5. Otherwise return -1.

### Algorithm 2

1. Compute overall sum of the array
2. Set left_sum to be 0.
3. loop over the givne nums array.
4. Compute right sum by the formula right_sum = total - left_sum - nums[i]
5. If left_sum == right_sum, return the index.
6. Update left_sum
7. If no common sum found then return -1.

## Complexity

### Approach 1

- **Time**: *O(n)*
- **Space**: *O(n)*

### Approach 2

- **Time**: *O(n)*
- **Space**: *O(1)*

## Code

### Code for Algorithm 1

```python
class Solution:
    def pivotIndex(self, nums: List[int]) -> int:
        n = len(nums)
        L = [0] * n
        R = [0] * n

        j = 1

        for i in range(1, n):
            L[j] = nums[i - 1] + L[j - 1]
            j += 1
        
        j = n - 2
        i = n - 2

        while i >= 0:
            R[j] = nums[i + 1] + R[j + 1]
            j -= 1
            i -= 1
        
        for i in range(n):
            if L[i] == R[i]:
                return i
        return -1

```

### Code for Algorithm 2

```python

class Solution:
    def pivotIndex(self, nums: List[int]) -> int:
        total = sum(nums)
        left = 0

        for i in range(len(nums)):
            right = total - left - nums[i]

            if right == left:
                return i
            left += nums[i]
        
        return -1

```

## Mistakes / Gotchas

1. Not updating j when computing L and R.
2. Not updating i when computing R.
3. Not update left when using the second approach.
