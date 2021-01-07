Given `n` nodes labeled from `0` to `n - 1` and a list of undirected edges (each edge is a pair of nodes), write a function to find the number of connected components in an undirected graph.



Examples:

```
Example 1:

Input: n = 5 and edges = [[0, 1], [1, 2], [3, 4]]

     0          3
     |          |
     1 --- 2    4 

Output: 2

Example 2:

Input: n = 5 and edges = [[0, 1], [1, 2], [2, 3], [3, 4]]

     0           4
     |           |
     1 --- 2 --- 3

Output:  1
```

Each element in the result should appear as many times as it shows in both arrays.
The result can be in any order.

## Solution - I SCC

c++

```c++
class Solution {
private:
    int WHITE = 0, GRAY = 1, BLACK = 2;

public:
    vector<vector<int>> GraphConstruction(int n, vector<vector<int>>& edges){
        vector<vector<int>> Graph(n+1, vector<int>());

        for(vector<int>& edge : edges){
            Graph[edge[0]+1].push_back(edge[1]+1);
            Graph[edge[1]+1].push_back(edge[0]+1);
        }

        return Graph;
    }

    vector<vector<int>> ReverseGraph(vector<vector<int>>& Graph){
        int n = Graph.size();
        vector<vector<int>> GraphR(n, vector<int>());

        for(int src = 1; src < n; src++){
            vector<int> vertex = Graph[src];
            for (int& dst : vertex){
                GraphR[dst].push_back(src);
            }
        }

        return GraphR;
    }

    void DFS(vector<vector<int>>& Graph, int root, vector<int>& color, vector<int>& DFS_Sequence){
        color[root] = GRAY;

        for (auto& node : Graph[root])
            if (color[node] == WHITE)
                DFS(Graph, node, color, DFS_Sequence);

        color[root] = BLACK;
        DFS_Sequence.push_back(root);
    }

    vector<int> DFSVisit(vector<vector<int>>& Graph){
        int n = Graph.size();
        vector<int> color(n, WHITE);
        vector<int> DFS_Sequence;

        for (int i = 1; i < Graph.size(); i++)
            if (color[i] == WHITE)
                DFS(Graph, i, color, DFS_Sequence);

        return DFS_Sequence;
    }

    vector<vector<int>> getSCCs(vector<vector<int>>& Graph, vector<int>& Sequence){
        int n = Graph.size();
        vector<int> color(n, WHITE);
        vector<vector<int>> SCCs;

        for (int i = 1; i < n; i++){
            int vertex = Sequence[i-1];
            if (color[vertex] == WHITE){
                vector<int> DFS_Sequence;
                DFS(Graph, vertex, color, DFS_Sequence);
                SCCs.push_back(DFS_Sequence);
            }
        }

        return SCCs;
    }

    int countComponents(int n, vector<vector<int>>& edges) {
        vector<vector<int>> Graph, GraphR, SCCs;
        vector<int> Sequence;

        Graph = GraphConstruction(n, edges);
        GraphR = ReverseGraph(Graph);

        Sequence = DFSVisit(GraphR);
        reverse(Sequence.begin(), Sequence.end());

        SCCs = getSCCs(Graph, Sequence);

        return SCCs.size();
    }
};
```

执行用时：60 ms, 在所有 C++ 提交中击败了18.78%的用户

内存消耗：15.5 MB, 在所有 C++ 提交中击败了13.23%的用户

## Solution - II Union-Find Set

c++

```c++
class Solution {
private:
    vector<int> parent, keys;

public:
    int Find_Set(int x){
        while (parent[x] != x)
            x = parent[x];
        
        return x;
    }

    void Union(int x, int y){
        int ROOT1 = Find_Set(x), ROOT2 = Find_Set(y);

        if (ROOT1 == ROOT2)
            return;
        
        parent[ROOT2] = ROOT1;
        parent[x] = ROOT1;
        parent[y] = ROOT1;
    }

    int countComponents(int n, vector<vector<int>>& edges) {
        parent.resize(n+1);

        for (int i = 1; i < n+1; i++)
            parent[i] = i;

        for (vector<int>& edge : edges)
            Union(edge[0] + 1, edge[1] + 1);

        for (int& key : parent)
            if (key != 0 && find(keys.begin(), keys.end(), Find_Set(key)) == keys.end())
                keys.push_back(Find_Set(key));

        return keys.size();
    }
};
```

执行用时：28 ms, 在所有 C++ 提交中击败了94.42%的用户

内存消耗：10.7 MB, 在所有 C++ 提交中击败了88.36%的用户

Attention

- 并查集的初始化
- 统计key的个数的时候要溯源(Find_Key)