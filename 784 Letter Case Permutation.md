Given a string `S`, we can transform every letter individually to be lowercase or uppercase to create another string.

Return a list of all possible strings we could create. You can return the output in **any order**.



**Example:**

```
Example 1:

Input: S = "a1b2"
Output: ["a1b2","a1B2","A1b2","A1B2"]

Example 2:

Input: S = "3z4"
Output: ["3z4","3Z4"]

Example 3:

Input: S = "12345"
Output: ["12345"]

Example 4:

Input: S = "0"
Output: ["0"]
```

## Solution


```c++
class Solution {
private:
    string res;
    vector<string> ans;

public:
    void backtrack(string& S, int i){
        if (i == S.size()){
            ans.push_back(res);
            return;
        }
        res += S[i];
        backtrack(S, i + 1);
        res = res.substr(0, res.size() - 1);
        if (S[i] >= 'A' && S[i] <= 'Z'){
            res += S[i] - 'A' + 'a';
            backtrack(S, i + 1);
            res = res.substr(0, res.size() - 1);
        }
        if (S[i] >= 'a' && S[i] <= 'z'){
            res += S[i] - 'a' + 'A';
            backtrack(S, i + 1);
            res = res.substr(0, res.size() - 1);
        }
    }
    vector<string> letterCasePermutation(string S) {
        backtrack(S, 0);
        return ans;
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了69.50%的用户

内存消耗：11.3 MB, 在所有 C++ 提交中击败了35.02%的用户