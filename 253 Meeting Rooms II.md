Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...]` (si < ei), find the minimum number of conference rooms required.



**Example:**
```
Input: [[0, 30],[5, 10],[15, 20]]
Output: 2

Input: [[7,10],[2,4]]
Output: 1
```

## Idea

从一组想要开会但还没有分配会议室的人员的角度来看这个问题。他们会怎么做？

> 这组人会依次查看会议室，检查是否有任何会议室是空闲的。如果他们找到一间空闲的会议室，就会在那个房间里开始开会。否则,他们会等待一个会议室空出来。一旦会议室空出来,他们就会占用它。

我们无法按任意顺序处理给定的会议。处理会议的最基本方式是按其 `开始时间` 顺序排序

我们可以将所有房间保存在最小堆中,堆中的键值是会议的结束时间。这样，每当我们想要检查有没有任何房间是空的，只需要检查最小堆堆顶的元素，它是最先开完会腾出房间的。如果堆顶的元素的房间并不空闲，那么其他所有房间都不空闲。这样，我们就可以直接开一个新房间。

算法

1. 按照 开始时间 对会议进行排序。
2. 初始化最小堆，只需要记录会议的结束时间，告诉我们什么时候房间会空。
3. 对每个会议，检查堆的最小元素（即堆顶部的房间）是否空闲。
4. 若房间空闲，则从堆顶拿出该元素，将其改为我们处理的会议的结束时间，加回到堆中。
5. 若房间不空闲。开新房间，并加入到堆中。
6. 处理完所有会议后，堆的大小即为开的房间数量。这就是容纳这些会议需要的最小房间数。

## Solution

### C++

```c++
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        std::priority_queue<int, vector<int>, greater<int>> Q;

        for (const vector<int>& interval : intervals){
            if (!Q.empty() && interval[0] >= Q.top())
                Q.pop();
            Q.push(interval[1]);
        }

        return Q.size();
    }
};
```

执行用时：24 ms, 在所有 C++ 提交中击败了59.57%的用户

内存消耗：11.3 MB, 在所有 C++ 提交中击败了68.34%的用户

### python

```python
class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        if not intervals:
            return 0

        # Sort the meetings in increasing order of their start time.
        intervals.sort(key = lambda x : x[0])
        
        # The heap initialization
        free_rooms = []
        heapq.heappush(free_rooms, intervals[0][1])

        for i in intervals[1:]:
            if free_rooms[0] <= i[0]:
                heapq.heappop(free_rooms)
            heapq.heappush(free_rooms, i[1])

        return len(free_rooms)
```

执行用时：52 ms, 在所有 Python3 提交中击败了63.84%的用户

内存消耗：16.3 MB, 在所有 Python3 提交中击败了26.24%的用户

Attention

- `list.sort(key = lambda x : x[0])`
- `heapq.heappush(list, item)`
- `heapq.heappop(list)`

### Java

```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        int length = intervals.length;
        
        if (length == 0)
            return 0;

        Comparator<Integer> HeapComparator = new Comparator<Integer>() {
            public int compare(Integer a, Integer b) {
                return a - b;
            }
        };

        Comparator<int[]> ArrayComparator = new Comparator<int[]>() {
            public int compare(int[] a, int[] b) {
                return a[0] - b[0];
            }
        };

        Arrays.sort(intervals, ArrayComparator);
        
        PriorityQueue<Integer> allocator = new PriorityQueue<Integer>(length, HeapComparator);
        allocator.add(intervals[0][1]);

        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] >= allocator.peek()) 
                allocator.poll();
            allocator.add(intervals[i][1]);
        }

        return allocator.size();
    }
}
```

执行用时：8 ms, 在所有 Java 提交中击败了77.09%的用户

内存消耗：37.9 MB, 在所有 Java 提交中击败了100.00%的用户

Attention

- ```java
  PriorityQueue<Integer>.add()
  ```

- ```java
  PriorityQueue<Integer>.peek()
  ```

- ```java
  PriorityQueue<Integer>.poll()
  ```

- ```java
  Comparator<Integer> HeapComparator = new Comparator<Integer>() {
      public int compare(Integer a, Integer b) {
      	return a - b;
      }
  };
  ```

- ```java
  Arrays.sort(int[][] array, Comparator<int[]>);
  Arrays.sort(int[] array, Comparator<Integer>);
  ```

  
