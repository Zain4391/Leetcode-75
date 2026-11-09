# Problem Statement

[LeetCode - Rooting Oranges](https://leetcode.com/problems/rotting-oranges/?envType=study-plan-v2&envId=leetcode-75)

You are given an `m x n` grid where each cell is:

- `0`: empty
- `1`: a fresh orange
- `2`: a rotten orange

Every minute, any fresh orange that is 4-directionally adjacent (up, down, left, right) to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return `-1`.

Constraints: `1 <= m, n <= 10`, `grid[i][j]` is `0`, `1`, or `2`.

## Examples

**Example 1**

```
Input:  [[2,1,1],
         [1,1,0],
         [0,1,1]]
Output: 4
```

**Example 2**

```
Input:  [[2,1,1],
         [0,1,1],
         [1,0,1]]
Output: -1
```

The bottom-left orange is never adjacent to a rotten one, so it never rots.

**Example 3**

```
Input:  [[0,2]]
Output: 0
```

There are no fresh oranges at minute 0, so the answer is 0.

## Observations

- Rot spreads outward from all rotten oranges at the same time, so this is a **multi-source BFS**.
- Each BFS level is one minute, so process the queue level by level.
- There is no target cell. We never "return when you reach the boundary". The BFS runs until the queue is empty or nothing is fresh.
- The answer depends on what is left over: track a `fresh` counter. If `fresh > 0` after the BFS, some oranges are unreachable (walled off by empty cells), so return `-1`.
- Marking a cell as `2` when it rots doubles as the visited marker, so no separate visited set is needed.
- Only cells equal to `1` need processing. Empty (`0`), rotten (`2`) and out-of-bounds neighbors are all skipped by the same condition.
- Edge case: no fresh oranges at the start returns `0`.

## Algorithm

1. Scan the grid. Push every rotten orange `(r, c)` into a queue and count the fresh oranges.
2. While the queue is non-empty **and** `fresh > 0`:
   1. `time += 1`
   2. For exactly `len(q)` items (the current level):
      1. Pop `(r, c)`.
      2. For each of the 4 directions, compute `(nr, nc) = (r + dr, c + dc)`.
      3. If `(nr, nc)` is in bounds **and** `grid[nr][nc] == 1`: mark it `2`, do `fresh -= 1`, and push it.
3. Return `time` if `fresh == 0`, otherwise `-1`.

The `fresh > 0` guard in the `while` condition stops the loop right after the last orange rots, so no extra empty level is counted and there is no need to return `time - 1`.

## Complexity

- **Time:** _O(m \* n)_. Each cell is enqueued at most once and checked a constant number of times.
- **Space:** _O(m \* n)_ for the queue in the worst case.

## Code

```python
from collections import deque
class Solution:
    def orangesRotting(self, grid: list[list[int]]) -> int:
        rows = len(grid)
        cols = len(grid[0])

        def isValid(cr, cc, r, c):
            if (cr < 0 or cr > r-1) or (cc < 0 or cc > c - 1):
                return False
            return True

        # count fresh oranges
        fresh = 0
        q = deque()
        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == 2:
                    q.append([r, c])
                elif grid[r][c] == 1:
                    fresh += 1

        time = 0
        dirs = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        while q and fresh > 0:
            time += 1
            for _ in range(len(q)):
                r, c = q.popleft()
                for dr, dc in dirs:
                    nr = r + dr
                    nc = c + dc

                    # check for validity of the move
                    if not isValid(nr, nc, rows, cols) or grid[nr][nc] == 2:
                        continue
                    if isValid(nr, nc, rows, cols) and grid[nr][nc] == 1:
                        fresh -= 1
                        grid[nr][nc] = 2
                        q.append([nr, nc])

        if fresh == 0:
            return time

        return -1
```

## Mistakes / Gotchas

- **Comparing against strings.** `grid[r][c] == '2'` is never true because the grid holds ints. Use `2` and `1`.
- **Wrong value when counting fresh oranges.** `elif grid[r][c] == 2:` should be `== 1`. That branch could never run (the `if` above already catches every 2), so `fresh` stayed 0 and the function always returned `0`.
- **`nr` and `nc` never initialized.** `nr += dr` raises `UnboundLocalError`. Compute a new neighbor each time with `nr, nc = r + dr, c + dc`.
- **Typo:** `frehs -= 1` instead of `fresh -= 1`.
- **Off-by-one in `isValid`.** `cr == r - 1` rejected the last valid row (and the same for columns). The correct check is `0 <= nr < rows`. `cr > r - 1` is equivalent to `cr >= r`, which is also fine.
- **Indexing the grid before checking bounds.** `grid[nr][nc] == 2 or not isValid(...)` reads the grid first. Out-of-range positive indexes raise `IndexError`, and negative ones silently wrap to the opposite side. Always check bounds first, then index.
- **A redundant first check.** `if not isValid(...) and grid[nr][nc] == 2: continue` is unnecessary and buggy (the `and` still evaluates the index when out of bounds). The single condition `isValid(nr, nc) and grid[nr][nc] == 1` already skips out-of-bounds, empty and rotten cells.
