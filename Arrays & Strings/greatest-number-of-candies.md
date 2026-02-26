# Problem Statement

[Leetcode - Kids with greatets number of candies](https://leetcode.com/problems/kids-with-the-greatest-number-of-candies/description/?envType=study-plan-v2&envId=leetcode-75)

There are n kids with candies. You are given an integer array candies, where each candies[i] represents the number of candies the ith kid has, and an integer extraCandies, denoting the number of extra candies that you have.

Return a boolean array result of length n, where result[i] is true if, after giving the ith kid all the extraCandies, they will have the greatest number of candies among all the kids, or false otherwise.

Note that multiple kids can have the greatest number of candies

## Examples

### Example 1

**Input**: candies = [2,3,5,1,3], extraCandies = 3
**Output**: [true,true,true,false,true]
**Explanation**: If you give all extraCandies to:

- Kid 1, they will have 2 + 3 = 5 candies, which is the greatest among the kids.
- Kid 2, they will have 3 + 3 = 6 candies, which is the greatest among the kids.
- Kid 3, they will have 5 + 3 = 8 candies, which is the greatest among the kids.
- Kid 4, they will have 1 + 3 = 4 candies, which is not the greatest among the kids.
- Kid 5, they will have 3 + 3 = 6 candies, which is the greatest among the kids.

### Example 2

**Input**: candies = [4,2,1,1,2], extraCandies = 1
**Output**: [true,false,false,false,false]
**Explanation**: There is only 1 extra candy.
Kid 1 will always have the greatest number of candies, even if a different kid is given the extra candy.

## Observations

A pretty straightforward question. We need to find the maximum candy initially. Then compare current candy at ith index + extra to the max value.

## Algorithm

1. Find maximum candy.
2. Iterate over the candies list.
3. Check candies[i] + extra > max_candy, if so then append True to the result otherwise False
4. If our candies[i] + extra = max_candy, then we will also append True as specified in example 1's explaination.

## Complexity

- **Time**: *O(n)*
- **Space**: *O(1)*

## Code

```python

class Solution:
    def kidsWithCandies(self, candies: List[int], extraCandies: int) -> List[bool]:
        n = len(candies)
        result = []
        max_c = max(candies)

        for c in candies:
            if c + extraCandies >= max_c:
                result.append(True)
            else:
                result.append(False)
        
        return result

```

## Mistakes / Gotchas

- Do not foget the *=* in if **c + extraCandies >= max_c**
