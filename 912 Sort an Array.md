Given an array of integers `nums`, sort the array in ascending order.

 **Example:**

```
Input: nums = [5,2,3,1]
Output: [1,2,3,5]
```

## Solution

### Quick Sort

Lecture-04 Polynomial Multiplication & Quicksort - p75

#### 前后双指针分割

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

#### 首尾双指针分割

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

### Heap Sort

Lecture-05 Heapsort & Lower Bound - p6

```c++
class HeapSort {
private:
    vector<int> Heap;

public:
    int _get_parent(int i){
        if (i == 0) return 0;
        if (i % 2 == 0) return i / 2 - 1;
        else return i / 2;
    }

    void _build_heap(vector<int> nums){
        for (int i = 0; i < nums.size(); i++){
            Heap.push_back(nums[i]);
            int curIndex = i;
            while (Heap[_get_parent(curIndex)] > Heap[curIndex]){
                swap(Heap[_get_parent(curIndex)], Heap[curIndex]);
                curIndex = _get_parent(curIndex);
            }
        }
    }

    void _insert(int num){
        int curIndex = Heap.size();
        Heap.push_back(num);
        while (Heap[_get_parent(curIndex)] > Heap[curIndex]){
            swap(Heap[_get_parent(curIndex)], Heap[curIndex]);
            curIndex = _get_parent(curIndex);
        }
    }

    int _extract_min(){
        int root = 0;
        int minEle = Heap[root];
        Heap[root] = Heap[Heap.size()-1];
        Heap.pop_back();

        while (true){
            int lchild = (2*root+1 < Heap.size()) ? Heap[2*root+1] : INT_MAX;
            int rchild = (2*root+2 < Heap.size()) ? Heap[2*root+2] : INT_MAX;
            if (lchild == INT_MAX || Heap[root] <= min(lchild, rchild)) break;
            if (lchild <= min(Heap[root], rchild)){
                swap(Heap[root], Heap[2*root+1]);
                root = 2*root+1;
            }
            else if (rchild < min(Heap[root], lchild)){
                swap(Heap[root], Heap[2*root+2]);
                root = 2*root+2;
            }
        }

        return minEle;
    }

    vector<int> sortArray(vector<int>& nums) {
        vector<int> rsp;
        _build_heap(nums);
        for (int i = 0; i < nums.size(); i++)
            rsp.push_back(_extract_min());
        return rsp;
    }
};
```

执行用时：80 ms, 在所有 C++ 提交中击败了44.28%的用户

内存消耗：31.2 MB, 在所有 C++ 提交中击败了8.01%的用户

### Merge Sort

```c++
class Solution {
public:
    vector<int> MergeLists(vector<int>& left, vector<int>& right){
        int i = 0, j = 0;
        vector<int> rsp;

        while (i < left.size() && j < right.size())
            rsp.push_back((left[i] < right[j]) ? left[i++] : right[j++]);

        if (i < left.size())
            rsp.insert(rsp.end(), left.begin() + i, left.end());
        else
            rsp.insert(rsp.end(), right.begin() + j, right.end());
        
        return rsp;
    }

    vector<int> MergeSort(vector<int>& nums){
        if (nums.size() == 1)
            return nums;

        int mid = nums.size() / 2;
        vector<int> left(nums.begin(), nums.begin() + mid);
        vector<int> right(nums.begin() + mid, nums.end());

        left = MergeSort(left);
        right = MergeSort(right);

        return MergeLists(left, right);
    }

    vector<int> sortArray(vector<int>& nums) {
        return MergeSort(nums);
    }
};
```

### Count Sort

Lecture-08 Midterm Review - p12

仅限非负数组，非基于比较的排序算法

![](http://r.photo.store.qq.com/psc?/fef49446-40e0-48c4-adcc-654c5015022c/TmEUgtj9EK6.7V8ajmQrEAc33.VcF06TJP6HI3KS6o*QBdymPx7ngVeDVYG0T3pssqi.1hGwseWXm11JhZdmvedKBd2KMjJ.7Y.mIXp8AVc!/r)

```c++
class Solution {
public:
    vector<int> CountSort(vector<int>& nums) {
        int K = INT_MIN, n = nums.size();

        for (const int& num : nums)
            K = max(K, num);
            
        vector<int> sorted_nums(n, 0), C(K+1, 0);
                    
        for (int i = 0; i < n; i++)
            C[nums[i]] += 1;

        for (int i = 1; i <= K; i++)
            C[i] += C[i-1];

        for (int i = n-1; i >= 0; i--){
            C[nums[i]] -= 1;
            sorted_nums[C[nums[i]]] = nums[i];
        }

        return sorted_nums;
    }
};
```

Time Complexity : O(n + k)
