Given an array of integers `nums`, sort the array in ascending order.

 **Example:**

```
Input: nums = [5,2,3,1]
Output: [1,2,3,5]
```

## Solution
### Quicksort

c++

```c++
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        QuickSort(nums, 0, nums.size() - 1);
        return nums;
    }

    void QuickSort(vector<int>& nums, int p, int q){
        if (p < q){
            int r = Partition(nums, p, q);
            QuickSort(nums, p, r-1);
            QuickSort(nums, r+1, q);
        }
    }

    int Partition(vector<int>& nums, int p, int q){
        int i = p - 1;
        int pivot = nums[q];

        for (int j = p; j < q; j++){
            if (nums[j] <= pivot){
                i += 1;
                swap(nums[i], nums[j]);
            }
        }

        swap(nums[i+1], nums[q]);

        return i + 1;
    }
};

```

执行用时：44 ms, 在所有 C++ 提交中击败了84.28%的用户

内存消耗：15.6 MB, 在所有 C++ 提交中击败了30.92%的用户

Attention:

- 快排的退出条件不能忘记
- &传地址
- swap