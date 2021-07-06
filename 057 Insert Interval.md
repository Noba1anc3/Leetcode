Given a set of non-overlapping intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.



Examples:

```
Example 1:

Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]

Example 2:

Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].

Example 3:

Input: intervals = [], newInterval = [5,7]
Output: [[5,7]]

Example 4:

Input: intervals = [[1,5]], newInterval = [2,3]
Output: [[1,5]]
```

## Solution

c++

```c++
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> ans;
        
        for (const vector<int>& interval : intervals) {
            if (interval[1] < newInterval[0] || interval[0] > newInterval[1])
                ans.push_back(interval);
            else {
                // 与插入区间有交集，计算它们的并集
                newInterval[0] = min(newInterval[0], interval[0]);
                newInterval[1] = max(newInterval[1], interval[1]);
            }
        }
		
        ans.push_back(newInterval);
        sort(ans.begin(), ans.end());
        
        return ans;
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了97.09%的用户

内存消耗：16.6 MB, 在所有 C++ 提交中击败了72.56%的用户

Attention
- vector<vector<int>>.push_back({ele a, ele b})
