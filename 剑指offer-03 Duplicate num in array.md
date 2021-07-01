找出数组中重复的数字。



在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。



Examples:

```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```

## Solution

### c++

```python
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        unordered_set<int> numset;
        for (const int& num : nums){
            if (numset.count(num))
                return num;
            numset.insert(num);
        }
        return 0;
    }
};
```
执行用时：52 ms, 在所有 C++ 提交中击败了50.33%的用户

内存消耗：26.8 MB, 在所有 C++ 提交中击败了35.52%的用户
