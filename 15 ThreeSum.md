Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.

Notice that the solution set must not contain duplicate triplets.

Example:
```
Example 1:

Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]

Example 2:

Input: nums = []
Output: []

Example 3:

Input: nums = [0]
Output: []
```

## Idea - Double Pointer

To be continued

## Solution

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());

        for (int i = 0; i < nums.size(); i++){
            if (nums[i] > 0) return result;
            if (i > 0 && nums[i] == nums[i-1]) continue;
            int l = i+1;
            int r = nums.size() - 1;
            while (l < r){
                int sum = nums[i] + nums[l] + nums[r];
                if (sum == 0){
                    result.push_back({nums[i], nums[l], nums[r]});
                    while (l < r && nums[l+1] == nums[l])
                        l++;
                    while (r > l && nums[r-1] == nums[r])
                        r--;
                    l++; r--;
                }
                else if (sum > 0) r--;
                else l++;
            }
        }
        return result;
    }
};
```

执行用时：56 ms, 在所有 C++ 提交中击败了97.97%的用户

内存消耗：19.5 MB, 在所有 C++ 提交中击败了60.08%的用户

面试时读取输入进行输出的完整代码

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());

        for (int i = 0; i < nums.size(); i++){
            if (nums[i] > 0) return result;
            if (i > 0 && nums[i] == nums[i-1]) continue;
            int l = i+1;
            int r = nums.size() - 1;
            while (l < r){
                int sum = nums[i] + nums[l] + nums[r];
                if (sum == 0){
                    result.push_back({nums[i], nums[l], nums[r]});
                    while (l < r && nums[l+1] == nums[l])
                        l++;
                    while (r > l && nums[r-1] == nums[r])
                        r--;
                    l++; r--;
                }
                else if (sum > 0) r--;
                else l++;
            }
        }
        return result;
    }
};

int main(){
    Solution slt = Solution();
    vector<int> nums;
    vector<vector<int>> rsps;
    int a;
    while (cin>>a) nums.push_back(a);
    rsps = slt.threeSum(nums);

    for (const vector<int>& rsp : rsps){
        for (const int& num : rsp)
            cout<<num<<" ";
        cout<<"\n";
    }
}

```
