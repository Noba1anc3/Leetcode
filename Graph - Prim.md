c++

因c++的优先队列不支持修改元素，在此将优先队列替换为普通列表，通过上色的方式标注其已加入MST元素集合当中

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
    vector<vector<int>> GraphConstruction(int n, vector<vector<int>> edges){
        vector<vector<int>> Graph(n+1, vector<int>());

        for(vector<int> edge : edges){
            Graph[edge[0]].push_back(edge[1]);
            Graph[edge[1]].push_back(edge[0]);
        }

        return Graph;
    }

    int findMin(vector<int> key, vector<int> color){
        int minKey = INT_MAX, minIndex = -1;
        for (int i = 1; i < key.size(); i++){
            if (key[i] < minKey && color[i] == WHITE){
                minIndex = i;
                minKey = key[i];
            }
        }
        return minIndex;
    }

    int Prim(int n, vector<vector<int>> edges, vector<vector<int>> weight, int root) {
        int vertices = 0, MSTValue = 0;
        vector<vector<int>> Graph;
        vector<int> color(n+1, WHITE), key(n+1, INT_MAX), pred(n+1, NULL);

        key[root] = 0;
        Graph = GraphConstruction(n, edges);

        while (vertices != n){
            int curVertex = findMin(key, color);
            for (int adjVertex : Graph[curVertex]){;
                if (color[adjVertex] == WHITE && (weight[curVertex][adjVertex] < key[adjVertex]
                || weight[adjVertex][curVertex] < key[adjVertex])){
                    key[adjVertex] = min(weight[adjVertex][curVertex], weight[curVertex][adjVertex]);
                    pred[adjVertex] = curVertex;
                }
            }
            vertices++;
            color[curVertex] = BLACK;
        }

        for(int i = 1; i < n+1; i++)
            MSTValue += key[i];

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
    slt.Prim(n, edges, weight, 1);
    return 0;
}
```

