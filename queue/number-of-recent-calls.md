# Problem Statement

[LeetCode - Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/?envType=study-plan-v2&envId=leetcode-75)

You have a RecentCounter class which counts the number of recent requests within a certain time frame.

Implement the RecentCounter class:

- **RecentCounter**() Initializes the counter with zero recent requests.
- int **ping**(int t) Adds a new request at time t, where t represents some time in milliseconds, and returns the number of requests that has happened in the past **3000** milliseconds (including the new request). Specifically, return the number of requests that have happened in the inclusive range **[t - 3000, t]**.
  It is guaranteed that every call to ping uses a strictly larger value of t than the previous call.

## Examples

### Example 1

**Input**
["RecentCounter", "ping", "ping", "ping", "ping"]
[[], [1], [100], [3001], [3002]]

**Output**
[null, 1, 2, 3, 3]

**Explanation**:

- RecentCounter recentCounter = new RecentCounter();
- recentCounter.ping(1); // requests = [1], range is [-2999,1], return 1
- recentCounter.ping(100); // requests = [1, 100], range is [-2900,100], return 2
- recentCounter.ping(3001); // requests = [1, 100, 3001], range is [1,3001], return 3
- recentCounter.ping(3002); // requests = [1, 100, 3001, 3002], range is [2,3002], return 3

## Observations

This is again a poorly worded problem. We have to return the number of calls which fall in the range **_[t-3,000, t]_**. The ping function simply means:

- At time t we had a call
- We calculate the bounds of range and check for requests within that range.

Notice that our LEFT bound will cause values in our data structure to become invalid where we store t. Also the question gurantees that every call will have _strictly_ larger value of t, it means First Call In, will be the first call out. We have noticed this in a Queue, which follows **FIFO** pattern and will be the data structure used here.

## Algorithm

1. Initialize an empty queue.
2. For every ping call:
   - Append it to the queue
   - If our queue has previous calls which fall below the lower bound, remove them.
   - return the length of our queue.

## Complexity

- **Time**: _O(n)_
- **Space**: _O(n)_

## Code

```python

from collections import deque
class RecentCounter:

    def __init__(self):
        self.req = deque()

    def ping(self, t: int) -> int:
        self.req.append(t)

        while self.req and (self.req[0] < t - 3000):
            self.req.popleft()

        return len(self.req)



# Your RecentCounter object will be instantiated and called as such:
# obj = RecentCounter()
# param_1 = obj.ping(t)

```

## Mistakes / Gotchas

1. Using list instead of queue, the insert/append here is costly (O(n)).
2. Not calculating the Lower bound correctly.
