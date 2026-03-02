# Problem Statement

[Leetcode - Max number of k-sum pairs](https://leetcode.com/problems/max-number-of-k-sum-pairs/description/?envType=study-plan-v2&envId=leetcode-75)

You are given an integer array **nums** and an integer **k**.

In one operation, you can pick two numbers from the array whose sum equals k and remove them from the array.
Return the **maximum** number of operations you can perform on the array.

## Examples

### Example 1

**Input**: nums = [1,2,3,4], k = 5
**Output**: 2
**Explanation**: Starting with nums = [1,2,3,4]:

- Remove numbers 1 and 4, then nums = [2,3]
- Remove numbers 2 and 3, then nums = []
There are no more pairs that sum up to 5, hence a total of 2 operations.

### Example 2

**Input**: nums = [3,1,3,4,3], k = 6
**Output**: 1
**Explanation**: Starting with nums = [3,1,3,4,3]:

- Remove the first two 3's, then nums = [1,4,3]
There are no more pairs that sum up to 6, hence a total of 1 operation.

## Observations

If we observe the examples, this is **two sum** problem at heart. In two sum our hashmap would look like:

- Key = complement, value = index of the complement

This was due to the nature of that problem. Here we can use the same approach but with slight modification:

- Key = complement, value = Number of times/occurences

The occurences tell us the number of times a specific complement has occured in our list and can be used to *potentially* form target k.

The formula we will use for complement:

$$ Complement = K - nums[i] $$

Another approach can be to use sorting. We sort the array and then apply the same algorithm as two sum - II (input array is sorted), just with slight change: increment count as well if K-pair is found.

## Algorithms

### Algorithm 1

1. Initlaize a hashmap m = {}
2. Loop through the list and use the formula to calculate the complement.
3. Check if the complement is in the array **and** if it's value in hashmap is greater than 0.
4. If yes, then increment count and decrement compliment's value in the hashmap.
5. Otherwise increment the complement's value in the hashmap.
6. Return count.

### Algorithm 2

1. Sort the given array
2. Initialize two pointers L, R with values 0, len(nums) - 1 respectively.
3. If nums[L] + nums[R] == K, increment count, L and decrement R.
4. If nums[L] + nums[R] > K, decrement R.
5. nums[L] + nums[R] < K, increment L.
6. Return count.

## Complexity

### Approach 1

- **Time**: *O(n)*
- **Space**: *O(n)*, as in worse case we will end up storing all elements in the hashmap only once.

### Approach 2

- **Time**: *O(nlogn)*
- **Space** : *O(1)*

## Code

### Code 1

```python

class Solution:
    def maxOperations(self, nums: List[int], k: int) -> int:
        m = {}
        count = 0

        for num in nums:
            c = k - num

            if c in m and m[c] > 0:
                count += 1
                m[c] -= 1
            else:
                m[num] = m.get(num, 0) + 1
        
        return count

```

### Code 2

```python

class Solution:
    def maxOperations(self, nums: List[int], k: int) -> int:
        nums.sort()
        L = 0
        R = len(nums) - 1
        count = 0

        while L < R:
            val = nums[L] + nums[R]

            if val == k:
                count += 1
                L += 1
                R -= 1
            elif val > k:
                R -= 1
            else:
                L += 1
        
        return count

```

## Mistakes / Gotcha

1. Not initializing hashmap.
2. Doing m[c] = m.get(c, 0) + 1 instead of m[num] = m.get(num, 0) + 1
3. Not moving the pointers inwards.
