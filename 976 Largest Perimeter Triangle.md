Given an array `A` of positive lengths, return the largest perimeter of a triangle with **non-zero area**, formed from 3 of these lengths.

If it is impossible to form any triangle of non-zero area, return `0`.



Examples:

```
Example 1:

Input: [2,1,2]
Output: 5

Example 2:

Input: [1,2,1]
Output: 0

Example 3:

Input: [3,2,3,4]
Output: 10

Example 4:

Input: [3,6,2,3]
Output: 8
```

## Solution

c++
```c++
class Solution {
public:
    static bool cmp(int a, int b){
        return a > b;
    }

    int largestPerimeter(vector<int>& nums) {
        sort(nums.begin(), nums.end(), cmp);
        for (int i = 2; i < nums.size(); i++)
            if (nums[i-2] < nums[i-1] + nums[i])
                return nums[i-2] + nums[i-1] + nums[i];
        return 0;
    }
};
```

执行用时：44 ms, 在所有 C++ 提交中击败了55.12%的用户

内存消耗：21.5 MB, 在所有 C++ 提交中击败了6.73%的用户
