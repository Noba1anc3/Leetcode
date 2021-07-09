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
            if (nums[j] < pivot){
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

### HeapSort

c++
```c++
class Solution {
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
