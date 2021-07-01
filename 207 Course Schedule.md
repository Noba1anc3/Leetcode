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

### Topological Sort

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
            if (InDegree[i] == 0) 
                Q.push(i);

        while (!Q.empty()){
            int vertex = Q.front(); Q.pop();
            Topological.push_back(vertex);
            for (const int& node : Graph[vertex]){
                InDegree[node]--;
                if (InDegree[node] == 0)
                    Q.push(node);
            }
        }
        return Topological.size() == numCourses;
    }
};
```

执行用时：28 ms, 在所有 C++ 提交中击败了53.83%的用户

内存消耗：13.6 MB, 在所有 C++ 提交中击败了34.07%的用户

### DFS Acyclic Judge

```c++
class Solution {
private:
    int WHITE = 0, GRAY = 1, BLACK = 2;
    bool acyclic = true;

    std::vector<int> color;
    std::vector<std::vector<int>> Graph;

public:
    void DFSVisit(int vertex){
        color[vertex] = GRAY;
        for (const int& adjVertex : Graph[vertex]){
            if (color[adjVertex] == WHITE){
                DFSVisit(adjVertex);
                if (!acyclic)
                    return;
            }
            else if (color[adjVertex] == GRAY){
                acyclic = false;
                return;
            }
        }
        color[vertex] = BLACK;
    }

    bool canFinish(int numCourses, std::vector<std::vector<int>>& prerequisites) {
        color.resize(numCourses);
        Graph.resize(numCourses);

        for (const std::vector<int>& edge : prerequisites)
            Graph[edge[0]].push_back(edge[1]);

        for (int root = 0; root < numCourses && acyclic; root++)
            if (color[root] == WHITE)
                DFSVisit(root);

        return acyclic;
    }
};
```

执行用时：20 ms, 在所有 C++ 提交中击败了93.82%的用户

内存消耗：13.5 MB, 在所有 C++ 提交中击败了40.26%的用户
