Given a non-empty array of integers `nums`, every element appears twice except for one. Find that single one.



 **Example:**

```
Example 1:

Input: nums = [2,2,1]
Output: 1

Example 2:

Input: nums = [4,1,2,1,2]
Output: 4

Example 3:

Input: nums = [1]
Output: 1
```

## Solution
### c++

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ret = 0;
        for (int n : nums)
            ret ^= n;
        return ret;
    }
};
```

执行用时：28 ms, 在所有 C++ 提交中击败了89.24%的用户

内存消耗：16.6 MB, 在所有 C++ 提交中击败了87.64%的用户

Attention

- ```c++
  int ret = 0;
  for (int n : nums)
  	ret ^= n;
  ```

