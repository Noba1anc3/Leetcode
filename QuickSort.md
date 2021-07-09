## 前后双指针分割

```c++
class Solution {
public:
    int partition(vector<int>& nums, int p, int q) {
        int pivotIndex = rand() % (q - p + 1) + p;
        swap(nums[q], nums[pivotIndex]);

        int i = p - 1;
        int pivot = nums[q];

        for (int j = p; j < q; j++){
            if (nums[j] < pivot){
                i++;
                swap(nums[i], nums[j]);
            }
        }

        swap(nums[i+1], nums[q]);

        return i+1;
    }

    void QuickSort(vector<int>& nums, int p, int q) {
        if (p < q) {
            int r = partition(nums, p, q);
            QuickSort(nums, p, r-1);
            QuickSort(nums, r+1, q);
        }
    }

    vector<int> sortArray(vector<int>& nums) {
        QuickSort(nums, 0, nums.size() - 1);
        return nums;
    }
};
```

执行用时：48 ms, 在所有 C++ 提交中击败了80.23%的用户

内存消耗：27.8 MB, 在所有 C++ 提交中击败了47.27%的用户

## 首尾双指针分割

```c++
class Solution {
public:
    int partition(vector<int>& nums, int p, int q) {
        int pivotIndex = rand() % (q - p + 1) + p;
        swap(nums[q], nums[pivotIndex]);

        int pivot = nums[q];
        int l = p, r = q-1;

        while (true) {
            while (l <= q && nums[l] < pivot) l++;
            while (r >= p && nums[r] > pivot) r--;
            if (l > r) break;
            swap(nums[l++], nums[r--]);
        }

        swap(nums[l], nums[q]);
        return l;
    }

    void QuickSort(vector<int>& nums, int p, int q) {
        if (p < q) {
            int r = partition(nums, p, q);
            QuickSort(nums, p, r-1);
            QuickSort(nums, r+1, q);
        }
    }

    vector<int> sortArray(vector<int>& nums) {
        QuickSort(nums, 0, nums.size() - 1);
        return nums;
    }
};
```

执行用时：40 ms, 在所有 C++ 提交中击败了85.32%的用户

内存消耗：27.8 MB, 在所有 C++ 提交中击败了30.24%的用户
