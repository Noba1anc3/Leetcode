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

### Non-Recursive QuickSort

#### Non-Randomized

c++

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
            swap(arr[lt], arr[rt]);
            lt++;
            rt--;
        }

        swap(arr[pivot], arr[lt]);
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

执行用时：96 ms, 在所有 C++ 提交中击败了13.84%的用户

内存消耗：10 MB, 在所有 C++ 提交中击败了17.33%的用户

Attention:
- 边界情况

#### Randomized

c++

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
            swap(arr[lt], arr[rt]);
            lt++;
            rt--;
        }

        swap(arr[pivot], arr[lt]);
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

执行用时：8 ms, 在所有 C++ 提交中击败了99.62%的用户

内存消耗：10.1 MB, 在所有 C++ 提交中击败了15.33%的用户

### Recursive Quicksort

#### Non-Randomized

c++

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

执行用时：256 ms, 在所有 C++ 提交中击败了6.75%的用户

内存消耗：10.4 MB, 在所有 C++ 提交中击败了5.08%的用户

#### Randomized

c++

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
        int pivotIndex = rand() % (q - p + 1) + p;

        swap(nums[pivotIndex], nums[q]);
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

执行用时：12 ms, 在所有 C++ 提交中击败了97.18%的用户

内存消耗：9.9 MB, 在所有 C++ 提交中击败了42.16%的用户