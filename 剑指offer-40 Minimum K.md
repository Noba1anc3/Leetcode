输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

 **Example:**

```
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]

输入：arr = [0,1,2,1], k = 1
输出：[0]
```

## Solution
### Brute Force

c++

```c++
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        sort(arr.begin(), arr.end());
        vector<int> leastK(arr.begin(), arr.begin()+k);

        return leastK;
    }
};
```

执行用时：76 ms, 在所有 C++ 提交中击败了69.22%的用户

内存消耗：18.8 MB, 在所有 C++ 提交中击败了65.86%的用户

复杂度分析

- 时间复杂度：O(n log n)，其中 n 是数组 `arr` 的长度。算法的时间复杂度即排序的时间复杂度。

- 空间复杂度：O(log n)，排序所需额外的空间复杂度为 O(log n)。

### Max Heap  a.k.a  Priority Queue

c++

```c++
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        std::vector<int> rsp;
        std::priority_queue<int> Q;

        if (k == 0) return rsp;
        
        for (int i = 0; i < k; i++)
            Q.push(arr[i]);
        
        for (int i = k; i < arr.size(); i++)
            if (arr[i] < Q.top()){
                Q.pop();
                Q.push(arr[i]);
            }
        
        while(!Q.empty()){
            rsp.push_back(Q.top());
            Q.pop();
        }

        return rsp;
    }
};
```

执行用时：40 ms, 在所有 C++ 提交中击败了39.83%的用户

内存消耗：19.5 MB, 在所有 C++ 提交中击败了23.26%的用户

**复杂度分析**

- 时间复杂度：O(n log k)，其中 n 是数组 `arr` 的长度。由于最大堆实时维护前 k 小值，所以插入删除都是 O(log k) 的时间复杂度，最坏情况下数组里 n 个数都会插入，所以一共需要 O(n log k) 的时间复杂度。

- 空间复杂度：O(k)，因为大根堆里最多 k 个数。

**Attention**

- vector<int> leastK(k, 0)
- priority_queue<int> Q
- Q.push()
- Q.pop()
- Q.top()

### Quicksort

c++

```c++
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        QuickSort(arr, 0, arr.size() - 1, k);
        vector<int> leastK(arr.begin(), arr.begin()+k);

        return leastK;
    }

    void QuickSort(vector<int>& nums, int p, int q, int k){
        if (p < q){
            int r = Partition(nums, p, q);
            int num = r - p + 1;
            if (num == k)
                return;
            else if (num > k)
                QuickSort(nums, p, r-1, k);
            else
                QuickSort(nums, r+1, q, k-num);
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
    
执行用时：28 ms, 在所有 C++ 提交中击败了87.68%的用户
    
内存消耗：18.3 MB, 在所有 C++ 提交中击败了80.96%的用户

**复杂度分析**

- 时间复杂度
  - 期望为 O(n)
  - 最坏情况下的时间复杂度为 O(n^2)
- 空间复杂度
  - 期望为 O(log n)
  - 最坏情况下的空间复杂度为 O(n)

**Attention**

- 快排的退出条件不能忘记
- &传地址
- swap
- vector<int> leastK(arr.begin(), arr.begin()+k);

