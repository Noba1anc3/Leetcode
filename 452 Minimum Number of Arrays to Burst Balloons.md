There are some spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter, and hence the x-coordinates of start and end of the diameter suffice. The start is always smaller than the end.

An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with `xstart` and `xend` bursts by an arrow shot at x if `xstart` ≤ x ≤ `xend`. There is no limit to the number of arrows that can be shot. An arrow once shot keeps traveling up infinitely.

Given an array `points` where `points[i] = [xstart, xend]`, return the minimum number of arrows that must be shot to burst all balloons.



**Example:**
```
Input: points = [[10,16],[2,8],[1,6],[7,12]]
Output: 2
Explanation: One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons).

Input: points = [[1,2],[3,4],[5,6],[7,8]]
Output: 4

Input: points = [[1,2],[2,3],[3,4],[4,5]]
Output: 2

Input: points = [[1,2]]
Output: 1

Input: points = [[2,3],[2,3]]
Output: 1
```

## Idea

活动选择问题

**如果按左端升序排序**，可能出现这种：[0, 9], [0, 6], [7, 8]

- 当前第一个区间和第二个重合，继续寻求重合，它和第三个也重合。
- 你想着一箭三雕，但第二个和第三个并不重合。
- 出现了「包含型重合」，被包含在当前区间的重合区间，不一定和别的重合区间重合
- 当前区间可能和很多人重合，但无法保证内部都互相重合。

**如果按右端升序排序**，就杜绝了「前面包后面」的情况。

```
当前区间  -----R
  区间a ---------R
      区间b  L-------
```

如上图，当前区间遇到重合区间 a，a 的右端>=自己的右端（因为右端递增），遇到下一个重合区间 b，b 的左端<=自己的右端（因为重合），所以 a 、b 一定有交集，参照的是**当前区间的右端**。

于是，放心地让当前区间一路找重合，直到遇到不重合，就有了一组能一箭穿的。

1. 拿当前区间的右端作为标杆。
2. 只要 下一个区间的左端 <= 标杆，则重合，继续寻求与下一个区间重合。
3. 直到遇到不重合的区间，弓箭数 +1。
4. 拿新区间的右端作为标杆，重复以上步骤。

## Solution

```c++
class Solution {
public:
    static bool cmp(vector<int>& pA, vector<int>& pB){
        return pA[1] < pB[1];
    }

    int findMinArrowShots(vector<vector<int>>& points) {
        if (points.empty()) return 0;

        sort(points.begin(), points.end(), cmp);
        int arrows = 1, curEnd = points[0][1];

        for (const vector<int>& point : points)
            if (point[0] > curEnd){
                curEnd = point[1];
                arrows++;
            }

        return arrows;
    }
};
```

执行用时：120 ms, 在所有 C++ 提交中击败了99.85%的用户

内存消耗：34.1 MB, 在所有 C++ 提交中击败了41.06%的用户
