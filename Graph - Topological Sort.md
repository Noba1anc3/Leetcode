```c++
class Solution {
public:
    std::vector<int> Topological_Sort(int n, std::vector<std::vector<int>>& edges) {
        std::vector<std::vector<int>> Graph(n, std::vector<int>());
        std::vector<int> InDegree(n, 0);
        std::vector<int> Topological;
        std::queue<int> Q;

        for (const std::vector<int>& edge : edges){
            Graph[edge[0]].push_back(edge[1]);
            InDegree[edge[1]]++;
        }

        for (int i = 0; i < n; i++)
            if (InDegree[i] == 0)
                Q.push(i);

        while (!Q.empty()){
            int vertex = Q.front(); Q.pop();
            Topological.push_back(vertex);
            for (const int& adjVertex : Graph[vertex]){
                InDegree[adjVertex]--;
                if (InDegree[adjVertex] == 0)
                    Q.push(adjVertex);
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

    std::vector<int> order;
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
        order.push_back(vertex);
        color[vertex] = BLACK;
    }

    std::vector<int> Topological_Sort(int n, std::vector<std::vector<int>>& edges) {
        color.resize(n);
        Graph.resize(n);

        for (const std::vector<int>& edge : edges)
            Graph[edge[0]].push_back(edge[1]);

        for (int root = 0; root < n && acyclic; root++)
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
