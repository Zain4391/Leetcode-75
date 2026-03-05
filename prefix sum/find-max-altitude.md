# Problem Statement

[Leetcode - Find the highest altitude](https://leetcode.com/problems/find-the-highest-altitude/description/?envType=study-plan-v2&envId=leetcode-75)

There is a biker going on a road trip. The road trip consists of **n + 1** points at different altitudes. The biker starts his trip on point 0 with altitude equal 0.

You are given an integer array gain of length n where **gain[i]** is the **net gain** in altitude between points i​​​​​​ and i + 1 for all (0 <= i < n). Return the highest altitude of a point.

## Examples

### Example 1

**Input**: gain = [-5,1,5,0,-7]
**Output**: 1
**Explanation**: The altitudes are [0,-5,-4,1,1,-6]. The highest is 1.

### Example 2

**Input**: gain = [-4,-3,-2,-1,4,3,2]
**Output**: 0
**Explanation**: The altitudes are [0,-4,-7,-9,-10,-6,-3,-1]. The highest is 0

## Observations

The key point to note here is that the given gain list is the **net** altitude. Which esentially means it is the difference between 2 **ACTUAL** altitudes. We have to return the max ACTUAL altitude.

Thus we can say:

$$ Actual Altitude [j] = gain[i] + Actual Altitude[j - 1] $$

We do j-1 as our inital point will have net altitude 0.

## Algorithm

1. Initialize altitude array of length len(gain) + 1.
2. Compute the altitude array using the provided formula.
3. Return max(altitude).

## Complexity

- **Time**: *O(n)*
- **Space**: *O(m)* - where m is the length of altitude array.

## Code

```python

class Solution:
    def largestAltitude(self, gain: List[int]) -> int:
        n = len(gain)
        N = n + 1
        alt = [0] * N

        j = 1
        for i in range(n):
            alt[j] = gain[i] + alt[j-1]
            j += 1
        
        return max(alt)

```

## Mistakes / Gotcha

1. Incorrect indexing of arrays
2. Not incrementing the j (placement) pointer.
