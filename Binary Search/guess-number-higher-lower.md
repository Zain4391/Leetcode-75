# Problem Statement

[LeetCode - Guess Number Higher or Lower](https://leetcode.com/problems/guess-number-higher-or-lower/description/?envType=study-plan-v2&envId=leetcode-75)

I pick a number from `1` to `n`. You have to guess which number I picked. Each time you guess wrong, I tell you whether my number is higher or lower than your guess. You use a predefined API `int guess(int num)`, which returns:

- `-1`: your guess is **higher** than the picked number (`num > pick`)
- `1`: your guess is **lower** than the picked number (`num < pick`)
- `0`: your guess is correct (`num == pick`)

Return the number that I picked.

**Constraints:** `1 <= n <= 2^31 - 1`, `1 <= pick <= n`

## Examples

| n   | pick | Output |
| --- | ---- | ------ |
| 10  | 6    | 6      |
| 1   | 1    | 1      |
| 2   | 1    | 1      |

Trace for `n = 10, pick = 6`:

| l   | r   | mid | guess(mid)    | Action   |
| --- | --- | --- | ------------- | -------- |
| 1   | 10  | 5   | 1 (too low)   | `l = 6`  |
| 6   | 10  | 8   | -1 (too high) | `r = 7`  |
| 6   | 7   | 6   | 0             | return 6 |

## Observations

Before anything, leetcode cannot explain a problem to save their life!

Anyways:

- The search space `[1, n]` is sorted, and `guess()` tells us which side the answer is on, so this is a classic binary search.
- Each call to `guess(mid)` eliminates half of the remaining range.
- The API's sign is from the perspective of the guess, not the pick: `-1` means _my guess is too high_, so the answer lies to the left.
- A linear scan would be O(n), which is far too slow for `n` up to ~2.1 billion.

## Algorithm

1. Set `l = 1`, `r = n`.
2. While `l <= r`:
   1. Compute `mid = (l + r) // 2`.
   2. Call `guess(mid)`.
   3. If `0` → return `mid`.
   4. If `-1` (guess too high) → search the left half: `r = mid - 1`.
   5. If `1` (guess too low) → search the right half: `l = mid + 1`.
3. Return `-1` (unreachable, since the pick is guaranteed to be in range).

## Complexity

- **Time:** O(log n) — the range halves on every call to `guess`.
- **Space:** O(1) — only a few integer variables.

## Code

```python
class Solution:
    def guessNumber(self, n: int) -> int:
        l = 1
        r = n

        while l <= r:
            mid = (l + r) // 2
            result = guess(mid)

            if result == 0:
                return mid

            elif result == -1:
                r = mid - 1

            else:
                l = mid + 1

        return -1
```

## Mistakes / Gotchas

- **Inverted sign:** `-1` means the _guess_ is too high, so move `r` down (`r = mid - 1`). It's easy to read it as "pick is lower, so go up" and flip the branches.
- **Loop condition:** use `l <= r`, not `l < r`; otherwise the case where the range shrinks to a single number (e.g. `n = 1`) is never checked.
- **Always move past `mid`:** use `mid - 1` / `mid + 1`. Using `r = mid` or `l = mid` can cause an infinite loop.
- **Overflow in other languages:** `(l + r) // 2` is safe in Python, but in Java/C++ with `n` near `2^31 - 1` use `mid = l + (r - l) // 2`.
- **Final `return -1`:** unreachable given the constraints, but needed so the function always returns a value.
