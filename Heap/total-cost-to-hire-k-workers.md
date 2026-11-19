# Problem Statement

[LeetCode - Total Cost to hire k workers](https://leetcode.com/problems/total-cost-to-hire-k-workers/description/?envType=study-plan-v2&envId=leetcode-75).

You are given a 0-indexed integer array `costs`, where `costs[i]` is the cost of hiring the `i`-th worker, and two integers `k` and `candidates`.

You must hire exactly `k` workers in `k` sessions. In each session:

- Look at the first `candidates` workers and the last `candidates` workers of the remaining array.
- Hire the worker with the lowest cost among them. If there is a tie, hire the one with the smallest index.
- If fewer than `candidates` workers remain on a side, consider all the remaining workers on that side.
- A worker can be hired only once.

Return the total cost of hiring exactly `k` workers.

## Examples

**Example 1**

```
costs = [17,12,10,2,7,2,11,20,8], k = 3, candidates = 4
Output: 11
```

- Session 1: candidates are `[17,12,10,2]` and `[2,11,20,8]`. The minimum 2 appears at index 3 and index 5, so index 3 wins the tie. Cost 2.
- Session 2: remaining is `[17,12,10,7,2,11,20,8]`. Hire the 2 at original index 5. Cost 2.
- Session 3: remaining is `[17,12,10,7,11,20,8]`. Hire the 7 at original index 4. Cost 7.
- Total = 2 + 2 + 7 = 11.

**Example 2**

```
costs = [1,2,4,1], k = 3, candidates = 3
Output: 4
```

- Session 1: all four workers are candidates. Hire index 0 (cost 1, ties with index 3 but has the smaller index).
- Session 2: remaining `[2,4,1]`. Hire cost 1.
- Session 3: remaining `[2,4]`. Hire cost 2.
- Total = 1 + 1 + 2 = 4.

## Observations

- Only two windows ever matter: the first `candidates` and the last `candidates` of the remaining workers. The middle is never touched until a slot opens up.
- Each window needs "give me the cheapest worker fast", so each one is a min-heap.
- Pushing `(cost, index)` tuples into the heap handles the tie-break for free. Python compares the cost first, then the index.
- Left-heap indices are always smaller than right-heap indices, so on a cost tie the left heap's top wins. Comparing the tuples `left[0] <= right[0]` gives the same answer.
- Two pointers `l` and `r` mark the next unused worker from each end. Everything in `[l, r]` is not in a heap yet. A worker enters a heap only if `l <= r`, and every push shrinks `[l, r]`, so no worker can be in both heaps.
- When the pointers cross, the heaps stop refilling and one of them can become empty. Handle that case when comparing the tops.

## Algorithm

1. Set `l = 0`, `r = n - 1`, and create two empty heaps `left` and `right`.
2. Repeat `k` times:
   1. While `left` has fewer than `candidates` items and `l <= r`, push `(costs[l], l)` and do `l += 1`.
   2. While `right` has fewer than `candidates` items and `l <= r`, push `(costs[r], r)` and do `r -= 1`.
   3. Take the top of each heap, using `(inf, inf)` for an empty heap.
   4. If `left_top <= right_top`, pop from `left`. Otherwise pop from `right`. Add the cost to the total.
3. Return the total.

Refilling at the start of each round is equivalent to refilling right after a pop. It also covers the initial fill.

## Complexity

- **Time:** O((candidates + k) log candidates). The initial fill pushes at most `2 * candidates` items, and each hire causes at most one pop and one push, each O(log candidates).
- **Space:** O(candidates) for the two heaps.

## Code

```python
import heapq
from math import inf


class Solution:
    def totalCost(self, costs: list[int], k: int, candidates: int) -> int:
        l, r = 0, len(costs) - 1
        left, right = [], []
        total = 0

        for _ in range(k):
            while len(left) < candidates and l <= r:
                heapq.heappush(left, (costs[l], l))
                l += 1
            while len(right) < candidates and l <= r:
                heapq.heappush(right, (costs[r], r))
                r -= 1

            lt = left[0] if left else (inf, inf)
            rt = right[0] if right else (inf, inf)

            if lt <= rt:
                total += heapq.heappop(left)[0]
            else:
                total += heapq.heappop(right)[0]

        return total
```

## Mistakes / Gotchas

- **Windows are the first `c` and the last `c` workers**, not a split at index `c`. Using `0..c` and `c+1..n-1` is wrong.
- **Use `l <= r`, not `l < r`.** Both pointers are inclusive "next unused" positions. When `l == r` one worker is still unused. With `l < r`, input like `costs = [5], k = 1` leaves both heaps empty and crashes.
- **Empty heap at comparison time.** After the pointers cross, a heap can run dry. For `costs = [1,2,3], k = 3, candidates = 1`, the left heap is empty in round 3 and `left[0]` raises `IndexError`. Use an `(inf, inf)` sentinel or check `if not right or (left and left[0] <= right[0])`.
- **Tie-break is automatic.** Do not write a custom comparator. Tuples `(cost, index)` already compare cost first, then index.
