# Problem Statement

[Leetcode - Asteroid Collision](https://leetcode.com/problems/asteroid-collision/description/?envType=study-plan-v2&envId=leetcode-75)

We are given an array asteroids of integers representing asteroids in a row. The indices of the asteroid in the array represent their relative position in space.

For each asteroid, the **absolute** value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If **two** asteroids meet, the **smaller** one will explode. If both are the same size, **s** will explode. Two asteroids moving in the same direction will never meet.

## Examples

### Example 1

**Input**: asteroids = [5,10,-5]
**Output**: [5,10]
**Explanation**: The 10 and -5 collide resulting in 10. The 5 and 10 never collide.

### Example 2

**Input**: asteroids = [3,5,-6,2,-1,4]​​​​​​​
**Output**: [-6,2,4]
**Explanation**: The asteroid -6 makes the asteroid 3 and 5 explode, and then continues going left. On the other side, the asteroid 2 makes the asteroid -1 explode and then continues going right, without reaching asteroid 4.

## Observations

Well, again a poorly worded problem. They simply cannot tell the two conditions which are needed:

- Opposite direction means collision
- The collision is detected through the sign of the asteroid which is at the top of the stack.

If our stack is empty, or asteroid is moving away (+ve) or already a -ve element sits at the top = PUSH
Otherwise we pop from the stack.

## Algorithm

1. Init s = []
2. Loop over the aesteroids array.
3. If we have an element in our stack and top of stack is > 0 and aesteroid is moving inwards (-ve) and top of stack < abs(aesteroid) we pop.
4. Similarly if our stack is empty or aesteroid is moving away (+ve) or top of stack is -ve, we push.
5. If magnitudes are same, both will be destroyed, which means simply pop from the stack.

## Complexity

- **Time**: *O(n)*
- **Space**: *O(n)*

## Code

```python

class Solution:
    def asteroidCollision(self, asteroids: List[int]) -> List[int]:
        s = []

        for i in range(len(asteroids)):
            a = asteroids[i]

            while s and s[-1] > 0 and a < 0 and s[-1] < abs(a):
                s.pop()
            
            if not s or s[-1] < 0 or a > 0:
                s.append(a)
            elif s[-1] == abs(a):
                s.pop()
        
        return s

```

## Mistakes / Gotchas

1. Over complicated nested IFs.
2. Not checking a < 0 before popping.
3. Not checking for empty stack before pushing.
