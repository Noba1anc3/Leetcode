Given a collection of intervals, find the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.



**Example:**
```
Input: [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of intervals are non-overlapping.

Input: [[1,2],[1,2],[1,2]]
Output: 2
Explanation: You need to remove two [1,2] to make the rest of intervals non-overlapping.

Input: [[1,2],[2,3]]
Output: 0
Explanation: You don't need to remove any of the intervals since they're already non-overlapping.
```

## Idea

活动选择问题

## Solution

```c++
class Solution {
public:
    static bool cmp(vector<int> a, vector<int> b){
        return a[1] < b[1];
    }

    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        int currentEnd = INT_MIN, erase = 0;
        sort(intervals.begin(), intervals.end(), cmp);
        
        for (const vector<int>& interval : intervals){
            if (interval[0] >= currentEnd)
                currentEnd = interval[1];
            else
                erase++;
        }
        
        return erase;
    }
};
```

执行用时：84 ms, 在所有 C++ 提交中击败了42.71%的用户

内存消耗：23.8 MB, 在所有 C++ 提交中击败了30.74%的用户
