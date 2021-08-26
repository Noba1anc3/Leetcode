Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.



**Example:**
```
Input: strs = ["flower","flow","flight"]
Output: "fl"

Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

## Solution

```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string prefix = strs.empty() ? "" : strs[0];
        for (const string& str : strs)
            while (str.find(prefix) != 0) prefix = prefix.substr(0, prefix.size()-1);
        return prefix;
    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：9.2 MB, 在所有 C++ 提交中击败了16.17%的用户

Attention
- string.find()
- string.substr()
- string.size()
- vector.empty()
