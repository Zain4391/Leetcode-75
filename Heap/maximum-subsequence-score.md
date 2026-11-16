# Problem Statement

**LeetCode 2542 – Maximum Subsequence Score**

You are given two 0-indexed integer arrays `nums1` and `nums2` of equal length `n`, and a positive integer `k`. Choose a subsequence of `k` indices from `nums1`.

For chosen indices `i0, i1, ..., i(k-1)`, the **score** is:

```
(nums1[i0] + nums1[i1] + ... + nums1[i(k-1)]) * min(nums2[i0], nums2[i1], ..., nums2[i(k-1)])
```

Return the **maximum** possible score.

**Constraints**

- `n == nums1.length == nums2.length`
- `1 <= n <= 10^5`
- `0 <= nums1[i], nums2[j] <= 10^5`
- `1 <= k <= n`

## Examples

**Example 1**

```
nums1 = [1,3,3,2], nums2 = [2,1,3,4], k = 3
Output: 12
```

Choose indices 0, 2, 3: sum = 1 + 3 + 2 = 6, min(nums2) = min(2, 3, 4) = 2, score = 6 \* 2 = 12.

**Example 2**

```
nums1 = [4,2,3,1,1], nums2 = [7,5,10,9,6], k = 1
Output: 30
```

Choose index 2: sum = 3, min = 10, score = 30.

## Observations

- The score depends on two things at once: a **sum** of `nums1` (want large) and a **min** of `nums2` (want large). One heap can't track both.
- **Fix one dimension by ordering.** Sort pairs `(nums2[i], nums1[i])` by `nums2` **descending**.
- While walking in that order, every element already seen has `nums2 >=` the current one. So if we pick the current element together with `k-1` earlier elements, the current `nums2` is guaranteed to be the **min** of the chosen set. No need to track or recompute a min.
- Once the min is fixed to the current element, the only job left is to maximise the sum: pick the `k-1` earlier elements with the **largest `nums1`** (plus the current one).
- A **min-heap of `nums1` values** (size `k`) with a running `total` keeps exactly the `k` largest `nums1` seen so far.
- **Why dropping an element is safe:** when the heap exceeds `k`, we pop the smallest `nums1`. Any future set has a smaller (or equal) min, and must include its own new element, so it only has room for `k-1` older elements, and the best `k-1` are never the popped one (it's the k-th best at best). Nothing useful is lost. The set we scored with was evaluated _before_ it could be affected.
- Sorting the `nums2` list alone breaks the problem: the pairing with `nums1` must move together (sort pairs or sort indices).

## Algorithm

1. Build pairs `(nums2[i], nums1[i])` and sort by `nums2` descending.
2. Keep a min-heap `heap` of `nums1` values, a running `total`, and `best = 0`.
3. For each `(n2, n1)` in sorted order:
   1. Push `n1` into `heap`, add it to `total`.
   2. If `len(heap) > k`, pop the smallest and subtract it from `total`.
   3. If `len(heap) == k`, then `n2` is the min of the chosen `k`, so `best = max(best, total * n2)`.
4. Return `best`.

**Intuition in one line:** if we have `k-1` earlier elements (all with `nums2 >=` the current one) and pick the current one, then the current `nums2` is the min, and the `nums1` values at those indices give the sum.

## Complexity

- **Time:** `O(n log n)` for the sort + `O(n log k)` for heap operations → `O(n log n)`.
- **Space:** `O(n)` for the sorted pairs + `O(k)` for the heap.

## Code

```python
import heapq

def maxScore(nums1, nums2, k):
    pairs = sorted(zip(nums2, nums1), reverse=True)   # by nums2 descending
    heap = []        # min-heap of nums1 values (the k largest so far)
    total = 0        # sum of values currently in heap
    best = 0

    for n2, n1 in pairs:
        heapq.heappush(heap, n1)
        total += n1
        if len(heap) > k:
            total -= heapq.heappop(heap)      # drop smallest nums1
        if len(heap) == k:
            best = max(best, total * n2)      # n2 is the min of the chosen k
    return best
```

## Mistakes / Gotchas

- **Sorting only `nums2`** loses the pairing with `nums1`. Sort pairs (or indices) so they move together.
- **Pushing `n2` into the heap** is wrong. The sort already handles the min of `nums2`; the heap exists to maintain the sum of `nums1`.
- **Max-heap vs min-heap:** we need a **min-heap** so the smallest `nums1` is at the top and can be popped. Python's `heapq` is a min-heap by default (no negation needed here).
- **Sorting ascending** instead of descending breaks the "current `n2` is the min" guarantee.
- **Scoring before size reaches `k`:** only compute `best` when `len(heap) == k`.
- **Forgetting to keep `total` in sync** with the heap on every push and pop.
- If sorting indices instead of pairs, key the heap by `nums1[i]` (not by the index itself) and subtract `nums1[popped]` from `total`.
