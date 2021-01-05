c++

```c++
#include <iostream>
#include <algorithm>
#include <vector>
#include <queue>

using namespace std;

class Solution {
private:
    int WHITE = 0, BLACK = 1;
    vector<int> parent, height;
    struct Edge{
        int Vertex1;
        int Vertex2;
        int Weight;
    };

public:
    vector<Edge> convertEdge(vector<vector<int>> weight){
        vector<Edge> Weights;
        for (int src = 1; src < weight.size(); src++){
            vector<int> srcEdges = weight[src];
            for (int dst = 1; dst < weight.size(); dst++){
                int w = weight[src][dst];
                if (w != INT_MAX){
                    Edge edge;
                    edge.Vertex1 = src;
                    edge.Vertex2 = dst;
                    edge.Weight = w;
                    Weights.push_back(edge);
                    edge.Vertex1 = dst;
                    edge.Vertex2 = src;
                    Weights.push_back(edge);
                }
            }
        }
        return Weights;
    }

    void Create_Set(int x){
        parent[x] = x;
        height[x] = 1;
    }

    int Find_Set(int x){
        while (parent[x] != x){
            x = parent[x];
        }
        return x;
    }

    void Union(int x, int y){
        int ROOT1 = Find_Set(x);
        int ROOT2 = Find_Set(y);

        if (height[ROOT1] <= height[ROOT2]){
            if (height[ROOT1] == height[ROOT2])
                height[ROOT2]++;
            parent[ROOT2] = ROOT1;
        }
        else{
            parent[ROOT1] = ROOT2;
        }
    }

    static bool cmp(Edge E1, Edge E2){
        return E1.Weight < E2.Weight;
    }

    int Kruskal(int n, vector<vector<int>> edges, vector<vector<int>> weight) {
        vector<vector<int>> Graph, MST;
        vector<Edge> Weights;
        int MSTValue = 0;
        
        parent.resize(n+1);
        height.resize(n+1);

        Weights = convertEdge(weight);
        sort(Weights.begin(), Weights.end(), cmp);

        for (int i = 1; i <= n; i++)
            Create_Set(i);

        for (Edge e : Weights){
            int v1 = e.Vertex1, v2 = e.Vertex2;
            if (Find_Set(v1) != Find_Set(v2)){
                MST.push_back({v1, v2});
                MSTValue += e.Weight;
                Union(v1, v2);
            }
        }
        
        return MSTValue;
    }
};

int main()
{
    int n = 7;
    vector<vector<int>> edges = {{1,2},{1,3},{2,3},{2,4},{3,4},
                                 {2,5},{5,4},{4,6},{5,6},{3,6},{5,7},{6,7}};
    vector<vector<int>> weight(n+1, vector<int>(n+1, INT_MAX));

    weight[1][2] = 4, weight[1][3] = 8, weight[2][3] = 9,
    weight[2][4] = 8,weight[3][4] = 2, weight[5][7] = 6,
    weight[3][6] = 1, weight[4][6] = 2, weight[2][5] = 10,
    weight[4][5] = 7, weight[5][6] = 5, weight[6][7] = 2;

    Solution slt = Solution();
    slt.Kruskal(n, edges, weight);

    return 0;
}
```

