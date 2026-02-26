# Problem Statement

[Leetcode - Can Place Flowers](https://leetcode.com/problems/can-place-flowers/description/?envType=study-plan-v2&envId=leetcode-75)

You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in **adjacent** plots.

Given an integer array flowerbed containing **0's and 1's**, where *0 means empty and 1* means not empty, and an integer *n*, return **true** if n new flowers can be planted in the flowerbed without violating the no-adjacent-flowers rule and **false** otherwise.

## Examples

### Example 1

**Input**: flowerbed = [1,0,0,0,1], n = 1
**Output**: true

### Example 2

**Input**: flowerbed = [1,0,0,0,1], n = 2
**Output**: false

## Observation

We have to simply perform adjacent checks. To do this safely we can *pad* the array with a 0 at start and end. Why 0? Because our padding just means we have no flowers at the end. It's simply a place holder to perform adjacency checks safely.

But if we do not want to perform padding, we can coun the *max* number of flowers that can be placed. To do so we would check left and right boundaries with the following conditions:

- **Left** = i == 0 or flowerbed[i - 1] == 0
- **Right** = i == len(flowerbed) - 1 or flowerbed[i + 1] == 0

if they're satisfied, then we can place a flower and increment the count.

## Algorithm 1

1. Append 0s at the start and end of the array
2. Loop from i = 1 (one ahead of padding) till len(flowerbed) + 1.
3. Perform adjacency checks.
4. If i, i - 1 and i + 1 all have 0, then a flower can be placed.
5. Place the flower
6. Return n <= 0, this means all flowers have been placed inside the flowerbed.

## Algorithm 2

1. Set count = 0
2. Iterate over the list
3. compute left and right boundaries
4. Check if they match the conditions; i == 0 or i + 1 == 0, i == len(flowerbed) - 1 or i + 1 == 0
5. Place the flower, increment the count
6. If count exceeds n, we have placed flowers, return true
7. Return false otherwise

## Complexity

### For Algorithm 1

- **Time**: *O(n)*
- **Space**: *O(n)* as we make a copy of original array with appended 0s

### For Algorithm 2

- **Time**: *O(n)*
- **Space**: *O(1)*

## Code

### algorithm 1

```python

class Solution:
    def canPlaceFlowers(self, flowerbed: List[int], n: int) -> bool:
        s = len(flowerbed)
        flowerbed = [0] + flowerbed + [0]

        for i in range(1, s+1):
            if flowerbed[i] == flowerbed[i-1] == flowerbed[i+1] == 0:
                flowerbed[i] = 1
                n -= 1
        
        return n <= 0

```

### algorithm 2

```python

class Solution(object):
    def canPlaceFlowers(self, flowerbed, n):
        """
        :type flowerbed: List[int]
        :type n: int
        :rtype: bool
        """
        if n == 0:
            return True
        count = 0
        for i in range(len(flowerbed)):
            if flowerbed[i] == 0:
                left = (i == 0) or (flowerbed[i - 1] == 0)
                right = (i == len(flowerbed) - 1) or (flowerbed[i + 1] == 0)

                if left and right:
                    flowerbed[i] = 1
                    count += 1
                
                if count >= n:
                    return True
        return False

```

## Mistakes / Gotcha

- Impoper boundary checks
- Padding with -1. Padding should be done with 0 only
