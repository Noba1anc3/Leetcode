```c++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

class Solution {
private:
    int WHITE = 0, GRAY = 1, BLACK = 2;
public:
    vector<vector<int>> GraphConstruction(int n, vector<vector<int>>& edges){
        vector<vector<int>> Graph(n+1, vector<int>());

        for(vector<int> edge : edges){
            Graph[edge[0]].push_back(edge[1]);
            Graph[edge[1]].push_back(edge[0]);
        }

        return Graph;
    }

    vector<vector<int>> ReverseGraph(vector<vector<int>>& Graph){
        int n = Graph.size();
        vector<vector<int>> GraphR(n, vector<int>());

        for(int src = 1; src < n; src++){
            vector<int> vertex = Graph[src];
            for (int dst : vertex){
                GraphR[dst].push_back(src);
            }
        }

        return GraphR;
    }

    void DFS(vector<vector<int>>& Graph, int root, vector<int>& color, vector<int>& DFS_Sequence){
        color[root] = GRAY;

        for (auto node : Graph[root])
            if (color[node] == WHITE)
                DFS(Graph, node, color, DFS_Sequence);

        color[root] = BLACK;
        DFS_Sequence.push_back(root);
    }

    vector<int> DFSVisit(vector<vector<int>>& Graph){
        int n = Graph.size();
        vector<int> color(n, WHITE);
        vector<int> DFS_Sequence;

        for (int i = 1; i < Graph.size(); i++)
            if (color[i] == WHITE)
                DFS(Graph, i, color, DFS_Sequence);

        return DFS_Sequence;
    }

    vector<vector<int>> getSCCs(vector<vector<int>>& Graph, vector<int>& Sequence){
        int n = Graph.size();
        vector<int> color(n, WHITE);
        vector<vector<int>> SCCs;

        for (int i = 1; i < n; i++){
            int vertex = Sequence[i-1];
            if (color[vertex] == WHITE){
                vector<int> DFS_Sequence;
                DFS(Graph, vertex, color, DFS_Sequence);
                SCCs.push_back(DFS_Sequence);
            }
        }

        return SCCs;
    }

    vector<vector<int>> SCC(int n, vector<vector<int>>& edges) {
        vector<vector<int>> Graph, GraphR, SCCs;
        vector<int> Sequence;

        Graph = GraphConstruction(n, edges);
        GraphR = ReverseGraph(Graph);

        Sequence = DFSVisit(GraphR);
        reverse(Sequence.begin(), Sequence.end());

        SCCs = getSCCs(Graph, Sequence);

        return SCCs;
    }
};

int main()
{
    vector<vector<int>> edges = {{1,3},{3,2},{2,1},{1,6},{6,4},{4,1},{4,7},{7,6},
    {5,4},{5,7},{8,7},{8,9},{9,8},{5,9},{4,10},{11,10},{10,12},{12,11},{6,11}};
    Solution slt = Solution();
    slt.SCC(12, edges);
    return 0;
}
```
