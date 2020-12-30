The set `[1, 2, 3, ..., n]` contains a total of `n!` unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for `n = 3`:

```
"123"
"132"
"213"
"231"
"312"
"321"
```


Given `n` and `k`, return the `kth` permutation sequence.



**Example:**

```
Example 1:

Input: n = 3, k = 3
Output: "213"

Example 2:

Input: n = 4, k = 9
Output: "2314"

Example 3:

Input: n = 3, k = 1
Output: "123"
```

Constraints:

1 <= n <= 9
1 <= k <= n!

## Solution - I 回溯

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

    string getPermutation(int n, int k) {
        vector<int> nums;
        for (int i = 1; i <= n; i++)
            nums.push_back(i);
        backtrack(nums);

        string permutation;
        for(int i : ans_list[k-1]){
            permutation += to_string(i);
        }
        return permutation;
    }
};
```

**48 / 200** 个通过测试用例

## Solution - II 回溯剪枝

通过除法运算得到结尾排列的开头元素，将除了开头元素之外的数组传入回溯算法计算其后的排列，当计算的排列个数达到取余运算所要求的个数时返回至主函数。


```c++
class Solution {
private:
    int ans_num;
    vector<int> res;
    vector<int> ans;

public:
    int jiecheng(int n){
        int ans = 1;
        for (int i = 1; i <= n; i++)
            ans *= i;
        return ans;
    }

    void backtrack(vector<int>& nums, int total){
        if (nums.empty()){
            ans = res;
            ans_num++;
            return;
        }
        for (int i = 0; i < nums.size(); i++){
            int num = nums[i];
            res.push_back(num);
            vector<int> new_nums = nums;
            new_nums.erase(new_nums.begin() + i);
            backtrack(new_nums, total);
            if (ans_num == total)
                return;
            res.pop_back();
        }
    }

    string getPermutation(int n, int k) {
        int x = jiecheng(n-1);
        int a = (k - 1) / x + 1;
        int b = k % x;
        
        vector<int> nums;
        for (int i = 1; i < a; i++) nums.push_back(i);
        for (int i = a + 1; i <= n; i++) nums.push_back(i);

        backtrack(nums, b);

        string permutation = to_string(a);
        for(int i : ans) permutation += to_string(i);
        
        return permutation;
    }
};
```

执行用时：1984 ms, 在所有 C++ 提交中击败了5.12%的用户

内存消耗：116.4 MB, 在所有 C++ 提交中击败了5.12%的用户

Attention

- 优化点：

  - ```
    vector<vector<int>> ans  ->  vector<ans> + int ans_num
    ```

  - 阶乘计算顺序调整为从小到大

  - new_num由insert改为erase