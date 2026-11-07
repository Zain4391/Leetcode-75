# Problem Statement

[LeetCode: Nearest Exit from Entrance in Maze](https://leetcode.com/problems/nearest-exit-from-entrance-in-maze/)

You are given an `m x n` grid `maze` where each cell is either empty (`'.'`) or a wall (`'+'`), and an `entrance` given as `[row, col]` (always an empty cell). In one step you can move up, down, left or right into an empty cell. You cannot walk through walls or leave the grid.

An **exit** is an empty cell on the border of the grid. The entrance itself does **not** count as an exit, even if it is on the border.

Return the number of steps in the shortest path from the entrance to the nearest exit, or `-1` if no exit is reachable.

## Examples

**Example 1**

```
maze = [["+","+",".","+"],
        [".",".",".","+"],
        ["+","+","+","."]]
entrance = [1,2]
Output: 1
```

The cell above the entrance, `(0,2)`, is an empty border cell, so it is 1 step away.

**Example 2**

```
maze = [["+","+","+"],
        [".",".","."],
        ["+","+","+"]]
entrance = [1,0]
Output: 2
```

The entrance is on the border but doesn't count. Walk right twice to reach `(1,2)`.

**Example 3**

```
maze = [[".","+"]]
entrance = [0,0]
Output: -1
```

The only empty cell is the entrance itself, so there is no exit.

## Observations

- Every move costs 1 step, so this is a shortest path in an unweighted graph, which means **BFS**.
- The grid is the graph: each cell is a node, and its neighbors are the 4 adjacent cells. There is no adjacency list, so neighbors come from direction offsets: `(0,1), (1,0), (0,-1), (-1,0)`.
- BFS visits cells in order of distance, so the **first border cell** we reach is the nearest exit.
- Count steps **level by level**: each pass of the `while` loop processes every cell at distance `d`, then `steps` goes up by 1.
- The entrance is marked visited at the start, so it can never be counted as an exit.
- Check for an exit on the neighbor cell (the one we are moving to), not on the cell we popped.

## Algorithm

1. Put the entrance in the queue and in `visited`. Set `steps = 0`.
2. While the queue is not empty:
   1. `steps += 1`.
   2. Process exactly `len(q)` cells (one full level). For each popped cell `(r, c)`:
      1. For each direction `(dr, dc)`, compute the neighbor `(nr, nc)`.
      2. Skip it if it is out of bounds, a wall, or already visited.
      3. If it is on the border (`nr` is `0` or `rows-1`, or `nc` is `0` or `cols-1`), return `steps`.
      4. Otherwise mark it visited and push it onto the queue.
3. If the queue empties without finding an exit, return `-1`.

## Complexity

- **Time:** `O(m * n)`. Each cell is enqueued at most once and has 4 neighbors checked.
- **Space:** `O(m * n)` for the `visited` set and the queue in the worst case.

## Code

```python
from collections import deque

class Solution:
    def nearestExit(self, maze: list[list[str]], entrance: list[int]) -> int:
        rows = len(maze)
        cols = len(maze[0])

        def isValid(i, j, r, c):
            if (i > r - 1 or i < 0) or (j > c - 1 or j < 0) or maze[i][j] == "+":
                return False
            return True

        self.dirs = [(0, 1), (1, 0), (0, -1), (-1, 0)]

        # BFS on the maze
        q = deque([entrance])
        visited = set()
        visited.add((entrance[0], entrance[1]))
        steps = 0

        while q:
            steps += 1
            for _ in range(len(q)):
                r, c = q.popleft()
                for d in self.dirs:
                    nr = r + d[0]
                    nc = c + d[1]

                    if not isValid(nr, nc, rows, cols) or (nr, nc) in visited:
                        continue

                    if nr == 0 or nr == rows - 1 or nc == 0 or nc == cols - 1:
                        return steps

                    q.append([nr, nc])
                    visited.add((nr, nc))

        return -1
```

## Mistakes / Gotchas

**Approach**

- "Calling BFS on each neighbor" is recursion, which gives DFS-like behavior. BFS is one loop over a queue, with no recursive calls.
- A grid has no `for neighbour in maze[u]`. A cell is a `(row, col)` pair, so generate neighbors with direction offsets.

**Setup**

- `visited.add(entrance)` raised an error because `entrance` is a list, and lists are unhashable. Convert it to a tuple: `(entrance[0], entrance[1])`.
- `deque(entrance)` creates a queue of two separate ints (`r` and `c`), not one coordinate. Use `deque([entrance])`.
- Mixing lists (`entrance`, `[nr, nc]`) and tuples in the queue works for unpacking, but be consistent. Tuples are safer.
- Use the right variable names. `row` and `col` were used but only `rows` and `cols` were defined.

**Level-by-level counting**

- The inner loop must be `range(len(q))`, not `range(rows)`. The size of the current level has nothing to do with the number of rows.

**Neighbor handling**

- Don't mutate the current coordinate (`coord[0] += d[0]`). The changes pile up across directions, so compute a fresh `nr, nc` from `r, c` each time.
- `isValid` must check bounds _and_ walls, and must not reject border cells, because border cells are the exits.
- Validate the neighbor (`isValid(nr, nc, ...)`), not the cell you are standing on (`isValid(r, c, ...)`).
- Invert the skip condition correctly. Skip when the neighbor is **not valid** or **is already visited**:
  `if not isValid(nr, nc, rows, cols) or (nr, nc) in visited: continue`.
  I had written `isValid(...) or (nr, nc) not in visited`, which skips the good cells.
- Don't put `continue` after `visited.add(...)`. It skips the exit check and never pushes the cell onto the queue.

**Indentation**

- `q.append(...)` and `visited.add(...)` must sit **inside** the `for d in self.dirs` loop, after the exit check. Placed one level out, they run once per popped cell using whatever `nr, nc` was left over from the last direction, even if it was out of bounds or a wall.

**Order of checks**

- Do the exit check _before_ the append, so a border cell returns immediately instead of being queued.
- Check for an exit on the neighbor you are moving to. The entrance is already in `visited`, so it can never be returned as an exit.
