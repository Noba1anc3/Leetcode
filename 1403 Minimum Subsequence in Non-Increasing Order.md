Given the array `nums`, obtain a **subsequence** of the array whose sum of elements is **strictly** greater than the sum of the non included elements in such subsequence. 

If there are multiple solutions, return the subsequence with **minimum size** and if there still exist multiple solutions, return the subsequence with **the maximum total sum** of all its elements. A subsequence of an array can be obtained by erasing some (possibly zero) elements from the array. 

Note that the solution with the given constraints is guaranteed to be **unique**. Also return the answer sorted in **non-increasing** order.



Examples:

```
Example 1:

Input: nums = [4,3,10,9,8]
Output: [10,9] 
Explanation: The subsequences [10,9] and [10,8] are minimal such that the sum of their elements is strictly greater than the sum of elements not included, however, the subsequence [10,9] has the maximum total sum of its elements. 

Example 2:

Input: nums = [4,4,7,6,7]
Output: [7,7,6] 
Explanation: The subsequence [7,7] has the sum of its elements equal to 14 which is not strictly greater than the sum of elements not included (14 = 4 + 4 + 6). Therefore, the subsequence [7,6,7] is the minimal satisfying the conditions. Note the subsequence has to returned in non-decreasing order.  

Example 3:

Input: nums = [6]
Output: [6]
```

## Solution

### python

```python
class Solution:
    def minSubsequence(self, nums: List[int]) -> List[int]:
        nums.sort(reverse=True)
        topArray = []
        tmp, Sum = 0, sum(nums)
        for i in nums:
            tmp += i
            topArray.append(i)
            if 2*tmp > Sum:  ## tmp > Sum - tmp
                return topArray
```

执行用时：60 ms, 在所有 Python3 提交中击败了41.24%的用户  
内存消耗：13.5 MB, 在所有 Python3 提交中击败了20.26%的用户

Attention:
- array.sort(reverse=True)  降序, 默认升序
- sum(array)

### c++

#### 基本方法

```c++
class Solution {
public:
    static bool cmp (int a, int b){
        return a > b;
    }

    vector<int> minSubsequence(vector<int>& nums) {
        int sum = 0, sequence_sum = 0;
        vector<int> sequence;

        for (const int& num : nums)
            sum += num;

        sort(nums.begin(), nums.end(), cmp);

        for (const int& num : nums){
            sequence_sum += num;
            sequence.push_back(num);
            if (2*sequence_sum > sum) return sequence;
        }

        return {};
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了40.19%的用户

内存消耗：10.4 MB, 在所有 C++ 提交中击败了39.36%的用户

#### 指针替代整型

```c++
class Solution {
public:
    vector<int> minSubsequence(vector<int>& nums) {
        int sum = 0, sequence_sum = 0;
        vector<int> sequence;
        vector<int>::iterator it = nums.begin();
        sort(nums.begin(), nums.end());

        while (it != nums.end())
            sum += *it++;

        it--;

        while (it != nums.begin() - 1){
            sequence_sum += *it;
            sequence.push_back(*it--);
            if (2*sequence_sum > sum) return sequence;
        }

        return {};
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了97.93%的用户

内存消耗：10.2 MB, 在所有 C++ 提交中击败了94.22%的用户
