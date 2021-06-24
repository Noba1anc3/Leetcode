There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the `ith` city and the `jth` city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return the total number of **provinces**.



Examples:

![](https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg)

```
Input: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
Output: 2
```

![](https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg)

```
Input: isConnected = [[1,0,0],[0,1,0],[0,0,1]]
Output: 3
```

## Idea

强连通分量个数问题

## Solution - I SCC

c++
```c++
class Solution {
private:
    int WHITE = 0, GRAY = 1, BLACK = 2;

public:
    vector<vector<int>> GraphConstruction(vector<vector<int>>& edges){
        vector<vector<int>> Graph(edges.size()+1, vector<int>());

        for(int i = 0; i < edges.size(); i++){
            vector<int> vertex = edges[i];
            for (int j = 0; j < vertex.size(); j++)
                if (i != j && vertex[j] == 1)
                    Graph[i+1].push_back(j+1);
        }

        return Graph;
    }

    vector<vector<int>> ReverseGraph(vector<vector<int>>& Graph){
        int n = Graph.size();
        vector<vector<int>> GraphR(n, vector<int>());

        for(int src = 1; src < n; src++){
            vector<int> vertex = Graph[src];
            for (const int& dst : vertex){
                GraphR[dst].push_back(src);
            }
        }

        return GraphR;
    }

    void DFS(vector<vector<int>>& Graph, int root, vector<int>& color, vector<int>& DFS_Sequence){
        color[root] = GRAY;

        for (const auto& node : Graph[root])
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

    int findCircleNum(vector<vector<int>>& isConnected) {
        vector<vector<int>> Graph, GraphR, SCCs;
        vector<int> Sequence;

        Graph = GraphConstruction(isConnected);
        GraphR = ReverseGraph(Graph);

        Sequence = DFSVisit(GraphR);
        reverse(Sequence.begin(), Sequence.end());

        SCCs = getSCCs(Graph, Sequence);

        return SCCs.size();
    }
};
```

执行用时：24 ms, 在所有 C++ 提交中击败了81.93%的用户

内存消耗：17.3 MB, 在所有 C++ 提交中击败了5.01%的用户

## Solution - II DFS

```c++
class Solution {
private:
    int WHITE = 0, GRAY = 1, BLACK = 2;

public:
    vector<vector<int>> GraphConstruction(vector<vector<int>>& edges){
        vector<vector<int>> Graph(edges.size()+1, vector<int>());

        for(int i = 0; i < edges.size(); i++){
            vector<int> vertex = edges[i];
            for (int j = 0; j < vertex.size(); j++)
                if (i != j && vertex[j] == 1)
                    Graph[i+1].push_back(j+1);
        }

        return Graph;
    }

    void DFS(vector<vector<int>>& Graph, int root, vector<int>& color){
        color[root] = GRAY;

        for (auto node : Graph[root])
            if (color[node] == WHITE)
                DFS(Graph, node, color);

        color[root] = BLACK;
    }

    int DFSVisit(vector<vector<int>>& Graph){
        int num = 0, n = Graph.size();
        vector<int> color(n, WHITE);

        for (int i = 1; i < Graph.size(); i++)
            if (color[i] == WHITE){
                num++;
                DFS(Graph, i, color);
            }

        return num;
    }

    int findCircleNum(vector<vector<int>>& isConnected) {
        vector<vector<int>> Graph;
        int num;

        Graph = GraphConstruction(isConnected);
        num = DFSVisit(Graph);

        return num;
    }
};
```

执行用时：52 ms, 在所有 C++ 提交中击败了91.55%的用户

内存消耗：15.6 MB, 在所有 C++ 提交中击败了5.01%的用户

## Solution - III Union-Find Set

```c++
class Solution {
private:
    int num = 0;
    vector<int> height;
    vector<int> parent;
    vector<int> created;
    vector<int> keys;

public:
    void Create_Set(int key){
        parent[key] = key;
        height[key] = 1;
        created[key] = 1;
    }

    int Find_Set(int key){
        while(parent[key] != key)
            key = parent[key];
        
        return key;
    }

    void Union(int key1, int key2){
        if (key1 == key2)
            return;

        int ROOT1 = Find_Set(key1);
        int ROOT2 = Find_Set(key2);

        if (ROOT1 == ROOT2)
            return;

        if (height[ROOT1] <= height[ROOT2]){
            if (height[ROOT1] == height[ROOT2]){
                height[ROOT2]++;
            }
            parent[ROOT2] = ROOT1;
        }
        else{
            parent[ROOT1] = ROOT2;
        }
    }

    int findCircleNum(vector<vector<int>>& isConnected) {
        parent.resize(isConnected.size()+1);
        height.resize(isConnected.size()+1);
        created.resize(isConnected.size()+1);

        for (int i = 0; i < isConnected.size(); i++){
            for (int j = 0; j < isConnected.size(); j++){
                if (isConnected[i][j]){
                    if (created[i+1] == 0){
                        Create_Set(i+1);
                    }
                    if (created[j+1] == 0){
                        Create_Set(j+1);
                    }
                    Union(i+1, j+1);
                }
            }
        }

        for (int key : parent){
            key = Find_Set(key);
            if (key != 0){
                vector<int>::iterator it = find(keys.begin(), keys.end(), key);
                if (it == keys.end()){
                    num++;
                    keys.push_back(key);
                }
            }
        }

        return num;
    }
};
```

执行用时：56 ms, 在所有 C++ 提交中击败了76.24%的用户

内存消耗：13.9 MB, 在所有 C++ 提交中击败了40.50%的用户

Attention

- union的时候如果key一样或setID一样则return
- keyID避免用0，使用i+1作为keyID
- 查看有集合集合的时候要推到集合id
