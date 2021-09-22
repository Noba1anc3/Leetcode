Given an integer array nums, return the length of the longest strictly increasing subsequence.

A subsequence is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, [3,6,2,7] is a subsequence of the array [0,3,1,6,2,2,7].
 
```
Example 1:

Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

Example 2:

Input: nums = [0,1,0,3,2,3]
Output: 4

Example 3:

Input: nums = [7,7,7,7,7,7,7]
Output: 1
```

## Idea - DP

Time Complexity : o(n2)

### Solution - DP

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> dp(nums.size(), 1);

        for (int i = 0; i < nums.size(); i++)
            for (int j = 0; j < i; j++){
                if (nums[i] > nums[j])
                    dp[i] = max(dp[i], dp[j] + 1);
            }

        return *max_element(dp.begin(), dp.end());
    }
};
```

执行用时：268 ms, 在所有 C++ 提交中击败了52.70%的用户

内存消耗：10.3 MB, 在所有 C++ 提交中击败了44.35%的用户

## Idea - Greedy + Half-Search

### Solution

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if (nums.empty()) return 0;
        
        vector<int> up_vec = {nums[0]};

        for (int i = 1; i < nums.size(); i++)
            if (nums[i] > up_vec.back())
                up_vec.push_back(nums[i]);
            else{
                int l = 0, r = up_vec.size(), pos = -1;
                while (l <= r){
                    int mid = (l + r) >> 1;
                    if (up_vec[mid] < nums[i]){
                        l = mid + 1;
                        pos = mid;
                    }
                    else{
                        r = mid - 1;
                    }
                }
                up_vec[pos + 1] = nums[i]; 
            }

        return up_vec.size();
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了99.21%的用户

内存消耗：10.1 MB, 在所有 C++ 提交中击败了85.49%的用户
