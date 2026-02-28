# Problem Statement

[Leetcode - Product of array except self](https://leetcode.com/problems/product-of-array-except-self/description/?envType=study-plan-v2&envId=leetcode-75)

Given an integer array **nums**, return an array answer such that **answer[i]** is equal to the product of all the elements of nums **except nums[i]**.

The product of any prefix or suffix of nums is guaranteed to fit in a *32-bit integer*.

You must write an algorithm that runs in **O(n)** time and without using the **division** operation.

## Examples

### Example 1

**Input**: nums = [1,2,3,4]
**Output**: [24,12,8,6]

### Example 2

**Input**: nums = [-1,1,0,-3,3]
**Output**: [0,0,9,0,0]

## Observations

We have to skip the number i and perform multiplication with all elements to it's:

- right
- left

This can be done using an approach called *prefix sum*. Though counter intuitive, it will be actually prefix *multiply*. We will build 2 arrays whose length will be equal to the given nums array.

- One will have product of elements towards left of i
- One will have product of elements towards right of i

Then we simply multiple the two resulting arrays values (index wise) to get the output

## Algorithm

1. Construct the L product array using formula: **L[j] = nums[i-1] * L[i-1]**. The -ve signifies left element.
2. Construct the R product array using formula: **R[j] = nums[i+1] * R[i+1]**. The +ve signifies left element.
3. Perform index wise multiplication: **ans.append(L[i] * R[i])**
4. Return the answer.

## Complexity

- **Time**: *O(n)*
- **Space**: *O(n)* - we construct 2 n sized arrays, where n is the size of original input

## Code

```python

class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        L = [1] * n
        R = [1] * n

        j = 1
        for i in range(1, n):
            L[j] = nums[i-1] * L[i-1]
            j += 1
        
        j = n - 2
        i = n - 2

        while i >= 0:
            R[j] = nums[i+1] * R[i+1]
            i -= 1
            j -= 1
        
        ans = []

        for i in range(n):
            ans.append(L[i] * R[i])
        
        return ans
        
```

## Mistaks / Gotcha

1. Not doing i-1 or i+1 at their correct positions
2. Forgetting to decrement i inside the while loop.
