Given an array of **distinct** integers `candidates` and a target integer `target`, return a list of all **unique combinations** of candidates where the chosen numbers sum to `target`. You may return the combinations **in any order**.

The **same** number may be chosen from candidates an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.



**Example:**

```
Example 1:

Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.

Example 2:

Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]

Example 3:

Input: candidates = [2], target = 1
Output: []

Example 4:

Input: candidates = [1], target = 1
Output: [[1]]

Example 5:

Input: candidates = [1], target = 2
Output: [[1,1]]
```

## Idea - I 回溯

首先排序原数组，这样可以在target越来越小的时候及时退出（小的元素都超过了，更不用说大的元素了）

回溯算法讲究还原，无论是匹配上了还是没匹配上都要pop回去

![](https://pic.leetcode-cn.com/1598091943-hZjibJ-file_1598091940241)

说明：

- 以 target = 7 为 **根结点** ，创建一个分支的时 **做减法** ；
- 每一个箭头表示：父结点的数值减去边上的数值，得到子结点的数值。边的值就是题目中给出的 `candidate` 数组的每个元素的值；
- 减到 0 或者负数的时候停止，即：结点 0 和负数结点成为叶子结点；
- 所有从根结点到结点 0 的路径（只能从上往下，没有回路）就是题目要找的一个结果。

## Solution

自己写的


```c++
class Solution {
private:
    vector<int> ans;
    vector<vector<int>> ans_list;

public:
    void backtrack(vector<int>& candidates, int target){
        for(int i = 0; i < candidates.size(); i++){
            int candidate = candidates[i];
            if (candidate > target){
                break;
            }
            if (candidate == target){
                ans.push_back(candidate);
                auto new_ans = ans;
                sort(new_ans.begin(), new_ans.end());
                ans_list.push_back(new_ans);
                ans.pop_back();
            }
            else{
                ans.push_back(candidate);
                backtrack(candidates, target - candidate);
                ans.pop_back();
            }
        }
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        backtrack(candidates, target);
        sort(ans_list.begin(), ans_list.end());
        ans_list.erase(unique(ans_list.begin(), ans_list.end()), ans_list.end());
        return ans_list;
    }
};
```

执行用时：192 ms, 在所有 C++ 提交中击败了5.65%的用户

内存消耗：20.6 MB, 在所有 C++ 提交中击败了12.99%的用户

Attention

- ```c++
  sort(list.begin(), list.end());
  list.erase(unique(list.begin(), list.end()), list.end());
  ```

## Solution

这是别人写的

```c++
class Solution {
private:
    vector<int> res;
    vector<vector<int>> ans;

public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        backtracking(candidates, target, 0);
        return ans;
    }

    void backtracking(vector<int>& candidates, int target, int index){
        if(target < 0) return;

        if(target == 0){
            ans.push_back(res);
            return;
        }

        for(int i = index; i < candidates.size(); i++){ // 不会错，只会多访问此前遍历时访问过的节点
            // candidates = [2,3,5], target = 8
            // i from 0 : [[2,2,2,2],[2,3,3],[3,2,3],[3,3,2],[3,5],[5,3]]
            // i from index : [[2,2,2,2],[2,3,3],[3,5]]
            res.push_back(candidates[i]);
            backtracking(candidates, target - candidates[i], i);
            res.pop_back();
        }
    }
};
```

执行用时：8 ms, 在所有 C++ 提交中击败了92.50%的用户

内存消耗：11.2 MB, 在所有 C++ 提交中击败了58.98%的用户



