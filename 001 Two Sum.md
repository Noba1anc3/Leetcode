Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

```
Example 1:

Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].

Example 2:

Input: nums = [3,2,4], target = 6
Output: [1,2]

Example 3:

Input: nums = [3,3], target = 6
Output: [0,1]
```

## Brute Force

### 原始方法
python
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        rsp = []

        for i in range(len(nums)):
            if (target - nums[i]) in nums:
                if nums.count(target - nums[i]) == 1 and nums[i] * 2 == target:
                    continue
                else:
                    j = nums.index(target - nums[i], i+1) #index(x,i+1)是从num1后的序列后找num2
                    rsp = [i, j]
                    break

        return rsp
```

执行用时：708 ms, 在所有 Python3 提交中击败了17.37%的用户

内存消耗：15.1 MB, 在所有 Python3 提交中击败了34.48%的用户

### 初步优化

优化: 对j的寻找在nums[i]的后面进行即可,不必在nums整个数组中进行

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        rsp = []
        for i in range(len(nums)):
            if (target - nums[i]) in nums[i+1:]:
                j = nums.index(target - nums[i], i+1)
                rsp = [i, j]
                break
        return rsp
```

执行用时：484 ms, 在所有 Python3 提交中击败了19.57%的用户

内存消耗：15 MB, 在所有 Python3 提交中击败了48.40%的用户

Attention
- list.index(element, index)，查找 index下标之后的元素中 element 的索引
- list.count(element), 查找 list 中有几个 element

## Hashmap

python

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashmap = {}
        for i, num in enumerate(nums):
            hashmap[num] = i
        for i, num in enumerate(nums):
            j = hashmap.get(target - num)
            if j is not None and not i == j:
                return [i, j]
```

执行用时：56 ms, 在所有 Python3 提交中击败了26.22%的用户

内存消耗：15.9 MB, 在所有 Python3 提交中击败了5.17%的用户

c++

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> M;
        for (int i = 0; i < nums.size(); i++)
            M[nums[i]] = i;

        for (int i = 0; i < nums.size(); i++) {
            int another = target - nums[i];
            if (M.find(another) != M.end() && M[another] != i)
                return {i, M[another]};
        }

        return {};
    }
};
```

执行用时：16 ms, 在所有 C++ 提交中击败了34.58%的用户

内存消耗：11.6 MB, 在所有 C++ 提交中击败了5.00%的用户

Attention:
- unordered_map<int, int> map
- map[key] = value
- unordered_map<int, int>::iterator it = map.find(key)
- it != map.end()
- it->second
