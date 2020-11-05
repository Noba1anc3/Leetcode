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

### Time Complexity : O(n·logn)  ;  Space Complexity : O(1)

c++

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        sort(nums.begin(), nums.end());
    }
};
```

## Store Value in Three Vectors

### Time Complexity : O(n)  ;  Space Complexity : O(n)

c++

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        vector<int> zero, one, two;
        for (int i = 0; i < nums.size(); i++){
            if (nums[i] == 0){
                zero.push_back(0);
            }
            else if (nums[i] == 1){
                one.push_back(1);
            }
            else{
                two.push_back(2);
            }
        }
        zero.insert(zero.end(), one.begin(), one.end());
        zero.insert(zero.end(), two.begin(), two.end());
        nums = zero;
    }
};
```

执行用时：0 ms, 在所有 Python3 提交中击败了100.0%的用户  
内存消耗：8.9 MB, 在所有 Python3 提交中击败了5.09%的用户

Attention:
- vector.insert(vector.end(), vector1.begin(), vector1.end())

## Loop Once without Extra Space Usage

### Time Complexity : O(n)  ; Space Complexity : O(1)

c++

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int zero_end = -1;
        for (int i = 0; i < nums.size(); i++){
            if (nums[i] == 0){
                zero_end += 1;
                swap(nums[i], nums[zero_end]);
            }
        }
        int one_end = zero_end;
        for (int i = zero_end + 1; i < nums.size(); i++){
            if (nums[i] == 1){
                one_end += 1;
                swap(nums[i], nums[one_end]);
            }
        }
    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户  

内存消耗：8.4 MB, 在所有 C++ 提交中击败了14.35%的用户

Attention:

- swap(a, b)

## Quick-Sort Based

### Time Complexity : O(n)  ; Space Complexity : O(1)

c++

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int size = nums.size();
        if (size < 2) {
            return;
        }

        // all in [0, zero) = 0
        // all in [zero, i) = 1
        // all in [two, len - 1] = 2

        int i = 0;
        int zero = 0;
        int two = size;
        
        while (i < two) {
            if (nums[i] == 0) {
                swap(nums[zero], nums[i]);
                zero++;
                i++;
            } else if (nums[i] == 1) {
                i++;
            } else {
                two--;
                swap(nums[i], nums[two]);
            }
        }
    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户  

内存消耗：8.6 MB, 在所有 C++ 提交中击败了5.09%的用户

Attention:

- swap(a, b)
- 边界条件
- 快排的partition天然的创造了一次partition完成分割，0在左，1在中间，2在右边

