Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.



Examples:

```
Example 1:

Input: [3,2,1,5,6,4] and k = 2
Output: 5

Example 2:

Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```

## Solution

### Brute Force

```c++
class Solution {
public:
    int findKthLargest(vector<int>& arr, int k) {
        sort(arr.begin(), arr.end());
        return arr[arr.size() - k];
    }
};
```

执行用时：24 ms, 在所有 C++ 提交中击败了57.93%的用户

内存消耗：9.7 MB, 在所有 C++ 提交中击败了59.41%的用户

### Iterative QuickSort

#### Non-Randomized

```c++
class Solution {
public:
    int partition(vector<int>& arr, int left, int right) {
        int pivot = right;
        int lt = left;
        int rt = right-1;

        while (true) {
            while (lt <= right && arr[lt] < arr[pivot]) lt++;
            while (rt >= left && arr[rt] > arr[pivot]) rt--;
            if (lt > rt) break;
            swap(arr[lt++], arr[rt--]);
        }

        swap(arr[lt], arr[pivot]);
        return lt;
    }

    int findKthLargest(vector<int>& arr, int k) {
        int left = 0, right = arr.size() -1;
        while (true){
            int pivot = partition(arr, left, right);
            if (pivot + k == arr.size())
                break;
            else if (pivot + k < arr.size())
                left = pivot + 1;
            else 
                right = pivot - 1;
        };
        return arr[arr.size() - k];
    }
};
```

执行用时：92 ms, 在所有 C++ 提交中击败了16.76%的用户

内存消耗：9.6 MB, 在所有 C++ 提交中击败了91.12%的用户

#### Randomized

```c++
class Solution {
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

        swap(arr[lt], arr[pivot]);
        return lt;
    }

    int findKthLargest(vector<int>& arr, int k) {
        int left = 0, right = arr.size() -1;
        while (true){
            int pivot = partition(arr, left, right);
            if (pivot + k == arr.size())
                break;
            else if (pivot + k < arr.size())
                left = pivot + 1;
            else 
                right = pivot - 1;
        };
        return arr[arr.size() - k];
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了98.72%的用户

内存消耗：9.8 MB, 在所有 C++ 提交中击败了34.41%的用户

### Recursive Quicksort

#### Non-Randomized

```c++
class Solution {
public:
    int findKthLargest(vector<int>& arr, int k) {
        QuickSort(arr, 0, arr.size() - 1, k);
        return arr[arr.size() - k];
    }

    void QuickSort(vector<int>& nums, int p, int q, int k){
        if (p < q){
            int r = Partition(nums, p, q);
            int num = q - r + 1;

            if (num == k)
                return;
            else if (num > k)
                QuickSort(nums, r+1, q, k);
            else
                QuickSort(nums, p, r-1, k-num);
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

执行用时：128 ms, 在所有 C++ 提交中击败了13.68%的用户

内存消耗：9.8 MB, 在所有 C++ 提交中击败了46.25%的用户

#### Randomized

```c++
class Solution {
public:
    int findKthLargest(vector<int>& arr, int k) {
        QuickSort(arr, 0, arr.size() - 1, k);
        return arr[arr.size() - k];
    }

    void QuickSort(vector<int>& nums, int p, int q, int k){
        if (p < q){
            int r = Partition(nums, p, q);
            int num = q - r + 1;

            if (num == k)
                return;
            else if (num > k)
                QuickSort(nums, r+1, q, k);
            else
                QuickSort(nums, p, r-1, k-num);
        }
    }

    int Partition(vector<int>& nums, int p, int q){
        int pivotIndex = rand() % (q - p + 1) + p;
        swap(nums[pivotIndex], nums[q]);
        
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

执行用时：4 ms, 在所有 C++ 提交中击败了98.70%的用户

内存消耗：9.8 MB, 在所有 C++ 提交中击败了36.42%的用户
