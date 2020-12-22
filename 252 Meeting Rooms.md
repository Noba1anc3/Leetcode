Given an array of meeting time `intervals` where `intervals[i] = [starti, endi]`, determine if a person could attend all meetings.



**Example:**
```
Input: intervals = [[0,30],[5,10],[15,20]]
Output: false

Input: intervals = [[7,10],[2,4]]
Output: true
```

## Solution

```c++
class Solution {
public:
    bool canAttendMeetings(vector<vector<int>>& intervals) {
        if (intervals.empty())
            return true;

        sort(intervals.begin(), intervals.end());

        for (int i = 1; i < intervals.size(); i++)
            if (intervals[i][0] < intervals[i-1][1])
                return false;
        return true;
    }
};
```

执行用时：48 ms, 在所有 C++ 提交中击败了50.00%的用户

内存消耗：11.8 MB, 在所有 C++ 提交中击败了50.00%的用户