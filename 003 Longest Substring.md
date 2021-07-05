Given a string, find the length of the longest substring without repeating characters.

**Example:**
```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3.

Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.

Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

## Solution
这道题主要用到思路是：滑动窗口

什么是滑动窗口？

其实就是一个队列,比如例题中的 ```abcabcbb```，进入这个队列 **（窗口）** 为 ```abc``` 满足题目要求，当再进入 ```a```，队列变成了 ```abca```，这时候不满足要求。所以，我们要移动这个队列！

如何移动？

我们只要把队列的左边的元素移出就行了，直到满足题目要求！

一直维持这样的队列，找出队列出现最长的长度时候，求出解！

时间复杂度：O(n)

### Python
```python
class Solution:
    def lengthOfLongestSubstring(self, string: str) -> int:
        lookup = set()
        left, max_len = 0, 0

        for char in string:
            while char in lookup:
                # 窗口左沿右移
                lookup.remove(string[left]) 
                left += 1
            # 窗口右沿右移
            lookup.add(char)
            max_len = max(max_len, len(lookup))

        return max_len
```

执行用时：68 ms, 在所有 Python3 提交中击败了80.94%的用户

内存消耗：15.1 MB, 在所有 Python3 提交中击败了29.92%的用户

Attention:
- set() : 无序不重复元素集（集合）
- set.add()  set.remove()
- while 不是 if : pwwb

### c++
```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int longest = 0, window_left = 0;
        unordered_set<char> S;
        for (const char& c : s){
            while (S.count(c))
                S.erase(s[window_left++]);
            S.insert(c);
            longest = max(longest, int(S.size()));
        }
        return longest;
    }
};
```

执行用时：20 ms, 在所有 C++ 提交中击败了68.47%的用户

内存消耗：10.6 MB, 在所有 C++ 提交中击败了25.01%的用户
