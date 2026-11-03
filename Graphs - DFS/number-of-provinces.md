# Problem Statement

[Leetcode - Number of provinces](https://leetcode.com/problems/number-of-provinces/description/?envType=study-plan-v2&envId=leetcode-75)

There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c.

A _province_ is a group of _directly_ or _indirectly_ connected cities and no other cities outside of the group.

You are given an n x n matrix **isConnected where isConnected[i][j] = 1** if the ith city and the jth city are directly connected, and **isConnected[i][j] = 0** otherwise.

Return the total number of **provinces**.

## Examples

**Input**: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
**Output**: 2

## Observations

The phrase "If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c" helps us identify that this is a DFS problem where we have to run a DFS from each node to check if the can form a province.
Other than that it is a standard DFS problem.

## Algorithm

1. Init a set v = set()
2. Start the DFS from first city, add it to the set.
3. For that city, check if it's neighbours are in the set, if not then it's a candidate to find another province.
4. After this, loop over the whole Adj Matrix, and for cities not visited in previous runs, run a DFS on them and increment the number of provinces.

## Complexity

- **Time**: _O(N)_
- **Space**: _O(1)_

## Code

```python

class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        self.v = set()

        def dfs(city):
            self.v.add(city)
            for n in range(len(isConnected)):
                if isConnected[city][n] == 1 and n not in self.v:
                    dfs(n)

        p = 0
        for city in range(len(isConnected)):
            if city not in self.v:
                dfs(city)
                p += 1

        return p

```

## Mistakes / Gotchas

1. Treating the Adj Matrix as a Adj List and looping over it like isConnected[city]
2. Not respecting the already visited nodes and running DFS on them again, leading to duplicate counting
