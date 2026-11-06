# Problem Statement

[LeetCode 399 - Evaluate Division](https://leetcode.com/problems/evaluate-division/description/?envType=study-plan-v2&envId=leetcode-75)

You are given:

- `equations`: a list of variable pairs `[Ai, Bi]`
- `values`: a list of reals where `values[i]` is the result of `Ai / Bi`
- `queries`: a list of pairs `[Cj, Dj]`, each asking for `Cj / Dj`

Return the answer to every query as a list of floats. If a query's answer cannot be determined, return `-1.0`.

Notes:

- All given values are positive, and the input has no contradictions.
- A variable that never appears in `equations` is undefined, so any query involving it is `-1.0` (even `x / x`).

## Examples

**Example 1**

```
equations = [["a","b"],["b","c"]]
values    = [2.0, 3.0]
queries   = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
Output    = [6.0, 0.5, -1.0, 1.0, -1.0]
```

| Query | Reasoning                   | Answer |
| ----- | --------------------------- | ------ |
| a / c | a→b (2) then b→c (3): 2 × 3 | 6.0    |
| b / a | reverse of a→b: 1 / 2       | 0.5    |
| a / e | `e` does not exist          | -1.0   |
| a / a | `a` exists, so self-ratio   | 1.0    |
| x / x | `x` does not exist          | -1.0   |

**Example 2 (disconnected components)**

```
equations = [["a","b"],["c","d"]]
values    = [2.0, 3.0]
queries   = [["a","d"]]
Output    = [-1.0]
```

Both `a` and `d` exist, but no path connects them, so the ratio is unknown.

## Observations

- Each variable is a **node**. Each equation `A / B = k` is an **edge** `A → B` with weight `k`.
- Walking along a path **multiplies** the edge weights: `a/b * b/c = a/c`.
- Walking an edge backwards uses the **reciprocal**: if `a/b = k` then `b/a = 1/k`. So store both `A → B (k)` and `B → A (1/k)` when building the graph.
- A query `C / D` becomes: find any path from `C` to `D` and multiply the weights along it. Because the input is consistent, any path gives the same answer.
- Two cases give `-1.0`: either variable is missing from the graph, or both exist but are in different connected components.
- `X / X = 1.0` **only if** `X` exists.
- The order of checks matters: existence first, then the self-case, then the search.

## Algorithm

1. **Build the graph** as a dict mapping each variable to a list of `(neighbor, weight)` pairs. For each equation `(a, b)` with value `k`, add `(b, k)` to `a`'s list and `(a, 1/k)` to `b`'s list.
2. **For each query `(src, dst)`:**
   - If `src` or `dst` is not in the graph, answer `-1.0`.
   - Else if `src == dst`, answer `1.0`.
   - Else run a DFS from `src` to `dst`, carrying a running product and a fresh `visited` set. Return the product when `dst` is reached, or `-1.0` if the DFS exhausts all paths.
3. Collect the answers in query order and return them.

## Complexity

Let `V` be the number of unique variables, `E` the number of equations, and `Q` the number of queries.

- **Time:** `O(E + Q * (V + E))`. Building the graph is `O(E)`. Each query's DFS visits each node and edge at most once.
- **Space:** `O(V + E)` for the graph, plus `O(V)` for the `visited` set and the recursion stack.

Alternative: weighted Union-Find stores each node's ratio to its root, which gives near `O(1)` per query after setup.

## Code

```python
from collections import defaultdict
from typing import List

class Solution:
    def calcEquation(self, equations: List[List[str]], values: List[float],
                     queries: List[List[str]]) -> List[float]:
        # 1. Build the graph: a/b = k  =>  a->b (k), b->a (1/k)
        graph = defaultdict(list)
        for (a, b), k in zip(equations, values):
            graph[a].append((b, k))
            graph[b].append((a, 1 / k))

        # 2. DFS: return the product of weights from cur to target, or -1.0
        def dfs(cur, target, product, visited):
            if cur == target:
                return product
            visited.add(cur)
            for neighbor, weight in graph[cur]:
                if neighbor not in visited:
                    result = dfs(neighbor, target, product * weight, visited)
                    if result != -1.0:
                        return result
            return -1.0

        # 3. Answer each query
        answers = []
        for src, dst in queries:
            if src not in graph or dst not in graph:
                answers.append(-1.0)
            elif src == dst:
                answers.append(1.0)
            else:
                answers.append(dfs(src, dst, 1.0, set()))
        return answers
```

## Mistakes / Gotchas

- **`x / x` is not always 1.** It is `1.0` only if `x` exists in the graph; otherwise `-1.0`.
- **Forgetting the reverse edge.** Without `B → A (1/k)`, queries like `b / a` fail.
- **Missing-node check must come first.** Test `src in graph` before any `graph[src]` access.
- **`defaultdict` phantom entries.** Reading `graph["x"]` on a `defaultdict` creates an empty entry, which makes a later `"x" in graph` true. Check membership before indexing, or use a plain dict with `setdefault`.
- **No `visited` set** causes infinite recursion on cycles (e.g. `a/b`, `b/c`, `c/a`).
- **Reusing `visited` across queries.** Create a fresh `set()` for every query.
- **Disconnected components** return `-1.0` even though both nodes exist.
- **Sizing an adjacency list by `len(equations)`** is wrong: that counts edges, not unique nodes, and the nodes are strings anyway. Use a dict.
- **Return floats**, not ints: `1.0` and `-1.0`.
