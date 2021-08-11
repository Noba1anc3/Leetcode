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

## Solution - DP

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
