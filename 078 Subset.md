Given an integer array `nums`, return all possible subsets (the power set).

The solution set must not contain duplicate subsets.



**Example:**

```
Example 1:

Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

Example 2:

Input: nums = [0]
Output: [[],[0]]
```

## Solution


```c++
class Solution {
private:
    vector<int> res;
    vector<vector<int>> ans;

public:
    void backtrack(vector<int>& nums, int index){
        if (index == nums.size())
            return;
        for (int i = index; i < nums.size(); i++){
            int ele = nums[i];
            res.push_back(ele);
            ans.push_back(res);
            backtrack(nums, i+1);
            res.pop_back();
        }
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        backtrack(nums, 0);
        ans.push_back({});
        return ans;
    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：7.6 MB, 在所有 C++ 提交中击败了34.17%的用户

Attention:

- 这道题和其他回溯题的区别在于res压入ans的时机，应为每次将元素压入res之后或在调用回溯函数之前。