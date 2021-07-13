仅限非负数组

```c++
vector<int> Count_Sort(vector<int>& nums) {
    int K = INT_MIN, n = nums.size();
    vector<int> sorted_nums(n, 0), C(K+1, 0);
    
    for (const int& num : nums)
        K = max(K, num);
        
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
```
