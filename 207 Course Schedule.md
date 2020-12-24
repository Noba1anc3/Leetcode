There are a total of `n` courses you have to take labelled from `0` to `n - 1`.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?



**Example:**
```
Example 1:

Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0. So it is possible.
             
Example 2:

Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible.
```

## Idea

Topological Sort

## Solution

### c++

```c++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
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
            return true;
        return false;
    }
};
```
执行用时：80 ms, 在所有 C++ 提交中击败了13.99%的用户

内存消耗：12.8 MB, 在所有 C++ 提交中击败了24.68%的用户

