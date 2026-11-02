# Problem Statement

[Leetcode - Keys and Rooms](https://leetcode.com/problems/keys-and-rooms/description/?envType=study-plan-v2&envId=leetcode-75)

There are **n rooms** labeled from **0 to n - 1** and all the rooms are locked except for room 0. Your goal is to visit all the rooms. However, you cannot enter a **locked** room without having its key.

When you visit a room, you may find a set of distinct keys in it. Each key has a number on it, denoting which room it unlocks, and you can take all of them with you to unlock the other rooms.

Given an array rooms where **rooms[i]** is the set of keys that you can obtain if you visited room i, return true if you can visit all the rooms, or false otherwise.

## Examples

### Example 1

**Input**: rooms = [[1],[2],[3],[]]
**Output**: true
**Explanation**:

- We visit room 0 and pick up key 1.
- We then visit room 1 and pick up key 2.
- We then visit room 2 and pick up key 3.
- We then visit room 3.
  Since we were able to visit every room, we return true.

### Example 2

**Input**: rooms = [[1,3],[3,0,1],[2],[0]]
**Output**: false
**Explanation**: We can not enter room number 2 since the only key that unlocks it is in that room.

## Observations

This is a BFS problem where room number (index of the rooms array) is the node and the subarray at that index are the directed edges. We have run a BFS here and then simply check if the length of our visited set is equal to the rooms array as that determines that we have visited all of the rooms.

## Algorithm

1. Init a visited set
2. Set a deque; q = deque()
3. add the first/starting room in the visited set.
4. Then push all of it's neighbours into the queue.
5. Also add the neighbors into the visited set as a key appearing here gurantees we can visit that room.
6. Run a while loop that runs untilq is present.
7. Pop the left most node and check if it's neighbours are in the vsisted set, if not add them into the set and the queue (to be further explored).
8. Return lenght(visited) == length(rooms)

## Complexity

- **Time**: _O(N+K) where N is the number of rooms and K are the number of Keys_
- **Space**: _O(N)_

## Code

```python

from collections import deque
class Solution:
    def canVisitAllRooms(self, rooms: list[list[int]]) -> bool:
        visited = set()
        q = deque()

        visited.add(0)
        for key in rooms[0]:
            q.append(key)
            visited.add(key)

        while q:
            node = q.popleft()
            for key in rooms[node]:
                if key not in visited:
                    visited.add(key)
                    q.append(key)

        return len(visited) == len(rooms)

```

## Mistakes / Gotchas

1. Not importing collections
2. Not adding the key in visited ste or the queue.
