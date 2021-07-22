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
    std::vector<std::vector<int>> GraphConstruction(int n, std::vector<std::vector<int>>& edges){
        std::vector<std::vector<int>> Graph(n+1, std::vector<int>());

        for(const std::vector<int>& edge : edges)
            Graph[edge[0]].push_back(edge[1]);

        return Graph;
    }

    std::vector<int> Dijkstra(int n, std::vector<std::vector<int>>& Graph, std::vector<std::vector<int>>& weight, int root) {
        std::vector<int> color(n+1, WHITE), dist(n+1, INT_MAX), pred(n+1, NULL);
        std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, std::greater<std::pair<int, int>>> Q;

        dist[root] = 0;
        Q.push(std::pair<int, int>(dist[root], root));

        while (!Q.empty()){
            int curVertex = Q.top().second; Q.pop();
            for (const int& adjVertex : Graph[curVertex]){;
                if (color[adjVertex] == WHITE &&
                    dist[curVertex] + weight[curVertex][adjVertex] < dist[adjVertex]){
                    dist[adjVertex] = dist[curVertex] + weight[curVertex][adjVertex];
                    pred[adjVertex] = curVertex;
                    Q.push(std::pair<int, int>(dist[adjVertex], adjVertex));
                }
            }
            color[curVertex] = BLACK;
        }

        return dist;
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

    vector<vector<int>> G = slt.GraphConstruction(n, edges);
    cout<<slt.Dijkstra(n, G, weight, 1)[5];

    return 0;
}
```
