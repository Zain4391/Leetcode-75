# Problem Statement

[LeetCode - Dota2 senate](https://leetcode.com/problems/dota2-senate/description/?envType=study-plan-v2&envId=leetcode-75)

In the world of Dota2, there are two parties: the Radiant and the Dire.

The Dota2 senate consists of senators coming from two parties. Now the Senate wants to decide on a change in the Dota2 game. The voting for this change is a round-based procedure. In each round, each senator can exercise one of the two rights:

- **Ban one senator's right**: A senator can make another senator lose all his rights in this and all the following rounds.
- **Announce the victory**: If this senator found the senators who still have rights to vote are all from the same party, he can announce the victory and decide on the change in the game.
  Given a string senate representing each senator's party belonging. The character 'R' and 'D' represent the Radiant party and the Dire party. Then if there are n senators, the size of the given string will be n.

The round-based procedure starts from the first senator to the last senator in the given order. This procedure will last until the end of voting. All the senators who have lost their rights will be skipped during the procedure.

Suppose every senator is smart enough and will play the best strategy for his own party. Predict which party will finally announce the victory and change the Dota2 game. The output should be "Radiant" or "Dire".

## Examples

### Example 1

**Input**: senate = "RD"
**Output**: "Radiant"
**Explanation**
The first senator comes from Radiant and he can just ban the next senator's right in round 1.
And the second senator can't exercise any rights anymore since his right has been banned.
And in round 2, the first senator can just announce the victory since he is the only guy in the senate who can vote.

## Observations

What an essay of a description, with zero sense. We have to simulate a voting process where each senator can vote another one out given that the senator being voted out is from another party.
To go about this we need to consider the following:

- How to ban another senator from voting?
- The question says election continues for multiple rounds, thus we circle back to the start.

To answer the above concerns:

- We can ban the senator by checking if they're from another party. The logic for it is simple:
  - If R bans D, then it needs to be the nearest one. If another D is chosen, then in future the nearest D can sabotage R's chances of winning.
  - Sample principle is applied to D banning R.

- How do we continue? We simply circle back by applying the logic r_popped + len(senate).

We will use two queues to track postions of the Rs and Ds as their position is the _most_ important thing here. If Ri < Di then that R can ban D and vice versa. Plus that R can also continue playing the election thus we append it to the queue with a new position (Ri + len(senate)) to simulate the multiple rounds logic.

## Algorithm

1. Convert string to list for easy operation
2. Init two queues r and c.
3. Append Ri and Di to respective queue.
4. While either one of the queue does not become empty, pop from each and compare Ri and Di.
   5.If Ri < Di, then just append Ri + len(senate) to the r queue
   6.Else do vice versa for D.
   7.Whichever queue remains, return the ans wrt to it.

## Complexity

- **Time**: _O(N)_
- **Space**: _O(N)_

## Code

```python

from collections import deque
class Solution:
    def predictPartyVictory(self, senate: str) -> str:
        senate = list(senate)
        r = deque()
        d = deque()

        for i in range(len(senate)):
            if senate[i] == "R":
                r.append(i)
            else:
                d.append(i)

        while r and d:
            rc = r.popleft()
            dc = d.popleft()

            if rc < dc:
                rc = rc + len(senate)
                r.append(rc)
            else:
                dc = dc + len(senate)
                d.append(dc)

        ans = None
        if r:
            ans = "Radiant"
        else:
            ans = "Dire"

        return ans

```

## Mistakes / Gotchas

1. Not appending Ri and Di to the right queue.
2. Not re-appending Ri or Di without adding len(senate) offset.
3. Returning ans with incorrect syntax.
