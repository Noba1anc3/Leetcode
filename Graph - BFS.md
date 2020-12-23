```c++
#include <iostream>
#include <algorithm>
#include <vector>
#include <queue>

using namespace std;

int WHITE = 0, GRAY = 1, BLACK = 2;

struct Graph{
    int Vnum;
    vector<vector<int>> AdjacentMatrix;
};

vector<int> adjacent(Graph G, int vertex){
    vector<int> Adjacent;
    vector<int> vertexRow = G.AdjacentMatrix[vertex];

    for (int i = 0; i < G.Vnum; i++){
        if (vertexRow[i]) Adjacent.push_back(i);
    }

    return Adjacent;
}

void BFSVisit(Graph G, int vertex, vector<int> &color, vector<int> &dist, vector<int> &pred){
    color[vertex] = GRAY;
    dist[vertex] = 0;
    
    queue<int> Q;
    Q.push(vertex);
    
    while (!Q.empty()){
        int curVertex = Q.front();
        Q.pop();
        
        for (auto adjVertex : adjacent(G, curVertex)){
            if (color[adjVertex] == WHITE){
                color[adjVertex] = GRAY;
                dist[adjVertex] = dist[curVertex] + 1;
                pred[adjVertex] = curVertex;
                Q.push(adjVertex);
            }
        }
        
        color[curVertex] = BLACK;
    }
    
    color[vertex] = BLACK;
}

void BFS(Graph G)
{
    vector<int> color(G.Vnum, WHITE), pred(G.Vnum, NULL), dist(G.Vnum, 0);

    BFSVisit(G, 1, color, dist, pred); // start from vertex-2

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
    vector<vector<int>> metrix(8, vector<int>(8, 0));

    metrix[0][1] = metrix[0][4] = metrix[1][0] = metrix[1][5] = metrix[4][0] = metrix[5][1] =
    metrix[5][2] = metrix[5][6] = metrix[6][5] = metrix[6][7] = metrix[6][2] = metrix[2][5] =
    metrix[2][6] = metrix[2][3] = metrix[7][6] = metrix[7][3] = metrix[3][2] = metrix[3][7] = 1;

    Graph G = {8, metrix};

    BFS(G);

    return 0;
}
```
