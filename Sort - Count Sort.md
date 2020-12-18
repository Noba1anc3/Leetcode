仅限非负数组

```c++
vector<int> sortArray(vector<int>& nums) {
    int K = nums[0], n = nums.size();
    for (auto num : nums)
        K = max(K, num);

    vector<int> sorted(n, 0), C(K+1, 0);

    for (int i = 0; i < n; i++)
        C[nums[i]] += 1;

    for (int i = 1; i <= K; i++)
        C[i] += C[i-1];

    for (int i = n-1; i >= 0; i--){
        C[nums[i]] -= 1;
        sorted[C[nums[i]]] = nums[i];
    }

    return sorted;
}
```