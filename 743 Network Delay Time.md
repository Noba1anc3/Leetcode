There are `N` network nodes, labelled `1` to `N`.

Given `times`, a list of travel times as directed edges `times[i] = (u, v, w)`, where `u` is the source node, `v` is the target node, and `w` is the time it takes for a signal to travel from source to target.

Now, we send a signal from a certain node `K`. How long will it take for all nodes to receive the signal? If it is impossible, return `-1`.

 

![](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)

**Example:**

```
Input: times = [[2,1,1],[2,3,1],[3,4,1]], N = 4, K = 2
Output: 2
```

## Solution

c++  SSSP 


```c++
class Solution {
private:
    int WHITE = 0, BLACK = 1;

public:
    vector<vector<vector<int>>> GraphConstruction(int n, vector<vector<int>>& edges){
        vector<vector<int>> Graph(n+1, vector<int>());
        vector<vector<int>> weight(n+1, vector<int>(n+1, INT_MAX));

        vector<vector<vector<int>>> ans;

        for(const vector<int>& edge : edges){
            Graph[edge[0]].push_back(edge[1]);
            weight[edge[0]][edge[1]] = edge[2];
        }

        ans.push_back(Graph);
        ans.push_back(weight);

        return ans;
    }

    int findMin(vector<int>& dist, vector<int>& color){
        int mindst = INT_MAX, minIndex = -1;
        for (int i = 1; i < dist.size(); i++){
            if (dist[i] < mindst && color[i] == WHITE){
                minIndex = i;
                mindst = dist[i];
            }
        }
        return minIndex;
    }

    vector<int> Dijkstra(int n, vector<vector<int>>& Graph, vector<vector<int>>& weight, int root) {
        int vertices = 0;
        vector<int> color(n+1, WHITE), dist(n+1, INT_MAX);
        
        dist[root] = 0;

        while (vertices != n){
            int curVertex = findMin(dist, color);
            if (curVertex == -1) return {-1};
            for (const int& adjVertex : Graph[curVertex]){;
                if (color[adjVertex] == WHITE &&
                    dist[curVertex] + weight[curVertex][adjVertex] < dist[adjVertex]){
                    dist[adjVertex] = dist[curVertex] + weight[curVertex][adjVertex];
                }
            }
            vertices++;
            color[curVertex] = BLACK;
        }

        return dist;
    }

    int networkDelayTime(vector<vector<int>>& times, int N, int K) {
        vector<vector<vector<int>>> ret = GraphConstruction(N, times);
        vector<vector<int>> Graph = ret[0];
        vector<vector<int>> weight = ret[1];
        
        vector<int> dist = Dijkstra(N, Graph, weight, K);
		int maxDist = -1;

        if (dist[0] != -1)
            for (int i = 1; i < N + 1; i++)
                if (maxDist < dist[i])
                    maxDist = dist[i];

        return maxDist;
    }
};
```

执行用时：160 ms, 在所有 C++ 提交中击败了84.71%的用户

内存消耗：30.2 MB, 在所有 C++ 提交中击败了22.04%的用户

### Priority Queue

```c++
class Solution {
private:
    int WHITE = 0, BLACK = 1;

public:
    vector<vector<vector<int>>> GraphConstruction(int n, vector<vector<int>>& edges){
        vector<vector<int>> Graph(n+1, vector<int>());
        vector<vector<int>> weight(n+1, vector<int>(n+1, INT_MAX));

        vector<vector<vector<int>>> ans;

        for(const vector<int>& edge : edges){
            Graph[edge[0]].push_back(edge[1]);
            weight[edge[0]][edge[1]] = edge[2];
        }

        ans.push_back(Graph);
        ans.push_back(weight);

        return ans;
    }

    vector<int> Dijkstra(int n, vector<vector<int>>& Graph, vector<vector<int>>& weight, int root) {
        vector<int> color(n+1, WHITE), dist(n+1, INT_MAX);
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> Q;
                
        dist[root] = 0;
        Q.push(pair<int, int>(0, root));

        while (!Q.empty()){
            int curVertex = Q.top().second; Q.pop();
            for (const int& adjVertex : Graph[curVertex]){;
                if (color[adjVertex] == WHITE &&
                    dist[curVertex] + weight[curVertex][adjVertex] < dist[adjVertex]){
                    dist[adjVertex] = dist[curVertex] + weight[curVertex][adjVertex];
                    Q.push(pair<int, int>(dist[adjVertex], adjVertex));
                }
            }
            color[curVertex] = BLACK;
        }

        return dist;
    }

    int networkDelayTime(vector<vector<int>>& times, int N, int K) {
        vector<vector<vector<int>>> ret = GraphConstruction(N, times);
        vector<vector<int>> Graph = ret[0];
        vector<vector<int>> weight = ret[1];
        
        vector<int> dist = Dijkstra(N, Graph, weight, K);
        int maxDist = -1;

        for (int i = 1; i < N + 1; i++)
            if (maxDist < dist[i])
                maxDist = dist[i];

        return maxDist == INT_MAX ? -1 : maxDist;
    }
};
```

执行用时：164 ms, 在所有 C++ 提交中击败了80.08%的用户

内存消耗：30.2 MB, 在所有 C++ 提交中击败了21.84%的用户