There are a total of `n` courses you have to take labelled from `0` to `n - 1`.

Some courses may have `prerequisites`, for example, if `prerequisites[i] = [ai, bi]` this means you must take the course `bi` before the course `ai`.

Given the total number of courses `numCourses` and a list of the `prerequisite` pairs, return the ordering of courses you should take to finish all courses.

If there are many valid answers, return **any** of them. If it is impossible to finish all courses, return **an empty array**.



**Example:**
```
Example 1:

Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].

Example 2:

Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].

Example 3:

Input: numCourses = 1, prerequisites = []
Output: [0]
```

## Idea

Topological Sort

## Solution

### c++

```c++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> Graph(numCourses, vector<int>());
        vector<int> InDegree(numCourses, 0);
        vector<int> Topological;
        queue<int> Q;

        for (auto prerequisite : prerequisites){
            Graph[prerequisite[1]].push_back(prerequisite[0]);
            InDegree[prerequisite[0]]++;
        }

        for (int i = 0; i < numCourses; i++)
            if (InDegree[i] == 0) Q.push(i);

        while (!Q.empty()){
            int vertex = Q.front();
            Q.pop();
            Topological.push_back(vertex);
            for (auto node : Graph[vertex]){
                InDegree[node]--;
                if (InDegree[node] == 0) Q.push(node);
            }
        }

        if (Topological.size() == numCourses)
            return Topological;
        return {};
    }
};
```
执行用时：56 ms, 在所有 C++ 提交中击败了36.87%的用户

内存消耗：14.2 MB, 在所有 C++ 提交中击败了42.33%的用户

