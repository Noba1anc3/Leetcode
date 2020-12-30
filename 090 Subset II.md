Given a collection of integers that might contain duplicates, `nums`, return all possible subsets (the power set).

**Note**: The solution set must not contain duplicate subsets.



**Example:**

```
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

## Solution - I 回溯


```c++
class Solution {
private:
    vector<int> res;
    vector<vector<int>> ans;

public:
    void backtrack(vector<int> nums, int index){
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

    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        backtrack(nums, 0);
        
        ans.push_back({});
        sort(ans.begin(), ans.end());
        ans.erase(unique(ans.begin(), ans.end()), ans.end());
        
        return ans;        
    }
};
```

执行用时：24 ms, 在所有 C++ 提交中击败了5.15%的用户

内存消耗：8.3 MB, 在所有 C++ 提交中击败了17.20%的用户

## Solution - II 回溯剪枝

```c++
class Solution {
private:
    vector<int> res;
    vector<vector<int>> ans;

public:
    void backtrack(vector<int> &nums, int index){
        if (index == nums.size())
            return;
        for (int i = index; i < nums.size(); i++){
            if (i > index && nums[i] == nums[i-1])
                continue;
            int ele = nums[i];
            res.push_back(ele);
            ans.push_back(res);
            backtrack(nums, i+1);
            res.pop_back();
        }
    }

    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        
        backtrack(nums, 0);
        ans.push_back({});

        return ans;        
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了85.09%的用户

内存消耗：7.7 MB, 在所有 C++ 提交中击败了32.17%的用户