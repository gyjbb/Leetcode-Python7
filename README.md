# Leetcode-Python7
## 344. Reverse String, 541. Reverse String II, Replace the tab, 151. Reverse Words in a String, Reverse certain characters in a string

May 19, 2023  4h

The seventh day, we will start to learn about **string**.\
The challenges today are about reverse strings. Since strings are immutable, so when we use python to reverse the string, we firstly change the string to a list. Then use slice or pointers to solve the prroblems. Or we can build a new list and use extra space to store the rersult, and use slice. Finally, join the characters in list to return the required string.

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
Same but different! When I used two pointers to solve this question, time limits were exceeded. The right solution directly used <ins>while left<len(s)</ins>. That's simple. 
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

## Replace the tab
[reading](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/%E5%89%91%E6%8C%87Offer05.%E6%9B%BF%E6%8D%A2%E7%A9%BA%E6%A0%BC.md)\
We can't change items in a string, so firstly we need to change the string to list. \
Then we expand the original list's length to new list's length.\
Use **two pointers**, one from the **end** of old string, and one from the **end** of new string, to build the loop.
```python
# ways 1: use two pointers. 转换成列表，并且添加相匹配的空间，然后进行填充
class Solution:
    def replaceSpace(self, s: str) -> str:
        counter = s.count(' ')
        res = list(s)
        # 每碰到一个空格就多拓展两个格子，1 + 2 = 3个位置存’%20‘
        res.extend([' '] * counter * 2)
        
        # 原始字符串的末尾，拓展后的末尾
        left, right = len(s) - 1, len(res) - 1
        
        while left >= 0:
            if res[left] != ' ':
                res[right] = res[left]
                right -= 1
            else:
                # [right - 2, right), 左闭右开
                res[right - 2: right + 1] = '%20'
                right -= 3
            left -= 1
        return ''.join(res)    
```
```python
# ways 2: build a new list to store the result:
class Solution:
    def replaceSpace(self, s: str) -> str:
        res = []
        for i in range(len(s)):
            if s[i] == ' ':
                res.append('%20')
            else:
                res.append(s[i])
        return ''.join(res)    
```
    
## 151. Reverse Words in a String
[leetcode](https://leetcode.com/problems/reverse-words-in-a-string/)\
[reading](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.md)\
[video](https://www.bilibili.com/video/BV1uT41177fX/?spm_id_from=333.788&vd_source=63f26efad0d35bcbb0de794512ac21f3)\
We can use built in function, or use two pointers, a slow one and a faster one to solve this question.\
When not using the built in function, we can firstly reverse the whole string, then reverse again within each word. The problem is how to delete multiple tabs, and only one tab can exist between two words. Here we use two pointers. Use the fast one to get the non-tab ones, and give to slow pointer.\
But one tab should still exists between words. So we use slow pointer to point to a tab place and move forward a step. Then pass the fast pointer's value to slow pointer.     
After removeing extra tabs, we can finally reverse the whole string, then reverse again within each word.  
```python
# ways 1: use built in function: 先删除空白，然后整个反转，最后单词反转。 因为字符串是不可变类型，所以反转单词的时候，需要将其转换成列表，然后通过join函数再将其转换成列表，所以空间复杂度不是O(1)
class Solution:
    def reverseWords(self, s: str) -> str:
        # 删除前后空白
        s = s.strip()
        # 反转整个字符串
        s = s[::-1]
        # 将字符串拆分为单词，并反转每个单词
        s = ' '.join(word[::-1] for word in s.split())
        return s    
```
```python
# ways 2: use two pointers:
class Solution:
    def reverseWords(self, s: str) -> str:
        # 将字符串拆分为单词，即转换成列表类型
        words = s.split()

        # 反转单词
        left, right = 0, len(words) - 1
        while left < right:
            words[left], words[right] = words[right], words[left]
            left += 1
            right -= 1

        # 将列表转换成字符串
        return " ".join(words)
```

## Reverse certain characters in a string
[reading](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/%E5%89%91%E6%8C%87Offer58-II.%E5%B7%A6%E6%97%8B%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.md)
Here, we use a special way to solve the problem: firstly reverse the first n characters, then reverse the (n+1) to the last characters. Finnaly, reverse the whole string and we will get the desired sring.
```python
# ways 1: use slice:
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        return s[n:] + s[:n]                           
```
```python
# ways 2: use reversed + join
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        s = list(s)
        s[0:n] = list(reversed(s[0:n]))
        s[n:] = list(reversed(s[n:]))
        s.reverse()
        
        return "".join(s)    
```
                           
                           
    
    
    
    
    
