Given two sorted arrays nums1 and nums2 of size m and n respectively, return the **median** of the two sorted arrays.

Follow up: The overall run time complexity should be O(log (m+n)).


**Example:**
```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.

Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.

Input: nums1 = [0,0], nums2 = [0,0]
Output: 0.00000

Input: nums1 = [], nums2 = [1]
Output: 1.00000

Input: nums1 = [2], nums2 = []
Output: 2.00000
```

## Solution - I

如果使用O(m+n)的时间复杂度和空间复杂度，则可以先将两列表合并，寻找其中位数

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int i = 0, j = 0;
        vector<int> nums;

        while (i < nums1.size() && j < nums2.size())
            if (nums1[i] < nums2[j]) 
                nums.push_back(nums1[i++]);
            else 
                nums.push_back(nums2[j++]);

        if (i == nums1.size())
            while (j < nums2.size())
                nums.push_back(nums2[j++]);
        else
            while (i < nums1.size())
                nums.push_back(nums1[i++]);

        if (nums.size() % 2 == 0) 
            return float(nums[nums.size() / 2 - 1] + nums[nums.size() / 2]) / 2;
        return nums[nums.size() / 2];
    }
};
```

执行用时：36 ms, 在所有 C++ 提交中击败了82.76%的用户

内存消耗：87.6 MB, 在所有 C++ 提交中击败了14.01%的用户



## Solution - II
### Python

```python
class Solution:
    def findMedianSortedArrays(self, nums1, nums2) -> float:
        if len(nums1) > len(nums2):
            nums1, nums2 = nums2, nums1
        len1, len2 = len(nums1), len(nums2)

        left, right, half_len = 0, len1, (len1 + len2 + 1) // 2
        mid1 = (left + right) // 2
        mid2 = half_len - mid1

        while left < right:
            if mid1 < len1 and nums1[mid1] < nums2[mid2 - 1]:
                left = mid1 + 1
            else:
                right = mid1
            mid1 = (left + right) // 2
            mid2 = half_len - mid1

        if mid1 == 0: max_of_left = nums2[mid2 - 1]
        elif mid2 == 0: max_of_left = nums1[mid1 - 1]
        else: max_of_left = max(nums1[mid1 - 1], nums2[mid2 - 1])

        if (len1 + len2) % 2 == 1: return max_of_left

        if mid1 == len1: min_of_right = nums2[mid2]
        elif mid2 == len2: min_of_right = nums1[mid1]
        else: min_of_right = min(nums1[mid1], nums2[mid2])

        return (max_of_left + min_of_right) / 2
```

执行用时：48 ms, 在所有 Python3 提交中击败了87.18%的用户

内存消耗：15.3 MB, 在所有 Python3 提交中击败了7.26%的用户

### C++

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        if (nums1.size() > nums2.size())
            return findMedianSortedArrays(nums2, nums1);

        int len1 = nums1.size(), len2 = nums2.size(), half_len = (len1 + len2 + 1) / 2;
        int left = 0, right = len1;
        int mid1 = (left + right) / 2, mid2 = half_len - mid1;
        int max_of_left, min_of_right;

        while (left < right){
            if (nums1[mid1] < nums2[mid2 - 1])
                left = mid1 + 1;
            else
                right = mid1;
            mid1 = (left + right) / 2;
            mid2 = half_len - mid1;
        }

        if (mid1 == 0) max_of_left = nums2[mid2 - 1];
        else if (mid2 == 0) max_of_left = nums1[mid1 - 1];
        else max_of_left = max(nums1[mid1 - 1], nums2[mid2 - 1]);

        if ((len1 + len2) % 2 == 1) return max_of_left;

        if (mid1 == len1) min_of_right = nums2[mid2];
        else if (mid2 == len2) min_of_right = nums1[mid1];
        else min_of_right = min(nums1[mid1], nums2[mid2]);

        return float(max_of_left + min_of_right) / 2;
    }
};
```

执行用时：20 ms, 在所有 C++ 提交中击败了99.03%的用户

内存消耗：86.8 MB, 在所有 C++ 提交中击败了86.79%的用户

