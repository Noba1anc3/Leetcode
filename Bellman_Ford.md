```c++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;


class Solution{
private:
    vector<long> dist;
    vector<vector<long>> Weights;

public:
    void relax(vector<int> edge){
        int v1 = edge[0], v2 = edge[1];
        dist[v2] = min(dist[v2], dist[v1] + Weights[v1][v2]);
    }

    void weights_change(int n, vector<vector<int>>& weights){
        Weights = vector<vector<long>>(n, vector<long>(n, INT_MAX));
        for (const vector<int>& weight : weights)
            Weights[weight[0]][weight[1]] = weight[2];
    }

    bool Bellman_Ford(int n, vector<vector<int>> edges){
        dist = vector<long>(n, INT_MAX);
        dist[edges[0][0]] = 0;

        for (int i = 1; i < n; i++)
            for (const vector<int>& edge : edges)
                relax(edge);

        for (const vector<int>& edge : edges)
            if (dist[edge[1]] > dist[edge[0]] + Weights[edge[0]][edge[1]])
                return false;
        return true;
    }
};


int main()
{
    int n = 5;
    vector<vector<int>> edges = {{0,1},{0,2},{1,2},{2,1},{1,4},
                            {1,3},{2,3},{3,4},{4,3}};

    vector<vector<int>> weights = {{0,1,2},{0,2,7},{1,2,3},{2,1,2},{1,4,5},
                            {1,3,8},{2,3,1},{3,4,4},{4,3,5}};

    int n = 8;
    vector<vector<int>> edges = {{0,1},{0,3},{0,5},{1,2},{2,7},
                            {3,4},{4,3},{4,7},{5,6},{6,5},{6,7}};

    vector<vector<int>> weights = {{0,1,3},{0,3,5},{0,5,2},{1,2,-4},{2,7,4},
                            {3,4,6},{4,3,-3},{4,7,8},{5,6,3},{6,5,-6},{6,7,7}};


    Solution Slt = Solution();
    Slt.weights_change(n, weights);
    cout<<Slt.Bellman_Ford(n, edges);

    return 0;
}
```
