# Problem Statement

[Leetcode - Find the Difference of Two Arrays](https://leetcode.com/problems/find-the-difference-of-two-arrays/description/?envType=study-plan-v2&envId=leetcode-75)

Given two 0-indexed **integer** arrays nums1 and nums2, return a list answer of size 2 where:

- **answer[0]** is a list of all distinct integers in nums1 which are not present in nums2.
- **answer[1]** is a list of all distinct integers in nums2 which are not present in nums1.

Note that the integers in the lists may be returned in any order.

## Examples

### Example 1

**Input**: nums1 = [1,2,3], nums2 = [2,4,6]
**Output**: [[1,3],[4,6]]
**Explanation**:
For nums1, nums1[1] = 2 is present at index 0 of nums2, whereas nums1[0] = 1 and nums1[2] = 3 are not present in nums2. Therefore, answer[0] = [1,3].
For nums2, nums2[0] = 2 is present at index 1 of nums1, whereas nums2[1] = 4 and nums2[2] = 6 are not present in nums1. Therefore, answer[1] = [4,6].

## Observations

We have to include *distinct* elements in our answer. This requires two sets, one for each array. Then we have to find if a number exists in in the other array. To do this efficiently, we use *hashset* here.

## Algorithm

1. Initialize ste1 and set2. Set2 holds nums1 elements and set1 holds nums2 elements.
2. Init two empty sets, v1 and v2.
3. Loop for both lists, check if the element is not in set1 or set2 and also not in v1 or v2 (for each element), then add it to answer.
4. Return the two lists.

## Complexity

- **TIme**: *O(n)*
- **Space**: *O(N)*, where N = number of elements in set1 + set2 + v1 + v2 and final answers.

## Code

```python

class Solution:
    def findDifference(self, nums1: List[int], nums2: List[int]) -> List[List[int]]:
        s1 = set(nums2)
        s2 = set(nums1)

        v1 = set()
        v2 = set()

        n1 = []

        for i in range(len(nums1)):
            if nums1[i] not in s1 and nums1[i] not in v1:
                n1.append(nums1[i])
                v1.add(nums1[i])
        n2 = []

        for i in range(len(nums2)):
            if nums2[i] not in s2 and nums2[i] not in v2:
                n2.append(nums2[i])
                v2.add(nums2[i])
        
        ans = []
        ans.append(n1)
        ans.append(n2)

        return ans

```

## Mistakes / Gotchas

1. Not making additional sets to check if an element is already included in answer.
2. Not looping over both lists
