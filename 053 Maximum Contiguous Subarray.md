Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

```
Example 1:

Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.

Example 2:

Input: nums = [1]
Output: 1

Example 3:

Input: nums = [5,4,-1,7,8]
Output: 23
```

## Solution

### Brute-Force Algorithm - O(n3)

c++

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int Max = nums[0];
        for (int i = 0; i < nums.size(); i++){
            for (int j = i; j < nums.size(); j++){
                int V = 0;
                for (int k = i; k <= j; k++){
                    V += nums[k];
                }
                if (V > Max){
                    Max = V;
                }
            }
        }
        return Max;
    }
};
```
**200 / 202** 个通过测试用例

状态：超出时间限制

### Data-reused Algorithm - O(n2)

c++

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int Max = nums[0];
        for (int i = 0; i < nums.size(); i++){
            int V = 0;
            for (int j = i; j < nums.size(); j++){
                V += nums[j];
                if (V > Max)
                    Max = V;
            }
        }
        return Max;
    }
};
```

执行用时：1040 ms, 在所有 C++ 提交中击败了5.01%的用户

内存消耗：13.4 MB, 在所有 C++ 提交中击败了5.42%的用户

### Divide & Conquer Algorithm - O(n logn)

c++

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if (nums.size() == 1)
            return nums[0];
        
        int mid = nums.size() / 2;

        vector<int> Left(nums.begin(), nums.begin() + mid);
        vector<int> Right(nums.begin() + mid, nums.end());

        int LeftMax = maxSubArray(Left);
        int RightMax = maxSubArray(Right);
        int GapMax = maxGapArray(nums, mid);

        return max(max(LeftMax, RightMax), GapMax);
    }

    int maxGapArray(vector<int> nums, int mid){
        int LeftCur = 0, RightCur = 0;
        int LeftMax = INT_MIN, RightMax = INT_MIN;

        for (int i = mid - 1; i >= 0; i--){
            LeftCur += nums[i];
            if (LeftCur > LeftMax)
                LeftMax = LeftCur;
        }
        for (int i = mid; i < nums.size(); i++){
            RightCur += nums[i];
            if (RightCur > RightMax)
                RightMax = RightCur;
        }

        return LeftMax + RightMax;        
    }
};
```

执行用时：44 ms, 在所有 C++ 提交中击败了6.93%的用户

内存消耗：41 MB, 在所有 C++ 提交中击败了5.14%的用户

### Dynamic Programming Algorithm - O(n)

c++

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int curAns = 0, maxAns = INT_MIN;
        for (const int& x: nums) {
            curAns = max(curAns + x, x);
            maxAns = max(maxAns, curAns);
        }
        return maxAns;
    }
};
```

执行用时：8 ms, 在所有 C++ 提交中击败了68.20%的用户

内存消耗：12.8 MB, 在所有 C++ 提交中击败了80.66%的用户
