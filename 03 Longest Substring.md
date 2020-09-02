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

其实就是一个队列,比如例题中的 ```abcabcbb```，进入这个队列**（窗口）**为 ```abc``` 满足题目要求，当再进入 ```a```，队列变成了 ```abca```，这时候不满足要求。所以，我们要移动这个队列！

如何移动？

我们只要把队列的左边的元素移出就行了，直到满足题目要求！

一直维持这样的队列，找出队列出现最长的长度时候，求出解！

时间复杂度：O(n)

Python
```python
class Solution:
    def lengthOfLongestSubstring(self, string: str) -> int:
        if not string:
            return 0

        lookup = set()
        left = 0
        max_len = 0
        cur_len = 0

        for char in string:
            cur_len += 1
            while char in lookup:
                lookup.remove(string[left])
                left += 1
                cur_len -= 1
            if cur_len > max_len:
                max_len = cur_len
            lookup.add(char)

        return max_len
```
Time : 68ms
Memory : 13.8MB

Attention:
- if not string
- set() : 无序不重复元素集　（集合）
- set.add()  set.remove()
- while 不是 if : pwwb

c++
```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        if(s.size() == 0) return 0;

        unordered_set<char> lookup;
        int maxStr = 0;
        int left = 0;
        for(int i = 0; i < s.size(); i++){
            while (lookup.find(s[i]) != lookup.end()){
                lookup.erase(s[left]);
                left++;
            }
            maxStr = max(maxStr,i-left+1);
            lookup.insert(s[i]);
    	}
        return maxStr;
    }
};

```
Time : 72ms
Memory : 11MB
