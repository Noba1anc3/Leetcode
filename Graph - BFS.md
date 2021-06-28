## Adjacent List

```c++
class Solution {
public:
    int WHITE = 0, GRAY = 1, BLACK = 2;

    void BFSVisit(vector<vector<int>> G, int vertex, vector<int> &color){
        color[vertex] = GRAY;
        
        queue<int> Q;
        Q.push(vertex);
        
        while (!Q.empty()){
            int curVertex = Q.front();
            Q.pop();
            
            for (int adjVertex : G[curVertex]){
                if (color[adjVertex] == WHITE){
                    color[adjVertex] = GRAY;
                    Q.push(adjVertex);
                }
            }
            
            color[curVertex] = BLACK;
        }
    }

    void BFS(int n, vector<vector<int>>& edges) {
        int start = 0;
        vector<int> color(n, WHITE);
        vector<vector<int>> Graph(n, vector<int>());
        
        for (vector<int> edge : edges){
            Graph[edge[0]].push_back(edge[1]);
            Graph[edge[1]].push_back(edge[0]);
        }

        BFSVisit(Graph, start, color);
    }
};
```

## Adjacent Metrix

```c++
#include <iostream>
#include <algorithm>
#include <vector>
#include <queue>

using namespace std;

int WHITE = 0, GRAY = 1, BLACK = 2;

struct Graph{
    int Vnum;
    std::vector<std::vector<int>> AdjacentMatrix;
};

std::vector<int> adjacent(Graph G, int vertex){
    std::vector<int> Adjacent;

    for (int i = 0; i < G.Vnum; i++)
        if (G.AdjacentMatrix[vertex][i])
            Adjacent.push_back(i);

    return Adjacent;
}

void BFSVisit(Graph G, int vertex, std::vector<int>& color, std::vector<int>& dist, std::vector<int>& pred){
    color[vertex] = GRAY;
    dist[vertex] = 0;

    queue<int> Q;
    Q.push(vertex);

    while (!Q.empty()){
        int curVertex = Q.front();
        Q.pop();

        for (const int& adjVertex : adjacent(G, curVertex)){
            if (color[adjVertex] == WHITE){
                color[adjVertex] = GRAY;
                dist[adjVertex] = dist[curVertex] + 1;
                pred[adjVertex] = curVertex;
                Q.push(adjVertex);
            }
        }

        color[curVertex] = BLACK;
    }
}

void BFS(Graph G)
{
    std::vector<int> color(G.Vnum, WHITE), pred(G.Vnum, NULL), dist(G.Vnum, 0);

    // BFSVisit(G, 1, color, dist, pred);  start from vertex-2

    for (int i = 0; i < G.Vnum; i++)
        if (color[i] == WHITE)
            BFSVisit(G, i, color, dist, pred);

    cout<<"color: ";
    for(auto ele : color)
        cout<<ele<<' ';
    cout<<'\n'<<" dist: ";
    for(auto ele : dist)
        cout<<ele<<' ';
    cout<<'\n'<<" pred: ";
    for(auto ele : pred)
        cout<<ele+1<<' ';
}



int main()
{
    std::vector<std::vector<int>> matrix(8, std::vector<int>(8, 0));

    matrix[0][1] = matrix[0][4] = matrix[1][0] = matrix[1][5] = matrix[4][0] = matrix[5][1] =
    matrix[5][2] = matrix[5][6] = matrix[6][5] = matrix[6][7] = matrix[6][2] = matrix[2][5] =
    matrix[2][6] = matrix[2][3] = matrix[7][6] = matrix[7][3] = matrix[3][2] = matrix[3][7] = 1;

    Graph G = {8, matrix};

    BFS(G);

    return 0;
}
```
