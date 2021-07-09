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

执行用时：632 ms, 在所有 C++ 提交中击败了5.01%的用户

内存消耗：222 MB, 在所有 C++ 提交中击败了5.04%的用户
