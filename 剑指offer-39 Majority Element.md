Given an array `nums` of size n, return the majority element.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.



 **Example:**

```
Example 1:

Input: nums = [3,2,3]
Output: 3
Example 2:

Input: nums = [2,2,1,1,1,2,2]
Output: 2
```

## Solution [Sort] - O(n log n) 

### python

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        return sorted(nums)[int(len(nums)/2)]
```

执行用时：60 ms, 在所有 Python3 提交中击败了33.62%的用户

内存消耗：15.9 MB, 在所有 Python3 提交中击败了85.84%的用户

## Solution [Hash] - O(n)

### c++

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int target = nums.size() % 2 == 1 ? nums.size() / 2 + 1 : nums.size() / 2;

        unordered_map<int, int> M;
        for (const int& num : nums)
            if (++M[num] == target)
                return num;

        return 0;
    }
};
```

执行用时：20 ms, 在所有 C++ 提交中击败了71.42%的用户

内存消耗：18.4 MB, 在所有 C++ 提交中击败了9.75%的用户

### python

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        if len(nums) % 2 == 1:
            target = int(len(nums) / 2) + 1
        else:
            target = len(nums) / 2

        M = {}
        for num in nums:
            M[num] = 0

        for num in nums:
            M[num] += 1
            if M[num] == target:
                return num
```

执行用时：60 ms, 在所有 Python3 提交中击败了33.62%的用户

内存消耗：16.2 MB, 在所有 Python3 提交中击败了9.48%的用户

## Solution [Vote] - O(n)

### c++

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int dom_num = 0, votes = 0;
        for (const int& num : nums){
            if(votes == 0) dom_num = num;
            votes += num == dom_num ? 1 : -1;
        }
        return dom_num;
    }
};
```

执行用时：16 ms, 在所有 C++ 提交中击败了90.26%的用户

内存消耗：18.4 MB, 在所有 C++ 提交中击败了25.69%的用户

### python

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        dom = vote = 0
        
        for num in nums:
            if vote == 0: dom = num
            if dom == num: vote += 1
            else: vote -= 1
        
        return dom
```

执行用时：44 ms, 在所有 Python3 提交中击败了89.33%的用户

内存消耗：16 MB, 在所有 Python3 提交中击败了75.51%的用户
