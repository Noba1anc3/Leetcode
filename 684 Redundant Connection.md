In this problem, a tree is an **undirected** graph that is connected and has **no cycles**.

The given input is a graph that started as a tree with N nodes (with distinct values 1, 2, ..., N), with one additional edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of `edges`. Each element of `edges` is a pair `[u, v]` with `u < v`, that represents an **undirected** edge connecting nodes `u` and `v`.

Return an edge that can be removed so that the resulting graph is a tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array. The answer edge `[u, v]` should be in the same format, with `u < v`.

Examples:

```
Example 1:

Input: [[1,2], [1,3], [2,3]]
Output: [2,3]

Explanation: The given undirected graph will be like this:
  1
 / \
2 - 3

Example 2:

Input: [[1,2], [2,3], [3,4], [1,4], [1,5]]
Output: [1,4]

Explanation: The given undirected graph will be like this:
5 - 1 - 2
    |   |
    4 - 3
```

## Solution

### Thinking Phase - I

如果最新一次的并集操作没有带来集合key数量的降低，则该边为冗余边

运行时提示超时，经过分析时间复杂度确实很高。思考后意识到无需统计有效key的数量，只需要判断这次并集的两个元素在合并之前是不是同一key就可以，果然时间复杂度大大降低。

c++

```c++
class Solution {
private:
    vector<int> parent;

public:
    int Find_Set(int x){
        while (parent[x] != x)
            x = parent[x];

        return x;
    }

    void Union(int x, int y){
        int ROOT1 = Find_Set(x), ROOT2 = Find_Set(y);

        if (ROOT1 == ROOT2)
            return;
        
        parent[ROOT2] = ROOT1;
        parent[x] = ROOT1;
        parent[y] = ROOT1;
    }

    int calKey(){
        vector<int> keys;
        for(int& key : parent){
            key = Find_Set(key);
            if (find(keys.begin(), keys.end(), key) == keys.end())
                keys.push_back(key);
        }

        return keys.size();
    }

    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        parent.resize(n);

        for(int i = 0; i < n; i++)
            parent[i] = i;
        
        int keyNum = n;
        for (vector<int>& edge : edges){
            Union(edge[0] - 1, edge[1] - 1);
            int curKey = calKey();
            if (keyNum == curKey)
                return edge;
            keyNum = curKey;
        }
        return {0};
    }
};
```

### Thinking Phase - II

```c++
class Solution {
private:
    vector<int> parent;
    
public:
    int Find_Set(int x){
        while (parent[x] != x)
            x = parent[x];

        return x;
    }

    void Union(int x, int y){
        int ROOT1 = Find_Set(x), ROOT2 = Find_Set(y);

        if (ROOT1 == ROOT2)
            return;
        
        parent[ROOT2] = ROOT1;
        parent[x] = ROOT1;
        parent[y] = ROOT1;
    }

    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        parent.resize(n);

        for(int i = 0; i < n; i++)
            parent[i] = i;

        for (vector<int>& edge : edges){
            if (Find_Set(edge[0] - 1) == Find_Set(edge[1] - 1))
                return edge;
            Union(edge[0] - 1, edge[1] - 1);
        }
        return {0};
    }
};
```

执行用时：8 ms, 在所有 C++ 提交中击败了95.33%的用户

内存消耗：8.5 MB, 在所有 C++ 提交中击败了46.60%的用户