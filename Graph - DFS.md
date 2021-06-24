## Adjacent List

### 连通图

```c++
class Solution {
private:
    int time = 0, root = 0;
    int WHITE = 0, GRAY = 1, BLACK = 2;
    std::vector<std::vector<int>> Graph;
    std::vector<int> start, finish, pred, color;

public:
    void init(int n){
        pred.resize(n);
        color.resize(n);
        start.resize(n);
        finish.resize(n);
        Graph.resize(n);
    }

    void DFSVisit(int vertex){
        color[vertex] = GRAY;
        start[vertex] = ++time;

        for (const int& adjVertex : Graph[vertex])
            if (color[adjVertex] == WHITE) {
                pred[adjVertex] = vertex;
                DFSVisit(adjVertex);
            }
	    
        color[vertex] = BLACK;
        finish[vertex] = ++time;
    }

    void DFS(int n, std::vector<std::vector<int>>& edges) {
        init(n);

        for (const std::vector<int>& edge : edges)
            Graph[edge[0]].push_back(edge[1]);

        DFSVisit(root);
    }
}
```

### 非连通图

```c++
class Solution {
private:
    int time = 0;
    int WHITE = 0, GRAY = 1, BLACK = 2;
    std::vector<std::vector<int>> Graph;
    std::vector<int> start, finish, pred, color;

public:
    void init(int n){
        pred.resize(n);
        color.resize(n);
        start.resize(n);
        finish.resize(n);
        Graph.resize(n);
    }

    void DFSVisit(int vertex){
        color[vertex] = GRAY;
        start[vertex] = ++time;

        for (const int& adjVertex : Graph[vertex])
            if (color[adjVertex] == WHITE) {
                pred[adjVertex] = vertex;
                DFSVisit(adjVertex);
            }

        color[vertex] = BLACK;
        finish[vertex] = ++time;
    }

    void DFS(int n, std::vector<std::vector<int>>& edges) {
        init(n);

        for (const std::vector<int>& edge : edges)
            Graph[edge[0]].push_back(edge[1]);

        for (int i = 0; i < n; i++)
            if (color[i] == WHITE)
                DFSVisit(i);
    }
}
```

### 环路测试

```c++
class Solution {
private:
    int time = 0;
    int WHITE = 0, GRAY = 1, BLACK = 2;
    std::vector<std::vector<int>> Graph;
    std::vector<int> start, finish, pred, color;
    bool acyclic = true;

public:
    void init(int n){
        pred.resize(n);
        color.resize(n);
        start.resize(n);
        finish.resize(n);
        Graph.resize(n);
    }

    void DFSVisit(int vertex){
        color[vertex] = GRAY;
        start[vertex] = ++time;

        for (const int& adjVertex : Graph[vertex]) {
            if (color[adjVertex] == GRAY) {
                acyclic = false;
                return;
            }
            
            if (color[adjVertex] == WHITE) {
                pred[adjVertex] = vertex;
                DFSVisit(adjVertex);
                if (!acyclic) return;
            }
        }
        
        color[vertex] = BLACK;
        finish[vertex] = ++time;
    }

    bool DFS(int n, std::vector<std::vector<int>>& edges) {
        init(n);

        for (const std::vector<int>& edge : edges)
            Graph[edge[0]].push_back(edge[1]);

        for (int i = 0; i < n; i++)
            if (color[i] == WHITE)
                DFSVisit(i);
        
        return acyclic;
    }
}
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

void DFSVisit(Graph G, int vertex, int &time, vector<int> &color,
              vector<int> &start, vector<int> &finish, vector<int> &pred){

    color[vertex] = GRAY;
    start[vertex] = ++time;

    for (auto adjVertex : adjacent(G, vertex)){
        if (color[adjVertex] == WHITE){
            pred[adjVertex] = vertex;
            DFSVisit(G, adjVertex, time, color, start, finish, pred);
        }
    }

    color[vertex] = BLACK;
    finish[vertex] = ++time;
}

void DFS(Graph G)
{
    vector<int> color(G.Vnum, WHITE), pred(G.Vnum, -1), start(G.Vnum, 0), finish(G.Vnum, 0);
    int time = 0;

    // DFSVisit(G, 1, time, color, start, finish, pred); // start from vertex-2

    for (int i = 0; i < G.Vnum; i++){
        if (color[i] == WHITE){
            DFSVisit(G, i, color, dist, pred);
        }
    }

    cout<<" color: ";
    for(auto ele : color)
        cout<<ele<<' ';

    cout<<'\n'<<" start: ";
    for(auto ele : start){
        if (ele < 10)
            cout<<'0'<<ele<<' ';
        else
            cout<<ele<<' ';
    }

    cout<<'\n'<<"finish: ";
    for(auto ele : finish){
        if (ele < 10)
            cout<<'0'<<ele<<' ';
        else
            cout<<ele<<' ';
    }

    cout<<'\n'<<"  pred: ";
    for(auto ele : pred)
        if (ele == -1)
            cout<<"ROOT"<<' ';
        else
            cout<<ele+1<<' ';
}



int main()
{
    vector<vector<int>> metrix(8, vector<int>(8, 0));

    metrix[0][1] = metrix[0][4] = metrix[1][0] = metrix[1][5] = metrix[4][0] = metrix[5][1] =
    metrix[5][2] = metrix[5][6] = metrix[6][5] = metrix[6][7] = metrix[6][2] = metrix[2][5] =
    metrix[2][6] = metrix[2][3] = metrix[7][6] = metrix[7][3] = metrix[3][2] = metrix[3][7] = 1;

    Graph G = {8, metrix};

    DFS(G);

    return 0;
}
```
