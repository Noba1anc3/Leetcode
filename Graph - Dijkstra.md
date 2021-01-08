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

public:
    vector<vector<int>> GraphConstruction(int n, vector<vector<int>>& edges){
        vector<vector<int>> Graph(n+1, vector<int>());

        for(const vector<int>& edge : edges)
            Graph[edge[0]].push_back(edge[1]);

        return Graph;
    }

    int findMin(vector<int>& dist, vector<int>& color){
        int mindst = INT_MAX, minIndex = -1;
        for (int i = 1; i < dist.size(); i++){
            if (dist[i] < mindst && color[i] == WHITE){
                minIndex = i;
                mindst = dist[i];
            }
        }
        return minIndex;
    }

    bool Dijkstra(int n, vector<vector<int>>& edges, vector<vector<int>>& weight, int root) {
        int vertices = 0;
        vector<vector<int>> Graph;
        vector<int> color(n+1, WHITE), dist(n+1, INT_MAX), pred(n+1, NULL);
        
        dist[root] = 0;
        Graph = GraphConstruction(n, edges);

        while (vertices != n){
            int curVertex = findMin(dist, color);
            if (curVertex == -1) return false; // 该源点无SSSP
            for (const int& adjVertex : Graph[curVertex]){;
                if (color[adjVertex] == WHITE &&
                    dist[curVertex] + weight[curVertex][adjVertex] < dist[adjVertex]){
                    dist[adjVertex] = dist[curVertex] + weight[curVertex][adjVertex];
                    pred[adjVertex] = curVertex;
                }
            }
            vertices++;
            color[curVertex] = BLACK;
        }
        
        return true;
    }
};

int main()
{
    int n = 5;
    vector<vector<int>> edges = {{1,3},{1,2},{2,3},{3,2},{3,4},
                                 {2,4},{2,5},{4,5},{5,4}};
    vector<vector<int>> weight(n+1, vector<int>(n+1, INT_MAX));

    weight[1][2] = 2, weight[1][3] = 7, weight[2][3] = 3,
    weight[3][4] = 1, weight[3][2] = 2, weight[2][4] = 8,
    weight[2][5] = 5, weight[4][5] = 4, weight[5][4] = 5;

    Solution slt = Solution();
    slt.Dijkstra(n, edges, weight, 1);

    return 0;
}
```

