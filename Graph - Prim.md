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
    std::vector<std::vector<int>> GraphConstruction(int n, std::vector<std::vector<int>>& edges, std::vector<std::vector<int>>& weights){
        std::vector<std::vector<int>> Graph(n+1, std::vector<int>());

        for(vector<int> edge : edges){
            Graph[edge[0]].push_back(edge[1]);
            Graph[edge[1]].push_back(edge[0]);
        }

        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= n; j++)
                weights[j][i] = weights[i][j];

        return Graph;
    }

    int Prim(int n, std::vector<std::vector<int>> Graph, std::vector<std::vector<int>> weight, int root) {
        int MSTValue = 0;
        vector<int> color(n+1, WHITE), key(n+1, INT_MAX);//, pred(n+1, NULL);
        std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, std::greater<std::pair<int, int>>> Q;
        Q.push(pair<int, int>(0, root));
        key[root] = 0;

        while (!Q.empty()){
            int curVertex = Q.top().second; Q.pop();
            for (int adjVertex : Graph[curVertex]){
                if (color[adjVertex] == WHITE && weight[curVertex][adjVertex] < key[adjVertex]){
                    key[adjVertex] = weight[curVertex][adjVertex];
                    // pred[adjVertex] = curVertex;
                    Q.push(pair<int, int>(key[adjVertex], adjVertex));
                }
            }
            color[curVertex] = BLACK;
        }

        for(int i = 1; i <= n; i++)
            MSTValue += key[i];

        return MSTValue;
    }
};

int main()
{
    int n = 7;
    vector<vector<int>> edges = {{1,2},{1,3},{2,3},{2,4},{2,5},{3,4},
                                 {3,6},{4,5},{4,6},{5,6},{5,7},{6,7}};
    vector<vector<int>> weight(n+1, vector<int>(n+1, INT_MAX));

    weight[1][2] = 4, weight[1][3] = 8, weight[2][3] = 9,
    weight[2][4] = 8, weight[2][5] = 10,weight[3][4] = 2,
    weight[3][6] = 1, weight[4][5] = 7, weight[4][6] = 2,
    weight[5][6] = 5, weight[5][7] = 6, weight[6][7] = 2;

    Solution slt = Solution();
    std::vector<std::vector<int>> G = slt.GraphConstruction(n, edges, weight);
    cout<<slt.Prim(n, G, weight, 1);

    return 0;
}
```
