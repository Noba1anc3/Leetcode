Given a binary array nums and an integer k, return the maximum number of consecutive 1's in the array if you can flip at most k 0's.

```
Example 1:

Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
Output: 6

Explanation: [1,1,1,0,0,1,1,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined

Example 2:

Input: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
Output: 10

Explanation: [0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
```

## Solution - Slide Window

右指针主动右移，如果右指针元素为0，则窗口内为0的元素加一，每次循环都计算窗口长度以更新最大值

左指针被动右移，只要窗口内0元素个数超过k，则不断前移左指针，遇到1则无视，遇到0则减少0元素的个数

```c++
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int l = 0, r = 0, K = 0, ans = 0;
        while(r < nums.size()){
            if(nums[r] == 0)
                K++;
            while(K > k)
                if(nums[l++] == 0)
                    K--;
            ans = max(ans, r-l+1);
            r++;
        }
        return ans;
    }
};
```

执行用时：52 ms, 在所有 C++ 提交中击败了69.93%的用户

内存消耗：54.1 MB, 在所有 C++ 提交中击败了55.30%的用户
