Given an array of meeting time `intervals` where `intervals[i] = [starti, endi]`, determine if a person could attend all meetings.



**Example:**
```
Input: intervals = [[0,30],[5,10],[15,20]]
Output: false

Input: intervals = [[7,10],[2,4]]
Output: true
```

## Idea

活动选择问题

## Solution

```c++
class Solution {
public:
    bool canAttendMeetings(vector<vector<int>>& intervals) {
        if (intervals.empty())
            return true;

        sort(intervals.begin(), intervals.end(), [](const vector<int> intA, const vector<int> intB){
            return intA[1] < intB[1];
        });

        int endTime = intervals[0][1];

        for (int i = 1; i < intervals.size(); i++){
            if (intervals[i][0] < endTime)
                return false;
            endTime = intervals[i][1];
        }

        return true;
    }
};
```

执行用时：196 ms, 在所有 C++ 提交中击败了5.48%的用户

内存消耗：27.2 MB, 在所有 C++ 提交中击败了5.17%的用户