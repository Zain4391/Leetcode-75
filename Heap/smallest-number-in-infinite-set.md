# Problem Statement

LeetCode 2336 — Smallest Number in Infinite Set.

You have a set containing all positive integers: `[1, 2, 3, 4, 5, ...]`.

Implement the `SmallestInfiniteSet` class:

- `SmallestInfiniteSet()` initializes the object to contain all positive integers.
- `int popSmallest()` removes and returns the smallest integer in the set.
- `void addBack(int num)` adds a positive integer `num` back into the set, if it is not already in the set.

**Constraints:**

- `1 <= num <= 1000`
- At most `1000` calls will be made in total to `popSmallest` and `addBack`.

## Examples

```
Input:
["SmallestInfiniteSet", "addBack", "popSmallest", "popSmallest", "popSmallest", "addBack", "popSmallest", "popSmallest", "popSmallest"]
[[], [2], [], [], [], [1], [], [], []]

Output:
[null, null, 1, 2, 3, null, 1, 4, 5]
```

Walkthrough:

1. `addBack(2)` does nothing, because 2 is already in the set.
2. `popSmallest()` returns 1. The set is now `{2, 3, 4, ...}`.
3. `popSmallest()` returns 2.
4. `popSmallest()` returns 3.
5. `addBack(1)` puts 1 back. The set is now `{1, 4, 5, ...}`.
6. `popSmallest()` returns 1.
7. `popSmallest()` returns 4.
8. `popSmallest()` returns 5.

## Observations

- The set is always "every integer `>= cur`, plus a few integers `< cur` that were added back". There is no need to store the infinite part.
- Pops always take the smallest element, so a **min-heap** fits the added-back numbers.
- `addBack(num)` only matters when `num < cur`, because anything `>= cur` is still in the set.
- A heap does not support O(1) membership checks, so a companion **set** guards against duplicates.
- Everything in the heap is smaller than `cur`, so `popSmallest` checks the heap first.

## Algorithm

State:

- `cur`: the smallest integer never popped from the natural sequence. It starts at 1.
- `heap`: a min-heap of added-back numbers (all `< cur`).
- `in_heap`: a set mirroring the heap, for O(1) duplicate checks.

**`popSmallest()`**

1. If `heap` is not empty, pop the smallest from it, remove it from `in_heap`, and return it.
2. Otherwise return `cur` and increment `cur`.

**`addBack(num)`**

1. If `num < cur` and `num` is not in `in_heap`, push it onto `heap` and add it to `in_heap`.
2. Otherwise do nothing.

## Complexity

Let `k` be the number of added-back elements currently waiting in the heap.

| Operation     | Time     |
| ------------- | -------- |
| `__init__`    | O(1)     |
| `popSmallest` | O(log k) |
| `addBack`     | O(log k) |

**Space:** O(k).

## Code

```python
import heapq

class SmallestInfiniteSet:

    def __init__(self):
        self.cur = 1          # smallest number never popped from the natural sequence
        self.heap = []        # numbers < cur that were added back
        self.in_heap = set()  # for O(1) duplicate checks

    def popSmallest(self) -> int:
        if self.heap:
            num = heapq.heappop(self.heap)
            self.in_heap.remove(num)
            return num
        num = self.cur
        self.cur += 1
        return num

    def addBack(self, num: int) -> None:
        if num < self.cur and num not in self.in_heap:
            heapq.heappush(self.heap, num)
            self.in_heap.add(num)
```

## Mistakes / Gotchas

- **Pre-filling the heap with 1..1000.** It passes only because of the constraints. It wastes O(n) setup and raises `IndexError` once the heap runs out, so it isn't really infinite.
- **Rebuilding `set(self.h)` inside `addBack`.** This makes every call O(n). Keep a persistent set in sync with the heap instead.
- **Forgetting the `num < cur` check.** A number `>= cur` is still in the set, so adding it to the heap creates a duplicate that would be popped twice.
- **Forgetting to remove from `in_heap` on pop.** The number could then never be added back again.
- **Heap-first order in `popSmallest`.** Heap elements are always smaller than `cur`, so check the heap before using `cur`.
- **Pattern to remember:** for "infinite" structures, store a pointer plus a small structure for the exceptions, instead of materializing the data.
