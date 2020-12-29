Given a collection of numbers, `nums`, that might contain duplicates, return all possible unique permutations in any order



**Example:**

```
Example 1:

Input: nums = [1,1,2]
Output:
[[1,1,2],
 [1,2,1],
 [2,1,1]]
 
Example 2:

Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

## Solution - I 回溯

c++


```c++
class Solution {

private:
    vector<int> ans;
    vector<vector<int>> ans_list;
public:
    void backtrack(vector<int> &nums){
        if (nums.empty()){
            ans_list.push_back(ans);
        }
        else{
            for (int i = 0; i < nums.size(); i++){
                int num = nums[i];
                ans.push_back(num);
                vector<int> newlist;
                newlist.insert(newlist.end(), nums.begin(), nums.begin() + i);
                newlist.insert(newlist.end(), nums.begin() + i + 1, nums.end());
                backtrack(newlist);
                
                ans.pop_back();
            }
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        backtrack(nums);
        sort(ans_list.begin(), ans_list.end());
        ans_list.erase(unique(ans_list.begin(), ans_list.end()), ans_list.end());
        return ans_list;
    }
};
```

执行用时：1220 ms, 在所有 C++ 提交中击败了5.04%的用户

内存消耗：48.9 MB, 在所有 C++ 提交中击败了5.00%的用户

Attention

- ```c++
  sort(list.begin(), list.end());
  list.erase(unique(list.begin(), list.end()), list.end());
  ```

## Idea II

需要使用剪枝来降低耗时，每一次递归到下一层的时候列表都会是全新的，下标也会被重置为0，不会存在因为当前元素与上一个元素相同而跳到下一个的情况。剪枝的情况只会发生在同一层当中，并且当前元素和上一个元素的数值相同，则无需处理。

## Solution

```c++
class Solution {

private:
    vector<int> ans;
    vector<vector<int>> ans_list;
public:
    void backtrack(vector<int> &nums){
        if (nums.empty())
            ans_list.push_back(ans);
        else{
            for (int i = 0; i < nums.size(); i++){
                if (i > 0 && nums[i] == nums[i-1])
                    continue;

                int num = nums[i];
                vector<int> newlist;
                newlist.insert(newlist.end(), nums.begin(), nums.begin() + i);
                newlist.insert(newlist.end(), nums.begin() + i + 1, nums.end());

                ans.push_back(num);
                backtrack(newlist);
                ans.pop_back();
            }
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        backtrack(nums);
        return ans_list;
    }
};
```

执行用时：20 ms, 在所有 C++ 提交中击败了33.74%的用户

内存消耗：9.9 MB, 在所有 C++ 提交中击败了19.91%的用户