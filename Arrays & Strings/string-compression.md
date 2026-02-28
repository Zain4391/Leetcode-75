# Problem statement

[Leetcode - String compression](https://leetcode.com/problems/string-compression/description/?envType=study-plan-v2&envId=leetcode-75)

Given an array of characters chars, **compress** it using the following algorithm:

Begin with an empty string **s**. For each group of consecutive repeating characters in chars:

- If the group's length is *1*, append the character to s.
- Otherwise, append the **character** followed by the **group's length**.

The compressed string **s** should not be returned separately, but instead, be stored in the input character array chars. Note that group lengths that are 10 or longer will be split into multiple characters in chars.

After you are done modifying the input array, return the new **length** of the array.

You must write an algorithm that uses only **constant** extra space.

Note: The characters in the array beyond the returned length do not matter and should be ignored.

## Examples

### Example 1

**Input**: chars = ["a","a","b","b","c","c","c"]
**Output**: Return 6, and the first 6 characters of the input array should be: ["a","2","b","2","c","3"]
**Explanation**: The groups are "aa", "bb", and "ccc". This compresses to "a2b2c3".

### Example 2

**Input**: chars = ["a"]
**Output**: Return 1, and the first character of the input array should be: ["a"]
**Explanation**: The only group is "a", which remains uncompressed since it's a single character.

### Example 3

**Input**: chars = ["a","b","b","b","b","b","b","b","b","b","b","b","b"]
**Output**: Return 4, and the first 4 characters of the input array should be: ["a","b","1","2"].
**Explanation**: The groups are "a" and "bbbbbbbbbbbb". This compresses to "ab12".

## Observations

So this problem is worded poorly. We simple need to do the following:

- Count occurnece of a character in the array
- If that count is > 1, place it with that character (like "A", "1" => Means A has one occurence)
- If count is in double digits, we break the digits and place them ahead of the character. (like "A": "1", "2" => Means A has 12 occurences)
- Return the length of the array which can be determined by number of times we **write** in the array, inplace.

We do all this due to the constraint: *Constant Space!*

## Algorithm

1. Start with i = 0 and write  = 0
2. Loop until i < n becomes False.
3. Within the Loop we count for characters' occurences. We do this by:

    - i + c < n [boundary check]
    - chars[i + c] == character to check for [See if characters beyond ith ones are the ones we are interested in]

4. After counting we write back the character in the array.
5. Now we write back the count. To do this, we typecast the count as a string, loop ober the digits and simple write them in the array.
6. We return the write pointer's value, as it indicates the length of the newly modified array.

## Complexity

- **Time**: *O(n)*
- **Space**: *O(1)*

## Code

```python

class Solution:
    def compress(self, chars: List[str]) -> int:
        i = 0
        write = 0

        n = len(chars)

        while i < n:
            ch = chars[i]
            c = 1

            while  i + c < n and chars[i + c] == ch:
                c += 1
            
            chars[write] = ch
            write += 1

            if c > 1:
                for digit in str(c):
                    chars[write] = digit
                    write += 1
            i += c
        return write
        
```

## Mistakes / Gotcha

1. not using i + c for indexing the chars array when comparing characters.
2. Not incrementing write pointer after *every* write operation.
3. Not returning anything. Despite the question not helping here, we have to return length of modified array [as stated in examples].
