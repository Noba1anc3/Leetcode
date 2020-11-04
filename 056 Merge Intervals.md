Given a collection of intervals, merge all overlapping intervals.



Examples:

```
Example 1:

Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].

Example 2:

Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

## Idea

如果我们按照区间的左端点排序，那么在排完序的列表中，可以合并的区间一定是连续的。

![](https://pic.leetcode-cn.com/50417462969bd13230276c0847726c0909873d22135775ef4022e806475d763e-56-2.png)



## Solution

### c++

```c++
#include <algorithm>

class Solution {
public:

    // 可以不用这个比较函数
    static bool compare(vector<int> lista, vector<int> listb){
        if (lista[0] < listb[0]){
            return true;
        }
        else{
            return false;
        }
    }

    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        // 边界条件
        if (intervals.empty()){
            return intervals;
        }

        sort(intervals.begin(), intervals.end(), compare);

        vector<vector<int>> sorted;
        vector<int> cur_max_interval = intervals[0];

        for (int i = 1; i < intervals.size(); i++){
            vector<int> cur_interval = intervals[i];
            if (cur_interval[0] <= cur_max_interval[1]){
                cur_max_interval[1] = max(cur_interval[1], cur_max_interval[1]);
            }
            else{
                sorted.push_back(cur_max_interval);
                cur_max_interval = cur_interval;
            }
        }

        sorted.push_back(cur_max_interval);

        return sorted;
    }
};
```

执行用时：72 ms, 在所有 C++ 提交中击败了22.39%的用户  
内存消耗：15 MB, 在所有 C++ 提交中击败了13.92%的用户

### python

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key=lambda x: x[0])

        merged = []
        for interval in intervals:
            # 如果列表为空，或者当前区间与上一区间不重合，直接添加
            if not merged or merged[-1][1] < interval[0]:
                merged.append(interval)
            else:
                # 否则的话，我们就可以与上一区间进行合并
                merged[-1][1] = max(merged[-1][1], interval[1])

        return merged
```

时间复杂度：O(nlogn)，其中 n 为区间的数量。除去排序的开销，我们只需要一次线性扫描，所以主要的时间开销是排序的 O(nlogn)。

Attention:
- 边界条件的情况
- vector.begin(), vector.end()
- vector.empty()
- vector.push_back()
