Given an array of non-negative integers `nums`, you are initially positioned at the **first index** of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

You can assume that you can always reach the last index.



**Example:**
```
Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

## Idea

如果希望跳跃的次数少，那么跳跃的距离一定是比较远的，但不代表每次都从跳到最远的地方往后跳。

这一次搜寻跳跃距离的区间是上次搜寻空间的终点的后一位开始，到上次搜寻得到的最远距离为止。

## Solution

```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        int start = 0, end = 0, steps = 0, maxDist = 0;
        while (end < nums.size() - 1){
            for (int i = start; i <= end; i++){
                maxDist = max(maxDist, i + nums[i]);
            }
            start = end + 1;
            end = maxDist;
            steps++;
        }
        return steps;
    }
};
```

执行用时：8 ms, 在所有 C++ 提交中击败了44.66%的用户

内存消耗：15.8 MB, 在所有 C++ 提交中击败了41.10%的用户
