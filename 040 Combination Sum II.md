Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note**: The solution set must not contain duplicate combinations.



**Example:**

```
Example 1:

Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
    [1,1,6],
    [1,2,5],
    [1,7],
    [2,6]
]

Example 2:

Input: candidates = [2,5,2,1,2], target = 5
Output: 
[
    [1,2,2],
    [5]
]
```

## Solution - I 回溯


```c++
class Solution {
private:
    vector<int> res;
    vector<vector<int>> ans;

public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        backtracking(candidates, target, 0);
        
        sort(ans.begin(), ans.end());
        ans.erase(unique(ans.begin(), ans.end()), ans.end());
        return ans;
    }

    void backtracking(vector<int>& candidates, int target, int index){
        if(target < 0) return;

        if(target == 0){
            //sort(new_res.begin(), new_res.end()); 最外面排序了，里面无需排序
            ans.push_back(res);
            return;
        }

        if (i > 0 && candidates[i] == candidates[i-1])
            return;
        
        for(int i = index; i < candidates.size(); i++){
            //vector<int> new_candidates;
            //new_candidates.insert(new_candidates.end(), candidates.begin(), candidates.begin() + i);
            //new_candidates.insert(new_candidates.end(), candidates.begin() + i + 1, candidates.end());

            res.push_back(candidates[i]);
            backtracking(candidates, target - candidates[i], i+1);
            res.pop_back();
        }
    }
};
```

执行用时：1068 ms, 在所有 C++ 提交中击败了6.96%的用户

内存消耗：12.1 MB, 在所有 C++ 提交中击败了29.49%的用户

Attention

- ```c++
  sort(list.begin(), list.end());
  list.erase(unique(list.begin(), list.end()), list.end());
  ```

- 与039的区别在于回溯调用时下标＋1，要进行预排序和后排序去重
- 不同于046，这道题可以在排序后直接掠过前面的下标，046只能剔除当前元素，但前面的不能忽略

## Idea - II 回溯剪枝

见图

![](https://pic.leetcode-cn.com/1599718525-iXEiiy-image.png)

## Solution - II 回溯剪枝

```c++
class Solution {
private:
    vector<int> res;
    vector<vector<int>> ans;

public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        backtracking(candidates, target, 0);
        
        // 剪枝后不需要排序了
        // sort(ans.begin(), ans.end());
        // ans.erase(unique(ans.begin(), ans.end()), ans.end());
        return ans;
    }

    void backtracking(vector<int>& candidates, int target, int index){
        if(target < 0) return;

        if(target == 0){
            ans.push_back(res);
            return;
        }
        
        for(int i = index; i < candidates.size(); i++){
            // 同一层被处理的节点，如果当前节点和前面节点的数值相同，则可以跳过（剪枝）
            if (i > index && candidates[i] == candidates[i-1])
                continue;
            
            res.push_back(candidates[i]);
            backtracking(candidates, target - candidates[i], i+1);
            res.pop_back();
        }
    }
};
```

执行用时：16 ms, 在所有 C++ 提交中击败了48.02%的用户

内存消耗：11 MB, 在所有 C++ 提交中击败了45.95%的用户