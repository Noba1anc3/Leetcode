```c++
class QuickSort {
public:
    int partition(vector<int>& arr, int left, int right) {
        int pivotIndex = rand() % (right - left + 1) + left;
        swap(arr[right], arr[pivotIndex]);

        int pivot = right;
        int lt = left;
        int rt = right-1;

        while (true) {
            while (lt <= right && arr[lt] < arr[pivot]) lt++;
            while (rt >= left && arr[rt] > arr[pivot]) rt--;
            if (lt > rt) break;
            swap(arr[lt++], arr[rt--]);
        }

        swap(arr[pivot], arr[lt]);
        return lt;
    }

    vector<int> QuickSort(vector<int>& nums, int i, int j) {
        if (i > j) return nums;
        int r = partition(nums, i, j);
        QuickSort(nums, i, r-1);
        QuickSort(nums, r+1, j);

        return nums;
    }

    vector<int> sortArray(vector<int>& nums) {
        return QuickSort(nums, 0, nums.size() - 1);
    }
};
```
