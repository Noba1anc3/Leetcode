Given an array `nums` with `n` objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

Here, we will use the integers `0`, `1`, and `2` to represent the color red, white, and blue respectively.



Examples:

```
Example 1:

Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]

Example 2:

Input: nums = [2,0,1]
Output: [0,1,2]

Example 3:

Input: nums = [0]
Output: [0]
```

# Solution

## Sort Lib

### Time Complexity : O(n logn); Space Complexity : O(1)

c++

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        sort(nums.begin(), nums.end());
    }
};
```

## Based on Vector

c++

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        vector<int> zero_one_two(3, 0);
        vector<int> rsp;
        
        for (const int& num : nums)
            zero_one_two[num]++;

        for (int i = 0; i < 3; i++)
            for (int j = 0; j < zero_one_two[i]; j++)
                rsp.push_back(i);
        
        nums = rsp;
        
    }
};
```

## Loop Once without Extra Space Usage

### Time Complexity : O(n); Space Complexity : O(1)

c++

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int zero_end = -1;
        for (int i = 0; i < nums.size(); i++)
            if (nums[i] == 0)
                swap(nums[i], nums[++zero_end]);
        for (int i = zero_end + 1; i < nums.size(); i++)
            if (nums[i] == 1)
                swap(nums[i], nums[++zero_end]);
    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户  

内存消耗：8.1 MB, 在所有 C++ 提交中击败了30.35%的用户

Attention:
- swap(a, b)

## Quick-Sort Based

### Time Complexity : O(n); Space Complexity : O(1)

c++

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int i = 0;
        int zero = -1, two = nums.size();
        
        while (i < two)
            if (nums[i] == 0)
                swap(nums[i++], nums[++zero]);
            else if (nums[i] == 1)
                i++;
            else
                swap(nums[i], nums[--two]);
    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户  

内存消耗：8.1 MB, 在所有 C++ 提交中击败了15.09%的用户

Attention:
- swap(a, b)
- 边界条件
- 快排的partition天然的创造了一次partition完成分割，0在左，1在中间，2在右边

