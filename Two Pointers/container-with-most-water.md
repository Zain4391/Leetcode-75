# Problem Statement

[Leetcode - Container with most water](https://leetcode.com/problems/container-with-most-water/description/?envType=study-plan-v2&envId=leetcode-75)

You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are **(i, 0) and (i, height[i]).**
Find two lines that together with the x-axis form a container, such that the container contains the most water.
Return the **maximum** amount of water a container can store.

Notice that you may not slant the container.

## Examples

### Example 1

**Input**: height = [1,8,6,2,5,4,8,3,7]
**Output**: 49
**Explanation**: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

### Example 2

**Input**: height = [1,1]
**Output**: 1

## Observations

- If we think about this problem like a daily life task, we would realize that the water inside *any* container is restricted by the lower height/lower miniscus. That exact principle applies here. The minimum of the height is waht restricts the maximum area. Thus this becomes a greedy problem of **finding the minumum height which gives the maximum area**.

- Secondly, maximum amount of water can also be stored in a vertically long container. Thus it then becomes **finding the maximum height which gives the maximum area**.

Thus it is a greedy problem at heart.

Aree can be calculated by the formula:

$$ Area = min(height[l], height[r]) * abs(l-r) $$

After calculation we have to greedly find the height which maximuzes the area. This can be done using if then else checks.

## Algorithm

1. Initialize l = 0, r = len(heights) - 1
2. Calculate area using the formula provided in observation.
3. Update the maximum area.
4. If our height at l is less than height at r, we need to find the height l which can vertically maximize the area.
5. Same principle is applied for r.
6. Return max area.

## Complexity

- **Time**: *O(n)*
- **Space**: *O(1)*

## Code

```python

class Solution:
    def maxArea(self, height: List[int]) -> int:
        l = 0
        r = len(height) - 1
        maxA = 0

        while l < r:
            area = min(height[l], height[r]) * abs(l-r)
            maxA = max(area, maxA)

            if height[l] < height[r]:
                l += 1
            else:
                r -= 1
        
        return maxA

```

## Mistakes / Gotcha

1. Multiplying min height with -ve difference.
2. Not updating pointers
3. Updating pointers based on max area, which is incorrect.
