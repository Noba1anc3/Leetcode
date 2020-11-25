Given the root of a binary tree, return the preorder traversal of its nodes' values.

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
        else{
            int mid = nums.size() / 2;

            vector<int> Left(nums.begin(), nums.begin() + mid);
            vector<int> Right(nums.begin() + mid, nums.end());

            int LeftMax = maxSubArray(Left);
            int RightMax = maxSubArray(Right);
            int GapMax = maxGapArray(nums);

            return max(max(LeftMax, RightMax), GapMax);
        }
    }

    int maxGapArray(vector<int> nums){
        int mid = nums.size() / 2;
        int left = mid - 1;
        int right = mid;

        int LeftMax = nums[left];
        int RightMax = nums[right];
        int LeftCur = LeftMax;
        int RightCur = RightMax;

        for (int i = left - 1; i >= 0; i--){
            LeftCur += nums[i];
            if (LeftCur > LeftMax){
                LeftMax = LeftCur;
            }
        }
        for (int i = right + 1; i < nums.size(); i++){
            RightCur += nums[i];
            if (RightCur > RightMax){
                RightMax = RightCur;
            }
        }

        return LeftMax + RightMax;        
    }
};
```

执行用时：56 ms, 在所有 C++ 提交中击败了5.47%的用户

内存消耗：45 MB, 在所有 C++ 提交中击败了5.02%的用户

### Dynamic Programming Algorithm - O(n)

c++

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int curAns = 0, maxAns = nums[0];
        for (const auto &x: nums) {
            curAns = max(curAns + x, x);
            maxAns = max(maxAns, curAns);
        }
        return maxAns;
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了65.74%的用户

内存消耗：13.3 MB, 在所有 C++ 提交中击败了12.45%的用户