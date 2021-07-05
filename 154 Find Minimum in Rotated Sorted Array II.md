Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`).

Find the minimum element.

The array may contain **duplicates**.



 **Example:**

```
Example 1:

Input: [1,3,5]
Output: 1

Example 2:

Input: [2,2,2,0,1]
Output: 0
```

## Solution
### c++

```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int left = 0, right = nums.size() - 1;
        while (left < right){
            int mid = (left + right) / 2;

            if (nums[mid] < nums[right]) 
                right = mid; // mid可能是答案
            else if (nums[mid] > nums[right]) 
                left = mid + 1;
            else
                right--;
        }

        return nums[right];
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了92.70%的用户

内存消耗：11.9 MB, 在所有 C++ 提交中击败了89.73%的用户
