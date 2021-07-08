Given a string `s` and a string `t`, check if `s` is subsequence of `t`.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

**Example:**
```
Input: s = "abc", t = "ahbgdc"
Output: true

Input: s = "axc", t = "ahbgdc"
Output: false
```

## Idea

如果找到了第一个匹配，则直接后移第一个字符串的指针，这就可以保留最长的第二个字符串给第一个字符串后面的字符去匹配。

无论是否匹配到，都要后移第二个字符串的指针，只要第二个字符串到尽头了就不能再匹配了。

## Solution

```c++
class Solution {
public:
    int isSubsequence(string s, string t) {
        int i = 0, j = 0;
        while (i < s.size() && j < t.size()){
            if (s[i] == t[j])
                i++;
            j++;    
        }
        return i == s.size();
    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：6.1 MB, 在所有 C++ 提交中击败了92.15%的用户
