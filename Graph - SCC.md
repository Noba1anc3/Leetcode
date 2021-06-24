```c++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

class Solution {
private:
    int WHITE = 0, GRAY = 1, BLACK = 2;
public:
    std::vector<std::vector<int>> GraphConstruction(int n, std::vector<std::vector<int>>& edges){
        std::vector<std::vector<int>> Graph(n+1, std::vector<int>());

        for(const vector<int>& edge : edges) {
            Graph[edge[0]].push_back(edge[1]);
            Graph[edge[1]].push_back(edge[0]);
        }

        return Graph;
    }

    std::vector<std::vector<int>> ReverseGraph(std::vector<std::vector<int>>& Graph){
        int n = Graph.size();
        std::vector<std::vector<int>> GraphR(n, std::vector<int>());

        for(int src = 1; src < n; src++){
            std::vector<int> vertex = Graph[src];
            for (const int& dst : vertex)
                GraphR[dst].push_back(src);
        }

        return GraphR;
    }

    void DFS(std::vector<std::vector<int>>& Graph, int root, std::vector<int>& color, std::vector<int>& DFS_Sequence){
        color[root] = GRAY;

        for (const auto& node : Graph[root])
            if (color[node] == WHITE)
                DFS(Graph, node, color, DFS_Sequence);

        color[root] = BLACK;
        DFS_Sequence.push_back(root);
    }

    std::vector<int> DFSVisit(std::vector<std::vector<int>>& Graph){
        int n = Graph.size();
        std::vector<int> color(n, WHITE);
        std::vector<int> DFS_Sequence;

        for (int i = 1; i < n; i++)
            if (color[i] == WHITE)
                DFS(Graph, i, color, DFS_Sequence);

        return DFS_Sequence;
    }

    std::vector<std::vector<int>> getSCCs(std::vector<std::vector<int>>& Graph, std::vector<int>& Sequence){
        int n = Graph.size();
        std::vector<int> color(n, WHITE);
        std::vector<std::vector<int>> SCCs;

        for (int i = 1; i < n; i++){
            int vertex = Sequence[i-1];
            if (color[vertex] == WHITE){
                std::vector<int> DFS_Sequence;
                DFS(Graph, vertex, color, DFS_Sequence);
                SCCs.push_back(DFS_Sequence);
            }
        }

        return SCCs;
    }

    std::vector<std::vector<int>> SCC(int n, std::vector<std::vector<int>>& edges) {
        std::vector<std::vector<int>> Graph, GraphR, SCCs;
        std::vector<int> Sequence;

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
