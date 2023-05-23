# Leetcode-Python7
## 344. Reverse String, 541. Reverse String II

May 19, 2023  4h

The seventh day, we will start to learn about **string**.\
The challenges today are about ~~need to finish soon~~.

## 344. Reverse String
[leetcode](https://leetcode.com/problems/reverse-string/)\
[reading](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0344.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.md)\
[video](https://www.bilibili.com/video/BV1fV4y17748/?spm_id_from=pageDriver&vd_source=63f26efad0d35bcbb0de794512ac21f3)\
Algorithms of string is similar to arrays.\
When to use functions and when not to use them.\
Here we can use two pointers, one at the head, one at the end. And they move together to the center to get the result.
```python
# Ways 1: use two pointers:
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left,right = 0, len(s)-1
        while (left<right):
            s[left], s[right] = s[right], s[left]
            left += 1
            right -= 1
```
```python
# Ways 2: use stack
class Solution:
    def reverseString(self, s: List[str]) -> None:
        stack = []
        for char in s:
            stack.append(char)
        for i in range(len(s)):
            s[i] = stack.pop()
```
```python
# Ways 3: use built in function
class Solution:
    def reverseString(self, s: List[str]) -> None:
        s[:] = reversed(s)
```

## 541. Reverse String II
[leetcode](https://leetcode.com/problems/reverse-string-ii/description/)\
[reading](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0541.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2II.md)\
[video](https://www.bilibili.com/video/BV1dT411j7NN/?spm_id_from=333.788&vd_source=63f26efad0d35bcbb0de794512ac21f3)\
Same but different! I came across problems when I use two pointers to solve this question. The right solution directly used <ins>while left<len(s)</ins>. That's cool. 
```python
# ways 1: use two pointers, a left and a right one. The right one is inside the left one's loop:
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        left = 0
        while (left < len(s)):
            right = left + k
            # Written in this could be more pythonic.
            s = s[:left] + s[left: right][::-1] + s[right:]
            left = left + 2 * k
        return s
```
```python
# ways 2: use slice to directly change values for string items:
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        res = list(s)
        for cur in range(0, len(s), 2 * k):
            res[cur: cur + k] = reversed(res[cur: cur + k])
        return ''.join(res)
```





