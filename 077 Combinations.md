Given two integers `n` and `k`, return all possible combinations of k numbers out of 1 ... n.

You may return the answer in any order.



**Example:**

```
Example 1:

Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]

Example 2:

Input: n = 1, k = 1
Output: [[1]]
```

## Solution


```c++
class Solution {
private:
    vector<int> res;
    vector<vector<int>> ans;

public:
    void backtrack(int max, int cur, int width){
        if (width == 0) 
            ans.push_back(res);
        else
            for (int i = cur; i <= max - width + 1; i++){
                res.push_back(i);
                backtrack(max, i + 1, width - 1);
                res.pop_back();
            }
    }

    vector<vector<int>> combine(int n, int k) {
        backtrack(n, 1, k);
        return ans;
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了99.781%的用户

内存消耗：9.9 MB, 在所有 C++ 提交中击败了37.51%的用户
