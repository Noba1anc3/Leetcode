## Python List
- num2 in nums，返回 True 说明有戏
- nums.index(num2)，查找 num2 的索引

### 方法一 
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            if (target - nums[i]) in nums:
                if nums.count(target - nums[i]) == 1 and 2 * nums[i] == target:
                    continue
                else:
                    j = nums.index(target - nums[i], i+1) #index(x,i+1)是从num1后的序列后找num2
                    break

        if j > 0 :
            return [i, j]
        else:
            return []
```
Time : 1140ms

### 方法二
优化: 对j的寻找在nums[i]的后面进行即可,不必在nums整个数组中进行

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            if (target - nums[i]) in nums[i+1:]:
                if nums.count(target - nums[i]) == 1 and 2 * nums[i] == target:
                    continue
                else:
                    j = nums.index(target - nums[i], i+1) #index(x,i+1)是从num1后的序列后找num2
                    break

        if j > 0 :
            return [i, j]
        else:
            return []
```
Time : 856 ms

## Hashmap

### python

使用字典模拟哈希表

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
Time : 44ms

### c++

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> dict;
        vector<int> ans;

        for (int i = 0; i < nums.size(); i++)
            dict[nums[i]] = i;

        for (int i = 0; i < nums.size(); i++){
            unordered_map<int,int>::iterator it;
            it = dict.find(target - nums[i]);
            if (it != dict.end() && i != it->second){
                ans.push_back(i);
                ans.push_back(it->second);
                break;
            }
        }

        return ans;
    }
};
```

执行用时：8 ms, 在所有 C++ 提交中击败了90.24%的用户

内存消耗：9.9 MB, 在所有 C++ 提交中击败了24.04%的用户

Attention:

- unordered_map<int, int> map
- map[key] = value
- unordered_map<int, int>::iterator it = map.find(key)
- it != map.end()
- it->second