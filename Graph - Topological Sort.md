```c++
class Solution {
public:
    vector<int> Topological_Sort(int vertices, vector<vector<int>>& edges) {
        vector<vector<int>> Graph(vertices, vector<int>());
        vector<int> InDegree(vertices, 0);
        vector<int> Topological;
        queue<int> Q;

        for (auto edge : edges){
            Graph[edge[1]].push_back(edge[0]);
            InDegree[edge[0]]++;
        }

        for (int i = 0; i < vertices; i++)
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

        return Topological;
    }
};
```

## DFS based

我们可以将深度优先搜索的流程与拓扑排序的求解联系起来，用一个栈来存储所有**已经搜索完成的节点**。(终点)

> 对于一个节点 u，如果它的所有相邻节点都已经搜索完成，那么在搜索回溯到 u 的时候，u 本身也会变成一个已经搜索完成的节点。这里的「相邻节点」指的是从 u 出发通过一条有向边可以到达的所有节点。
>

这样一来，我们对图进行深度优先搜索。当每个节点进行**回溯**的时候，我们把该节点放入栈中。最终从栈顶到栈底的序列就是拓扑排序。

```c++
class Solution {
private:
    int WHITE = 0, GRAY = 1, BLACK = 2;
    bool acyclic = true;

    vector<int> order;
    vector<int> color;
    vector<vector<int>> Graph;

public:
    void DFSVisit(int vertex){
        color[vertex] = GRAY;
        for (auto adjVertex : Graph[vertex]){
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
        order.push_back(vertex);
        color[vertex] = BLACK;
    }

    vector<int> Topological_Sort(int vertices, vector<vector<int>>& edges) {
        color.resize(vertices);
        Graph.resize(vertices);

        for (vector<int> edge : edges)
            Graph[edge[0]].push_back(edge[1]);
        
        for (int root = 0; root < vertices && acyclic; root++)
            if (color[root] == WHITE)
                DFSVisit(root);

        if (!acyclic)
            return {};

        reverse(order.begin(), order.end());
        return order;
    }
};
```

Attention

- vector<int> array.resize(int size)
- reverse(vector<int>.begin(), vector<int>.end())