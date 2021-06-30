Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.



**Example:**

```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.

Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9
```

## Idea

我们考虑枚举数组中的每个数 x，考虑以其为起点，不断尝试匹配 x+1, x+2, ⋯ 是否存在，假设最长匹配到了 x+y，那么以 x 为起点的最长连续序列即为 x, x+1, x+2, ⋯, x+y，其长度为 y+1，我们不断枚举并更新答案即可。

对于匹配的过程，暴力的方法是 O(n) 遍历数组去看是否存在这个数，但其实更高效的方法是用一个**哈希表**存储数组中的数，这样查看一个数是否存在即能优化至 **O(1)** 的时间复杂度。

仅仅是这样我们的算法时间复杂度最坏情况下还是会达到 **O(n^2)** 即外层需要枚举 **O(n)** 个数，内层需要暴力匹配 **O(n)** 次，无法满足题目的要求。但仔细分析这个过程，我们会发现其中执行了很多不必要的枚举，如果已知有一个 x, x+1, x+2, ⋯, x+y 的连续序列，而我们却重新从 x+1，x+2 或者是 x+y 处开始尝试匹配，那么得到的结果肯定不会优于枚举 x 为起点的答案，因此我们在外层循环的时候碰到这种情况跳过即可。

那么怎么判断是否跳过呢？由于我们要枚举的数 x 一定是在数组中不存在前驱数 x-1 的，不然按照上面的分析我们会从 x-1 开始尝试匹配，因此我们每次在哈希表中检查是否存在 x-1 即能判断是否需要跳过了。

## Solution - O(n)

c++

```c++
class Solution {
private:
    int maxLength = 0;
    unordered_set<int> nums_set;

public:
    int longestConsecutive(vector<int>& nums) {
        for (const int& num : nums)
            nums_set.insert(num);
        
        for (const int& num : nums_set){
            if (!nums_set.count(num-1)){
                int curNum = num;
                int curLength = 1;
                while (nums_set.count(++curNum))
                    curLength++;
                maxLength = max(maxLength, curLength);
            }
        }

        return maxLength;
    }
};
```

执行用时：72 ms, 在所有 C++ 提交中击败了47.62%的用户

内存消耗：30.2 MB, 在所有 C++ 提交中击败了12.35%的用户

Attention:
- ```
  unorder_set.count是O(1)的复杂度
  ```

python

对于以下在c++中数据结构替换实验带来的效果提升，python中有相同的结论。

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        nums = set(nums)
        length = 0
        
        for num in nums:
            if num - 1 not in nums:
                curNum = num + 1
                curLength = 1
                while curNum in nums:
                    curLength += 1
                    curNum += 1
                length = max(length, curLength)
        return length
```
 
执行用时：56 ms, 在所有 Python3 提交中击败了64.66%的用户

内存消耗：26.1 MB, 在所有 Python3 提交中击败了18.38%的用户

## Solution - Brute Force

时间复杂度过高，计算超时。

```c++
class Solution {
private:
    int maxLength = 0;

public:
    int longestConsecutive(vector<int>& nums) {
        for (const int& num : nums){
            int curNum = num;
            int curLength = 1;
            while (find(nums.begin(), nums.end(), ++curNum) != nums.end())
                curLength++;
            maxLength = max(maxLength, curLength);
        }

        return maxLength;
    }
};
```

## Solution - 减少无效计算

进一步地，如果数组中有前一个数字，则这次搜索一定是冗余计算。结果仍然超时。

```c++
class Solution {
private:
    int maxLength = 0;

public:
    int longestConsecutive(vector<int>& nums) {
        for (const int& num : nums){
            if (find(nums.begin(), nums.end(), num-1) == nums.end()){
                int curNum = num;
                int curLength = 1;
                while (find(nums.begin(), nums.end(), ++curNum) != nums.end())
                    curLength++;
                maxLength = max(maxLength, curLength);
            }
        }

        return maxLength;
    }
};
```

## Solution - 减小数组查找的时间复杂度

用set存储所有数字，查找的时间复杂度由O(n)下降到O(log n), 结果仍然超时。将set替换为查找复杂度为O(1)的unordered_set, 不再超时。

```c++
class Solution {
private:
    int maxLength = 0;
    unordered_set<int> nums_set;

public:
    int longestConsecutive(vector<int>& nums) {
        for (const int& num : nums)
            nums_set.insert(num);

        for (const int& num : nums){
            if (nums_set.find(num-1) == nums_set.end()){
                int curNum = num;
                int curLength = 1;
                while (nums_set.find(++curNum) != nums_set.end())
                    curLength++;
                maxLength = max(maxLength, curLength);
            }
        }

        return maxLength;
    }
};
```

执行用时：580 ms, 在所有 C++ 提交中击败了17.38%的用户

内存消耗：30.1 MB, 在所有 C++ 提交中击败了27.09%的用户

## Solution - 被遍历的vector替换为unordered_set

或许是由于其哈希表机制的原因，导致计算时间复杂度大大降低

```c++
class Solution {
private:
    int maxLength = 0;
    unordered_set<int> nums_set;

public:
    int longestConsecutive(vector<int>& nums) {
        for (const int& num : nums)
            nums_set.insert(num);
        
        for (const int& num : nums_set){
            if (nums_set.find(num-1) == nums_set.end()){
                int curNum = num;
                int curLength = 1;
                while (nums_set.find(++curNum) != nums_set.end())
                    curLength++;
                maxLength = max(maxLength, curLength);
            }
        }

        return maxLength;
    }
};
```

执行用时：68 ms, 在所有 C++ 提交中击败了48.59%的用户

内存消耗：30.1 MB, 在所有 C++ 提交中击败了29.26%的用户
