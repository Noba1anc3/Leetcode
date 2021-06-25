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
#include <vector>

using namespace std;

struct Graph{
    int Vnum;
    vector<vector<int>> AdjacentMatrix;
};


class DFS_Traverse{
private:
    int time = 0;
    int WHITE = 0, GRAY = 1, BLACK = 2;

public:
    vector<int> adjacent(Graph G, int vertex){
        vector<int> Adjacent;
        vector<int> vertexRow = G.AdjacentMatrix[vertex];

        for (int i = 0; i < G.Vnum; i++)
            if (vertexRow[i]) Adjacent.push_back(i);

        return Adjacent;
    }

    void DFSVisit(Graph G, int vertex, vector<int> &color,
                  vector<int> &start, vector<int> &finish, vector<int> &pred){

        color[vertex] = GRAY;
        start[vertex] = ++time;

        for (const int& adjVertex : adjacent(G, vertex))
            if (color[adjVertex] == WHITE){
                pred[adjVertex] = vertex;
                DFSVisit(G, adjVertex, color, start, finish, pred);
            }

        color[vertex] = BLACK;
        finish[vertex] = ++time;
    }

    void DFS(Graph G)
    {
        vector<int> color(G.Vnum, WHITE), pred(G.Vnum, -1), start(G.Vnum, 0), finish(G.Vnum, 0);

        // DFSVisit(G, 1, color, start, finish, pred); // start from vertex-2

        for (int i = 0; i < G.Vnum; i++)
            if (color[i] == WHITE)
                DFSVisit(G, i, color, start, finish, pred);

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
};


int main()
{
    vector<vector<int>> matrix(8, vector<int>(8, 0));

    matrix[0][1] = matrix[0][4] = matrix[1][0] = matrix[1][5] = matrix[4][0] = matrix[5][1] =
    matrix[5][2] = matrix[5][6] = matrix[6][5] = matrix[6][7] = matrix[6][2] = matrix[2][5] =
    matrix[2][6] = matrix[2][3] = matrix[7][6] = matrix[7][3] = matrix[3][2] = matrix[3][7] = 1;

    Graph G = {8, matrix};

    DFS_Traverse DFS = DFS_Traverse();
    DFS.DFS(G);

    return 0;
}
```
