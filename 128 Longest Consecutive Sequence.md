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


```c++
class Solution {
private:
    int maxLength = 0;
    unordered_set<int> hashmap;

public:
    int longestConsecutive(vector<int>& nums) {
        for (int& num : nums)
            hashmap.insert(num);
        
        for (const int& num : hashmap){
            if (!hashmap.count(num-1)){
                int curNum = num;
                int curLength = 1;
                while (hashmap.count(++curNum))
                    curLength++;
                maxLength = max(maxLength, curLength);
            }
        }

        return maxLength;
    }
};
```

执行用时：16 ms, 在所有 C++ 提交中击败了94.14%的用户

内存消耗：10.9 MB, 在所有 C++ 提交中击败了39.86%的用户

Attention:

- 遍历列表时，使用&可以变快

- ```
  unorder_set.count是O(1)的复杂度
  ```

## Solution - O(n3)

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

## Solution - O(n2)

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

执行用时：1604 ms, 在所有 C++ 提交中击败了5.02%的用户

内存消耗：10 MB, 在所有 C++ 提交中击败了94.21%的用户
