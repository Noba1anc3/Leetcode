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
c++

```python
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string prefix;
        strs.empty() ? prefix = "" : prefix = strs[0];
        for (string str : strs){
            while (str.find(prefix) != 0){
                prefix = prefix.substr(0, prefix.size()-1);
            }
        }
        return prefix;
    }
};
```
执行用时：8 ms, 在所有 C++ 提交中击败了47.42%的用户

内存消耗：9.7 MB, 在所有 C++ 提交中击败了10.98%的用户

Attention

- string.find()
- string.substr()
- string.size()
- vector.empty()