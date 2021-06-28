Given `n` nodes labeled from `0` to `n-1` and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

 

**Example:**

```
Example 1:

Input: n = 5, and edges = [[0,1], [0,2], [0,3], [1,4]]
Output: true

Example 2:

Input: n = 5, and edges = [[0,1], [1,2], [2,3], [1,3], [1,4]]
Output: false
```

## Solution

### DFS

```c++
class Solution {
public:
    int WHITE = 0, GRAY = 1, BLACK = 2;

    void DFSVisit(vector<vector<int>> &G, int vertex, vector<int> &color){
        
        color[vertex] = GRAY;

        for (auto adjVertex : G[vertex]){
            if (color[adjVertex] == WHITE){
                DFSVisit(G, adjVertex, color);
            }
        }

        color[vertex] = BLACK;
    }

    bool validTree(int n, vector<vector<int>>& edges) {
        if(edges.size() + 1 != n) 
            return false;
            
        int root = 0;
        vector<int> color(n, WHITE);
        vector<vector<int>> Graph(n, vector<int>());

        for (auto edge : edges){
            Graph[edge[0]].push_back(edge[1]);
            Graph[edge[1]].push_back(edge[0]);
        }

        DFSVisit(Graph, root, color);
        
        for (auto ele : color)
            if (ele != BLACK)
                return false;
        return true;
    }
};
```

执行用时：28 ms, 在所有 C++ 提交中击败了75.97%的用户

内存消耗：11.2 MB, 在所有 C++ 提交中击败了47.30%的用户

### BFS

```c++
class Solution {
public:
    int WHITE = 0, GRAY = 1, BLACK = 2;

    bool BFSVisit(vector<vector<int>> &G, int vertex, vector<int> &color){
        color[vertex] = GRAY;
        
        queue<int> Q;
        Q.push(vertex);
        
        while (!Q.empty()){
            int curVertex = Q.front();
            Q.pop();
            
            for (auto adjVertex : G[curVertex]){
                if (color[adjVertex] == WHITE){
                    color[adjVertex] = GRAY;
                    Q.push(adjVertex);
                }
            }
            
            color[curVertex] = BLACK;
        }
        
        return true;
    }

    bool validTree(int n, vector<vector<int>>& edges) {
        if (edges.size() + 1 != n)
            return false;

        vector<int> color(n, WHITE);    
        vector<vector<int>> Graph(n, vector<int>());

        for (auto edge : edges){
            Graph[edge[0]].push_back(edge[1]);
            Graph[edge[1]].push_back(edge[0]);
        }

        BFSVisit(Graph, 0, color);
            
        for (auto ele : color)
            if (ele != BLACK)
                return false;
        return true;
    }
};
```

执行用时：28 ms, 在所有 C++ 提交中击败了75.97%的用户

内存消耗：11.2 MB, 在所有 C++ 提交中击败了47.30%的用户
