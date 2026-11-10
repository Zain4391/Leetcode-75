# Problem Statement

[LeetCode - Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/?envType=study-plan-v2&envId=leetcode-75)

Given an integer array `nums` and an integer `k`, return the `k`th largest element in the array.

It is the kth largest in sorted order, not the kth distinct element. Duplicates count.

Constraints: `1 <= k <= nums.length <= 10^5`, `-10^4 <= nums[i] <= 10^4`.

## Examples

```
Input:  nums = [3, 2, 1, 5, 6, 4], k = 2
Output: 5

Input:  nums = [3, 2, 3, 1, 2, 4, 5, 5, 6], k = 4
Output: 4
```

## Observations

- Sorting works in O(n log n), but we don't need a full order, only the kth position.
- A min-heap of size `k` always holds the k largest values seen so far.
- The root of that heap is the smallest of the top k, which is the kth largest overall.
- Any new value smaller than the root cannot be in the top k, so it gets pushed and immediately popped.
- Alternative: build a max-heap of all n items and pop `k - 1` times. The root is then the answer. This is better only when `k` is close to `n`.
- Quickselect solves it in O(n) average time, and is the usual follow-up.

## Algorithm

Min-heap of size k:

1. Create an empty min-heap `h`.
2. For each `x` in `nums`:
   1. Push `x` onto `h`.
   2. If `len(h) > k`, pop the root (the smallest).
3. Return `h[0]`.

Dry run on `[3, 2, 1, 5, 6, 4]`, `k = 2`:

| x   | Heap after push | Pop?   | Heap after |
| --- | --------------- | ------ | ---------- |
| 3   | `[3]`           | no     | `[3]`      |
| 2   | `[2, 3]`        | no     | `[2, 3]`   |
| 1   | `[1, 3, 2]`     | pops 1 | `[2, 3]`   |
| 5   | `[2, 3, 5]`     | pops 2 | `[3, 5]`   |
| 6   | `[3, 5, 6]`     | pops 3 | `[5, 6]`   |
| 4   | `[4, 6, 5]`     | pops 4 | `[5, 6]`   |

Final root is `5`, the 2nd largest.

## Complexity

| Approach                | Time                       | Space        |
| ----------------------- | -------------------------- | ------------ |
| Min-heap of size k      | O(n log k)                 | O(k)         |
| Max-heap, pop k-1 times | O(n + k log n)             | O(n)         |
| Sort                    | O(n log n)                 | O(1) or O(n) |
| Quickselect             | O(n) average, O(n^2) worst | O(1)         |

## Code

```python
import heapq

def find_kth_largest(nums, k):
    h = []
    for x in nums:
        heapq.heappush(h, x)
        if len(h) > k:
            heapq.heappop(h)
    return h[0]
```

Max-heap variant (negate values, since `heapq` is min-only):

```python
def find_kth_largest_maxheap(nums, k):
    h = [-x for x in nums]
    heapq.heapify(h)              # O(n)
    for _ in range(k - 1):
        heapq.heappop(h)
    return -h[0]
```

Built-in shortcut:

```python
heapq.nlargest(k, nums)[-1]
```

## Mistakes / Gotchas

- `heapq` is a min-heap only. For a max-heap, negate on the way in and negate again on the way out.
- A heap list is not sorted. Only `h[0]` is guaranteed to be the smallest.
- Use a min-heap for the kth largest, not a max-heap. The min-heap evicts the smallest of the top k, which keeps the size at `k`.
- Pop only when `len(h) > k`. Popping at `== k` leaves the heap one element short.
- Duplicates count as separate elements, so there is no deduplication.
- Return `h[0]` here. In Top K Frequent Elements you return the whole heap instead.
- In the max-heap variant, pop `k - 1` times, not `k`. Then read the root.
- Don't forget to negate the result in the max-heap variant (`-h[0]`).
