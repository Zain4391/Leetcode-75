# Problem Statement

[Leetcode - Equal number of row and col pair](https://leetcode.com/problems/equal-row-and-column-pairs/description/?envType=study-plan-v2&envId=leetcode-75)

Given a 0-indexed n x n integer matrix grid, return the number of pairs (ri, cj) such that row ri and column cj are equal.

A row and column pair is considered **equal** if they contain the **same elements in the same order** (i.e., an equal array).

## Examples

### Example 1

**Input**: grid = [[3,2,1],[1,7,6],[2,7,7]]
**Output**: 1
**Explanation**: There is 1 equal row and column pair:

- (Row 2, Column 1): [2,7,7]

## Observations

We can solve this problem using hashmaps. There are two approaches:

- Use 2 hashmaps with key: row number/col number and value being the list of that specific row and column. Then comparing the two hashmaps for eqaul values.
- Use 2 hashmaps as frequency counters; key: tuple of values at specific row, value: amount of time that tuple has been seen. We then add the amount of time the value was seen into our final answer as we compare column lists to the stored tuples.

## Algorithm

### Algorithm 1

1. Init two hashmaps, R and C respectively.
2. Construct them using row major and col major ordering.
3. Compare the value at R[i] with all values in C[j].
4. Upon match increment count += 1.
5. Return count.

### Algorithm 2

1. Init a hashmap row
2. Add rowlist : count in the hashmap.
3. Set count = 0
4. For each column, get its count from the hashmap and add it to our ans.
5. Return count.

## Complexity

### Approach 1

- **Time**: *O(n^3)*
- **Space**: *O(N + M)* - where N and M are the size of the hashmaps.

### Approach 2

- **Time**: *O(n^2)*
- **Space**: *O(N + M)* - where N and M are the size of hashmaps

## Code

### Code 1

```python

class Solution:
    def equalPairs(self, grid: List[List[int]]) -> int:
        row, col = len(grid), len(grid[0])

        R = {}
        C = {}

        for i in range(row):
            R[i] = grid[i]
        
        for j in range(col):
            C[j] = []
            for i in range(row):
                C[j].append(grid[i][j])
        
        c = 0

        # The catch for O(n^3) is that R[i] == C[j] compares EACH element which will be done in a loop
        for i in range(row):
            for j in range(col):
                if R[i] == C[j]:
                    c += 1
        
        return c

```

### Code 2

```python

class Solution:
    def equalPairs(self, grid: List[List[int]]) -> int:
        rc = {}

        row, col = len(grid), len(grid[0])

        for i in grid:
            tup = tuple(i)
            rc[tup] = rc.get(tup, 0) + 1
        
        c = 0
        for j in range(col):
            col = []
            for i in range(row):
                col.append(grid[i][j])
            col = tuple(col)
            c += rc.get(col, 0)
        
        return c

```

## Mistakes / Gotchas

1. Not constructing hashmaps properly in either approaches.
2. Doing col[j].append() in approach 2 despite the col being an empty list at the start.
