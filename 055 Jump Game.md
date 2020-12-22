Given an array of non-negative integers `nums`, you are initially positioned at the **first index** of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.



**Example:**
```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```

## Idea

设想一下，对于数组中的任意一个位置 y，我们如何判断它是否可以到达？

根据题目的描述，只要存在一个可以到达的位置 x，它可达的最大长度为 `x+nums[x]`，这个值大于等于 y，即 `x + nums[x] ≥ y`，那么位置 y 也可以到达。

换句话说，对于每一个可以到达的位置 x，它使得 x+1,x+2,⋯,x+nums[x] 这些连续的位置都可以到达。

这样以来，我们依次遍历数组中的每一个位置，并实时维护 **最远可以到达的位置**。对于当前遍历到的位置 x，如果它在 **最远可以到达的位置** 的范围内，那么我们就可以从起点通过若干次跳跃到达该位置，因此我们可以用 x+nums[x] 更新 最远可以到达的位置。

在遍历的过程中，如果 最远可以到达的位置 大于等于数组中的最后一个位置，那就说明最后一个位置可达，我们就可以直接返回 `True` 作为答案。反之，如果在遍历结束后，最后一个位置仍然不可达，我们就返回 `False` 作为答案。

## Solution

```c++
class Solution {
public:
    bool canJump(vector<int>& nums) { 
        int maxDistance = 0;

        for (int i = 0; i < nums.size(); i++){
            if (maxDistance >= i)
                maxDistance = max(maxDistance, i + nums[i]);
            if (maxDistance >= nums.size() - 1)
                return true;
        }
        return false;
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了99.81%的用户

内存消耗：13 MB, 在所有 C++ 提交中击败了30.36%的用户