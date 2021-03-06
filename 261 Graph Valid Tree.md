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

- 首先判断边的个数加1是否等于节点的个数
  - 少了一定无法存在孤立点
  - 多了一定存在环路
- 从0节点遍历一次DFS后仍然有白色节点，意味着有环路+孤立节点 返回false

### DFS

```c++
class Solution {
public:
    int WHITE = 0, GRAY = 1, BLACK = 2;

    void DFSVisit(vector<vector<int>> &G, int vertex, vector<int> &color){
        
        color[vertex] = GRAY;

        for (const int& adjVertex : G[vertex])
            if (color[adjVertex] == WHITE)
                DFSVisit(G, adjVertex, color);

        color[vertex] = BLACK;
    }

    bool validTree(int n, vector<vector<int>>& edges) {
        if(edges.size() + 1 != n) 
            return false;
            
        int root = 0;
        vector<int> color(n, WHITE);
        vector<vector<int>> Graph(n, vector<int>());

        for (const vector<int>& edge : edges){
            Graph[edge[0]].push_back(edge[1]);
            Graph[edge[1]].push_back(edge[0]);
        }

        DFSVisit(Graph, root, color);
        
        for (const int& c : color)
            if (c == WHITE)
                return false;
        return true;
    }
};
```

执行用时：20 ms, 在所有 C++ 提交中击败了74.74%的用户

内存消耗：12.3 MB, 在所有 C++ 提交中击败了24.91%的用户

### BFS

```c++
class Solution {
public:
    int WHITE = 0, GRAY = 1, BLACK = 2;

    bool BFSVisit(vector<vector<int>>& G, int vertex, vector<int>& color){
        queue<int> Q;
        
        Q.push(vertex);
        color[vertex] = GRAY;

        while (!Q.empty()){
            int curVertex = Q.front();
            Q.pop();
            for (const int& adjVertex : G[curVertex]){
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

        for (const vector<int>& edge : edges){
            Graph[edge[0]].push_back(edge[1]);
            Graph[edge[1]].push_back(edge[0]);
        }

        BFSVisit(Graph, 0, color);
            
        for (const int& ele : color)
            if (ele == WHITE)
                return false;
        return true;
    }
};
```

执行用时：20 ms, 在所有 C++ 提交中击败了74.74%的用户

内存消耗：11.9 MB, 在所有 C++ 提交中击败了50.30%的用户

### Union-Find Set

```c++
class Solution {
private:
    vector<int> parent, height;

public:
    int findSet(int x){
        while (parent[x] != x)
            x = parent[x];
        return x;
    }

    bool Union(int x, int y){
        int ROOT1 = findSet(x);
        int ROOT2 = findSet(y);

        if (ROOT1 == ROOT2) return false;

        if (height[ROOT1] <= height[ROOT2]) {
            if (height[ROOT1] == height[ROOT2])
                height[ROOT2]++;
            parent[ROOT1] = ROOT2;
        }
        else{
            parent[ROOT2] = ROOT1;
        }

        return true;
    }

    bool validTree(int n, vector<vector<int>>& edges) {
        if (edges.size() + 1 != n)
            return false;

        height.resize(n);
        parent.resize(n);

        for (int i = 0; i < n; i++)
            parent[i] = i;  

        for (const vector<int>& edge : edges)
            if (!Union(edge[0], edge[1]))
                return false;

        return true;
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了98.64%的用户

内存消耗：11.7 MB, 在所有 C++ 提交中击败了75.25%的用户
