# Problem Statement

[Leetcode - Reverse words in string](https://leetcode.com/problems/reverse-words-in-a-string/description/?envType=study-plan-v2&envId=leetcode-75)

Given an input string s, reverse the order of the words.

A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.

Return a string of the words in reverse order concatenated by a single space.

Note that s may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

## Examples

### Example 1

**Input**: s = "the sky is blue"
**Output**: "blue is sky the"

### Example 2

**Input**: s = "  hello world  "
**Output**: "world hello"
**Explanation**: Your reversed string should not contain leading or trailing spaces.

## Observations

In easy/simple words, the first word of our output needs to tbe the last word of the input, thus we should start at the end of the input and start constructing our final string from there. Secondly, to deal with trailing spaces we can either use **split()** function or manually skip past the trailing whitespaces.

## Algorithm

1. Initialize i = len(s) - 1 and j = i - 1. The i pointer tracks end of a word where as j detects start, thus making it easier to extract it from the stirng.
2. Loop until i >= 0 and j >= 0
3. While we encounter whitespaces from the end of the string, we skip past them
4. If i < 0, then break **(this is an edge case!)**
5. Set j = i - 1 and start tracking the starting of a word.
6. Once j encounters " ", loop stops
7. Extract word s[j + 1 : i + 1]
8. Add to answer
9. Return ' '.join(ans)

## Complexity

- **Time**: *O(n)*
- **Space**: *O(n)* - as we store words in an array called ans

## Code

```python

class Solution(object):
    def reverseWords(self, s):
        """
        :type s: str
        :rtype: str
        """
        if len(s) == 1:
            return s
        ans = []
        i = len(s) - 1
        j = i - 1

        while i >= 0 and j >= 0:
            while i > -1 and s[i] == " ":
                i -= 1
            if i < 0:
                break
            j = i - 1
            while j > -1 and s[j] != " ":
                j -= 1

            ans.append(s[j + 1: i + 1])
            i = j - 1
        return ' '.join(ans)

```

## Mistakes / Gotcha

- Not adjusting j before starting the second inner while loop
- Not updating i **after** extracting the word.
- Using ''.join(ans) instead of ' '.join(ans)
