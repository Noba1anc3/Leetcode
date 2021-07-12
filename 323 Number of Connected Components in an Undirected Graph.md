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
    std::vector<std::vector<int>> GraphConstruction(int n, std::vector<std::vector<int>>& edges){
        std::vector<std::vector<int>> Graph(n+1, std::vector<int>());

        for(const vector<int>& edge : edges) {
            Graph[edge[0]+1].push_back(edge[1]+1);
            Graph[edge[1]+1].push_back(edge[0]+1);
        }

        return Graph;
    }

    std::vector<std::vector<int>> ReverseGraph(std::vector<std::vector<int>>& Graph){
        int n = Graph.size();
        std::vector<std::vector<int>> GraphR(n, std::vector<int>());

        for(int src = 1; src < n; src++){
            std::vector<int> vertex = Graph[src];
            for (const int& dst : vertex)
                GraphR[dst].push_back(src);
        }

        return GraphR;
    }

    void DFS(std::vector<std::vector<int>>& Graph, int root, std::vector<int>& color, std::vector<int>& DFS_Sequence){
        color[root] = GRAY;

        for (const auto& node : Graph[root])
            if (color[node] == WHITE)
                DFS(Graph, node, color, DFS_Sequence);

        color[root] = BLACK;
        DFS_Sequence.push_back(root);
    }

    std::vector<int> DFSVisit(std::vector<std::vector<int>>& Graph){
        int n = Graph.size();
        std::vector<int> color(n, WHITE);
        std::vector<int> DFS_Sequence;

        for (int i = 1; i < n; i++)
            if (color[i] == WHITE)
                DFS(Graph, i, color, DFS_Sequence);

        return DFS_Sequence;
    }

    std::vector<std::vector<int>> getSCCs(std::vector<std::vector<int>>& Graph, std::vector<int>& Sequence){
        int n = Graph.size();
        std::vector<int> color(n, WHITE);
        std::vector<std::vector<int>> SCCs;

        for (int i = 1; i < n; i++){
            int vertex = Sequence[i-1];
            if (color[vertex] == WHITE){
                std::vector<int> DFS_Sequence;
                DFS(Graph, vertex, color, DFS_Sequence);
                SCCs.push_back(DFS_Sequence);
            }
        }

        return SCCs;
    }

    int countComponents(int n, std::vector<std::vector<int>>& edges) {
        std::vector<vector<int>> Graph, GraphR, SCCs;
        std::vector<int> Sequence;

        Graph = GraphConstruction(n, edges);
        GraphR = ReverseGraph(Graph);

        Sequence = DFSVisit(GraphR);
        reverse(Sequence.begin(), Sequence.end());

        SCCs = getSCCs(Graph, Sequence);

        return SCCs.size();
    }
};
```

执行用时：36 ms, 在所有 C++ 提交中击败了40.07%的用户

内存消耗：16.5 MB, 在所有 C++ 提交中击败了17.23%的用户

## Solution - II Union-Find Set

c++

```c++
class Solution {
private:
    vector<int> parent, height, keys;

public:
    int Find_Set(int x){
        while (parent[x] != x)
            x = parent[x];
        return x;
    }

    void Union(int x, int y){
        int ROOT1 = Find_Set(x), ROOT2 = Find_Set(y);

        if (ROOT1 == ROOT2) return;
        
        if (height[ROOT1] <= height[ROOT2]) {
            if (height[ROOT1] == height[ROOT2])
                height[ROOT2]++;
            parent[ROOT1] = ROOT2;
        }
        else
            parent[ROOT2] = ROOT1;
    }

    int countComponents(int n, vector<vector<int>>& edges) {
        parent.resize(n+1);
        height.resize(n+1);

        for (int i = 1; i < n+1; i++) {
            parent[i] = i;
            height[i] = 1;
        }

        for (const vector<int>& edge : edges)
            Union(edge[0] + 1, edge[1] + 1);

        for (const int& key : parent)
            if (key != 0 && find(keys.begin(), keys.end(), Find_Set(key)) == keys.end())
                keys.push_back(Find_Set(key));

        return keys.size();
    }
};
```

执行用时：20 ms, 在所有 C++ 提交中击败了91.56%的用户

内存消耗：11.8 MB, 在所有 C++ 提交中击败了74.67%的用户

Attention

- 并查集的初始化
- 统计key的个数的时候要溯源(Find_Key)
