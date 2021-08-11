Given an integer array nums, return true if there exists a triple of indices (i, j, k) such that i < j < k and nums[i] < nums[j] < nums[k]. If no such indices exists, return false.

 
```
Example 1:

Input: nums = [1,2,3,4,5]
Output: true
Explanation: Any triplet where i < j < k is valid.
Example 2:

Input: nums = [5,4,3,2,1]
Output: false
Explanation: No triplet exists.
Example 3:

Input: nums = [2,1,5,0,4,6]
Output: true
Explanation: The triplet (3, 4, 5) is valid because nums[3] == 0 < nums[4] == 4 < nums[5] == 6.
```

## Idea

两遍遍历的双指针方法可以解决该问题。

每轮遍历的目的在于寻找可以成为中间数j的位置，正向遍历时如果当前数是最小数则其不可能成为j，反向遍历时如果当前数十最大数则亦不可能成为j。

## Solution

```c++
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int minNum = INT_MAX, maxNum = INT_MIN;
        vector<bool> JForward(nums.size()-1, false), JBackward(nums.size()-1, false);

        for(int i = 0; i < nums.size(); i++)
            if (nums[i] <= minNum)
                minNum = nums[i];
            else
                JForward[i] = true;

        for(int i = nums.size() - 1; i >= 0; i--)
            if (nums[i] >= maxNum)
                maxNum = nums[i];
            else
                if (JForward[i]) return true;
                else JBackward[i] = true;

        return false;
    }
};
```

执行用时：48 ms, 在所有 C++ 提交中击败了81.07%的用户

内存消耗：60.2 MB, 在所有 C++ 提交中击败了7.06%的用户
