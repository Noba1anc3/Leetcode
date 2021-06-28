

# Language Specialty

## C++

### int

```c++
// c++中对负数求余数不会是正数，需要加上余数

// int -> string
to_string(integer)
```

### struct

```c++
struct Edge{
	int vertex1;
	int vertex2;
    int weight;
};
```

### char

```c++
'c' + 3 = 'f'
'c' - 'a' = 2
'2' - '0' = 2  // 不能用int('2')  会变成ascii码
```

### string

```c++
std::string += std::string / char
std::string.push_back(char)
std::string.pop_back()
std::string.substr(0, std::string.size()-1)
    
std::string -> int
	char* s = std::string.c_str()
	integer I = atoi(char*)
    
int -> std::string
   	std::string s = to_string(integer)
```

### vector

```c++
// vector排序
static bool cmp(type ele1, type ele2) {}
sort(std::vector.begin(), std::vector.end(), cmp)

// 去除vector中的相同元素
sort(std::vector.begin(), std::vector.end())
std::vector.erase(unique(std::vector.begin(), std::vector.end()), std::vector.end())

// 判断vector为空
std::vector.empty()
    
// 清除vector指定位置元素，一般用于回溯
std::vector.erase(std::vector.begin() + i)

// vector进出元素
std::vector.push_back()
std::vector.pop_back()

// vector大小
std::vector.size()

// vector调整大小
std::vector.resize(integer)
    
// vector初始化
std::vector<type> vector2(vector1.begin(), vector1.end())
std::vector<std::vector<type>> vector(n, std::vector<type>())
std::vector<std::vector<type>> vector(n, std::vector<type>(n, INT_MAX))
    
// vector反转
reverse(std::vector.begin(), std::vector.end())
```

### set & unordered_set

![](https://img-blog.csdn.net/20180906204853658?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhbHVvbHVvMjEx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```c++
// set大小
std::set.size()

// 插入set
std::set.insert(ele)

// 遍历set
for (std::set<type>::iterator it = set.begin(); it != set.end(); it++)
	value = *it;

// 查找set
if (std::set.find(element) == std::set.end())
if (std::set.count(element))

// vector转set
std::set(std::vector.begin(), std::vector.end())
```

### map & unordered_map

```c++
std::unordered_map<char, string> mapping = {
	{'2', "abc"}, {}, {}
}
std::unordered_map.at(key)
```

### pair

```c++
std::pair.first
std::pair.second
```

### queue

```c++
std::queue<type> Q;
std::queue.empty()
std::queue.front()
std::queue.pop()
std::queue.push()
```

### priority_queue

c++

```c++
std::priority_queue // default: big root heap
std::priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> Q;
std::priority_queue.top()
std::priority_queue.pop()
std::priority_queue.push()
std::priority_queue.size()
std::priority_queue.empty()
```

python

```python
heapq.heappush(list, element)
heapq.heappop(list)
```

### other

c++

```c++
INT_MAX
max(integer a, integer b)
pow(integer, 2)
NULL
unsigned long long
```

python

```python
list.sort(key = lambda x : x[0])
maximum: float('inf')
```

# Data Structure

## Queue

### Questions

- [BFS](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20BFS.md)
  - 只要队列不空
    - 取出队首
    - 遍历其白色邻接点
      - 改为灰色后压入队列
    - 遍历后将其改为黑色
- [拓扑排序](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Topological%20Sort.md)
  - 将所有入度为0的节点压入队列
  - 只要队列不空
    - 取出队首，压入最终结果
    - 遍历所有指向该节点的源节点
      - 入度减一
      - 如果入度变为0，压入队列
- [绕过不可走的位置，解开转盘锁](https://github.com/Noba1anc3/Leetcode/blob/master/752%20Open%20the%20Lock.md)
  - 将初始状态压入队列，进行BFS搜索，只要队列不空
    - 取出队首，如果是结果，返回步数
    - 如果在不可走位置列表中，continue
    - 计算其4*2个邻居，如果不在走过的位置中，就加入走过的位置，并压入队列

## Stack

### Questions

- 拓扑排序
  - 基于DFS算法
  - 在完成对邻接点的遍历后将节点压入栈
  - 栈顶到栈底的序列即为拓扑排序
- [用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/submissions/)
  - 入队压入1号栈
  - 出队
    - 2号栈不空，弹出2号栈顶
    - 2号栈空，将1号栈逐一弹出并压入2号栈
    - 2号栈不空，弹出2号栈顶
    - 2号栈空，返回错误

## Heap

### Questions

- [最小生成树 - Prim](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Prim.md)
- [单源最短路径 - Dijkstra](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Dijkstra.md)
- [最小的k个数](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-40%20Minimum%20K.md)
- [需要几间会议室](https://github.com/Noba1anc3/Leetcode/blob/master/253%20Meeting%20Rooms%20II.md)
  - 按开始时间排序
  - 遍历会议，按结束时间进入最小堆
    - 开始时间晚于堆顶结束时间则弹出堆顶
    - 无论是否弹出，将当前会议加入堆中
  - 返回堆的大小
- [出租车能否坐下所有乘客](https://github.com/Noba1anc3/Leetcode/edit/master/1094%20Car%20Pooling.md)
  - 简单方法
    - 乘客个数变化只发生在上下车时间
    - 遍历所有路程中间点，如果容量为负，则不能坐下
  - 最小堆
    - 按上客地点排序
    - 遍历行程，按下客地点进入最小堆
      - 上客地点远于堆顶下客地点则弹出堆顶，恢复容量
      - 容量减去当前行程的人数，如果为负，则不能坐下
      - 无论是否弹出，将当前行程加入堆中
- [拼接火柴棍的最小代价](https://github.com/Noba1anc3/Leetcode/blob/master/1167%20Minimum%20Cost%20to%20Connect%20Sticks.md)
  - 基于最小堆的哈夫曼树

## Union-Find Set

### Questions

- [Kruskal生成最小生成树MST](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Kruskal.md)
- [被围绕的区域](https://github.com/Noba1anc3/Leetcode/blob/master/130%20Surrounded%20Regions.md)
  - 对所有‘O’元素生成并查集并合并
  - 对四条边的‘O’元素，找到其并查集的key，加入set
  - 对所有中间‘O’元素，如果不在set中，则改为‘X’
- [岛屿数量](https://github.com/Noba1anc3/Leetcode/blob/master/200%20Number%20of%20Islands.md)
  - 生成每个陆地的并查集
  - 对相邻陆地进行合并
  - 查找最后有几个root
- [无向图中连通分量的个数](https://github.com/Noba1anc3/Leetcode/blob/master/547%20Number%20of%20Provinces.md)
- [寻找树中的冗余边](https://github.com/Noba1anc3/Leetcode/blob/master/684%20Redundant%20Connection.md)
  - 如果一次union操作的两个root key相同，则找到该边
- [账号合并](https://github.com/Noba1anc3/Leetcode/blob/master/721%20Accounts%20Merge.md)
  - 并查集的key为每个账户的下标
  - 建立邮件set和邮件到人名的映射
- [等式可满足性](https://github.com/Noba1anc3/Leetcode/blob/master/990%20Satisfiability%20of%20Equality%20Equations.md)
  - 建立26个字母的并查集
  - 对相等符号连接的字母合并
  - 判断不等符号连接的字母是否是一个key
- [判断一系列**无向边**能否组成一棵树](https://github.com/Noba1anc3/Leetcode/blob/master/261%20Graph%20Valid%20Tree.md)
  - 首先判断边的个数加1是否等于节点的个数
  - 如若并查集合并时根相同，则不能组成一棵树

### Algorithm

```c++
std::vector<int> parent(N, 0), height(N, 0);

void Create_Set(int x) {
    if (parent[x] != 0) return;
    parent[x] = x;
    height[x] = 1;
}

int Find_Set(int x) {
    while (parent[x] != x)
        x = parent[x];
    return x;
}

void Union(int x, int y) {
    int ROOT1 = Find_Set(x);
    int ROOT2 = Find_Set(y);

    if (ROOT1 == ROOT2) return;
    
    if (height[ROOT1] <= height[ROOT2]) {
        parent[ROOT2] = ROOT1;
        height[ROOT2] = height[ROOT1] + 1;
        parent[y] = ROOT1;
        height[y] = height[ROOT1] + 1;
    }
    else{
        parent[ROOT1] = ROOT2;
        height[ROOT1] = height[ROOT2] + 1;
        parent[x] = ROOT2;
        height[x] = height[ROOT2] + 1;
    }
}
```

## Tree

### Questions

- [求二叉树宽度](https://github.com/Noba1anc3/Leetcode/blob/master/662%20Maximum%20Width%20of%20Binary%20Tree.md)

  - ```c++
    pair<TreeNode*, pair<int, unsigned long long>> ROOT(root, level_id)
    ```

  - 若左右儿子不空，则将左右儿子压入队列，高度+1，id分别为 2* id, 2* id + 1

  - 每次进入下一层的时候，更新left的id

- [求二叉树深度](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-55%20Depth%20of%20Binary%20Tree.md)

  - ```c++
    pair<TreeNode*, int>(T, 1)
    ```

  - 若左右儿子不空，则将左右儿子压入队列，高度+1

## Graph

### BFS

要点：dist, pred, color

- 只要队列不空
  - 取出队首
  - 遍历其白色邻接点
    - 改为灰色后压入队列
  - 遍历后将其改为黑色

对于连通图，只需要BFS一个root节点即可；对于非连通图，需要遍历所有白色节点的BFS

```c++
void BFSVisit(Graph G, int vertex){
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
```

#### Questions

[判断一系列**无向边**能否组成一棵树](https://github.com/Noba1anc3/Leetcode/blob/master/261%20Graph%20Valid%20Tree.md)

- 首先判断边的个数加1是否等于节点的个数
  - 少了一定无法存在孤立点
  - 多了一定存在环路
- 从0节点遍历一次DFS后仍然有白色节点，意味着有环路+孤立节点 返回false

### DFS

要点：start，finish，pred，color

- 被访问节点改为灰色
- 遍历邻接点中的白色节点，递归进入该节点
- 访问完邻接点后颜色改黑
- start和finish的触发时间与color变灰及变黑的时间一致

对于连通图，只需要DFS一个root节点即可；对于非连通图，需要遍历所有白色节点的DFS

```c++
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
```

#### Questions

[判断一系列**无向边**能否组成一棵树](https://github.com/Noba1anc3/Leetcode/blob/master/261%20Graph%20Valid%20Tree.md)

- 首先判断边的个数加1是否等于节点的个数
  - 少了一定无法存在孤立点
  - 多了一定存在环路
- 从0节点遍历一次DFS后仍然有白色节点，意味着有环路+孤立节点 返回false

### SCC

DFS Based

- 求所有SCCs
  - 建图
  - 反转图
  - 求DFS序列
  - 反转DFS序列
  - 根据反转DFS序列在图上进行DFS，得到SCCs
- 只需要SCC个数
  - DFS遇到WHITE节点就+1
  - 并查集

### Prim

Priority Queue Based

[Prim](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Prim.md)

- 构建图和权重vector时要将边的两侧都赋值
- 基于pair<key, vertex>构建最小堆（优先队列）
- 只要队列不空
  - 弹出队顶
  - 对其每个白色邻接点，如果边权重小于邻接点的key，则修改邻接点key
  - 将邻接点的pair推进队列
  - 队顶元素改为黑色

### Kruskal



### Dijkstra

Priority Queue Based

[Dijkstra](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Dijkstra.md)

- 基于pair<dist, vertex>构建最小堆（优先队列）
- 只要队列不空
  - 弹出队顶
  - 对其每个白色邻接点，如果边权重加队顶元素距离小于邻接点的距离，则修改邻接点的距离
  - 将邻接点的pair推进队列
  - 队顶元素改为黑色

### Topological Sort

[Algorithm](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Topological%20Sort.md)

#### Queue Based

- 构建反向图和节点入度表
- 将所有入度为0的节点压入队列
- 只要队列不空
  - 取出队首，压入最终结果
  - 遍历所有指向该节点的源节点
    - 入度减一
    - 如果入度变为0，压入队列

```c++
std::vector<int> Topological_Sort(int n, std::vector<std::vector<int>>& edges) {
    std::vector<std::vector<int>> GraphReverse(n, std::vector<int>());
    std::vector<int> InDegree(n, 0);
    std::vector<int> Topological;
    std::queue<int> Q;

    for (const std::vector<int>& edge : edges){
        GraphReverse[edge[1]].push_back(edge[0]);
        InDegree[edge[0]]++;
    }

    for (int i = 0; i < n; i++)
        if (InDegree[i] == 0)
            Q.push(i);

    while (!Q.empty()){
        int vertex = Q.front();
        Q.pop();
        Topological.push_back(vertex);
        for (const int& adjVertex : GraphReverse[vertex]){
            InDegree[adjVertex]--;
            if (InDegree[adjVertex] == 0)
                Q.push(adjVertex);
        }
    }

    return Topological;
}
```

#### DFS & Stack Based

- 需要加入有环的判断，否则算法停不下来
- 基于DFS算法
- 在完成对邻接点的遍历后，将源节点压入栈
- 最后从栈顶到栈底的序列即为DFS序列

```c++
class Solution {
private:
    int WHITE = 0, GRAY = 1, BLACK = 2;
    bool acyclic = true;

    std::vector<int> order;
    std::vector<int> color;
    std::vector<std::vector<int>> Graph;

public:
    void DFSVisit(int vertex){
        color[vertex] = GRAY;
        for (const int& adjVertex : Graph[vertex]){
            if (color[adjVertex] == WHITE){
                DFSVisit(adjVertex);
                if (!acyclic)
                    return;
            }
            else if (color[adjVertex] == GRAY){
                acyclic = false;
                return;
            }
        }
        order.push_back(vertex);
        color[vertex] = BLACK;
    }

    std::vector<int> Topological_Sort(int n, std::vector<std::vector<int>>& edges) {
        color.resize(n);
        Graph.resize(n);

        for (const std::vector<int>& edge : edges)
            Graph[edge[0]].push_back(edge[1]);

        for (int root = 0; root < n && acyclic; root++)
            if (color[root] == WHITE)
                DFSVisit(root);

        if (!acyclic)
            return {};

        reverse(order.begin(), order.end());
        return order;
    }
};
```

#### Questions

- [给定课程学习先决条件，判断能否可行](https://github.com/Noba1anc3/Leetcode/blob/master/207%20Course%20Schedule.md)
  - 拓扑排序序列长度是否等于节点个数
  - DFS判断是否有环
- [给定课程学习先决条件，返回拓扑序列](https://github.com/Noba1anc3/Leetcode/blob/master/210%20Course%20Schedule%20II.md)
  - 拓扑排序
  - DFS

# Algorithm

## BFS

### Questions

- [绕过不可走的位置，解开转盘锁](https://github.com/Noba1anc3/Leetcode/blob/master/752%20Open%20the%20Lock.md)
  - 将初始状态压入队列，进行BFS搜索，只要队列不空
    - 取出队首，如果是结果，返回步数
    - 如果在不可走位置列表中，continue
    - 计算其4*2个邻居，如果不在走过的位置中，就加入走过的位置，并压入队列

## DFS

### Questions

- 岛屿数量
  - 先行后列遍历每个点，如果是岛屿就开始dfs，结束时岛屿个数+1
  - dfs时判断该格子出界或非1返回，不返回则修改该格子为2
- 最大岛屿面积
  - dfs时判断该格子出界或非1返回0，不返回则修改该格子为2
  - dfs的递推公式为 1 + ... + ... + ... + ...
- 填海造陆
  - 计算出每个岛屿的面积，将格子修改为面积下标，并创建面积数组存储下标对应的面积
  - 遍历海洋格子，用set存周围陆地格子的面积数组下标，对下标对应的面积求和
- 岛屿周长
  - 先行后列遍历每个点，如果是岛屿就开始dfs
  - DFS时判断该格子出界或为海洋返回1，遍历过的格子返回0，不返回则修改为2
  - 四方向遍历求和
- 判断回路
  - 在遍历邻接点时，如果是灰色节点代表有回路

### Algorithm

[DFS](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20DFS.md)

要点：start，finish，pred，color

- 刚访问时color改灰，访问完相邻节点后color改黑
- start和finish的触发时间与color变灰及变黑的时间一致
- 遍历邻接点时，对于白色结点，将邻接点的pred设为本节点

对于连通图，只需要DFS一个root节点即可；对于非连通图，需要遍历所有白色节点的DFS

## Backtrack

### Questions

- 手机打字的可能组合
- 生成括号
- 填数独
- 可以无限使用，数组内元素组成一个和
- 一个数只能用一次，数组内元素组成一个和
- 集合内元素的全排列
- 集合内元素的unique全排列
  - 如果同一层当前元素和上一个元素一样，就continue
- 1到N元素的第K个全排列
  - 计算N-1的阶乘，得到第K个全排列的首数字和其是剩余数字的第几个全排列
  - 对剩余数字回溯计算全排列
  - 直接找到指定枝叶
- 1到N元素的由k个元素构成的组合
- 求集合的所有子集
  - 将回溯过程中所有子过程的res都加到ans中
  - 最后加空集合{}
- 求无重复元素的集合的所有子集
- 求集合的所有子集
  - 加一步排序
  - 对同一层，该元素与上一个元素相同的情况，进行剪枝
- N皇后
  - 放之前检查（当前位置为0且不会被攻击）
  - 落子
  - 退子
- 还原ip地址
  - 对切成的段数和当前下标进行回溯
    - 如果段数为4或下标到字符串长度则返回
      - 如果两个条件都满足，将该ip添加到结果当中
    - 遍历时从1位到3位
      - 如果index+i超限，返回
      - 如果首字母为0且长度不为1，返回
      - 如果长度为3且index到i的字串超过255，返回
      - ip加该子串
- 大小写全排列
  - 如果是小写字母，改大写
  - 如果是大写字母，改小写
  - 大小写字母回溯结束后，再进行一次回溯，针对原大小写
- 输出1到10的N次方之间的所有数字
  - 从0-9对N位进行回溯

### Algorithm

vector<string> 存所有结果

string 存每次回溯之前的临时结果

控制回溯进程

- 如果每次前进的内容不同（输入法）
  - 指针移动
  - 每次回溯，指针+1
- 如果每次潜近的内容相同（生成括号）
  - 判断总长度是否满足要求即可
  - 每次回溯，判断长度是否达到要求
- 每次进入回溯函数，先判断指针是否到头，如果到头就把临时结果存到所有结果
- push进临时结果 -> 回溯 -> pop临时结果

做题的时候，建议先画**树形图** ，画图能帮助我们想清楚递归结构，想清楚如何剪枝。

在画图的过程中思考清楚：

- 分支如何产生；

- 题目需要的解在哪里？是在叶子结点、还是在非叶子结点、还是在从跟结点到叶子结点的路径？

- 哪些搜索会产生不需要的解？

  如果在浅层就知道这个分支不能产生需要的结果，应该提前剪枝，剪枝的条件是什么，代码怎么写？

### Generate Parenthesis

除回溯函数，要写check函数，判断字符串是否符合要求

回溯剪枝

- 如果左括号个数少于n，生成左括号
- 如果右括号个数少于左括号，生成右括号

### Sudoku

1. 找到当前数独中缺少的数字个数
2. 根据缺少个数进行回溯
   1. 对行列进行遍历，找到每一个缺数的位置
   2. 遍历1-9，如果该位置可以被填入数字，填入数字，缺少个数-1
   3. 进入下一层，递归
   4. 退出时，如果缺少个数为0，返回
   5. 否则，将该位置的数字去掉，缺少个数+1

### Combination Sum

回溯法讲究还原场景，无论是否得到一个tmp_ans，都需要在加入到ans之后还原现场

无论是非法结果还是得到了一个答案，都需要return

## Dynamic Programming

### Questions

- [求组成数字需要的最少完全平方数](https://github.com/Noba1anc3/Leetcode/blob/master/279%20Perfect%20Squares.md)

  - 外层遍历数字，内层遍历平方数列表

  - ```c++
    dp[i] = min(dp[i], dp[i - square] + 1)
    ```

## Greedy

### Questions

- [求组成数字需要的最少完全平方数](https://github.com/Noba1anc3/Leetcode/blob/master/279%20Perfect%20Squares.md)

  - DFS Based

    - 对组成数字的完全平方数由小到大进行贪心

    - 递归函数的返回条件为需要一个数时，该数在完全平方数列表中

    - ```c++
      canDivide(n, count) -> canDivide(n - square, count - 1);
      ```

  - BFS Based

    - 根据贪心算法的思想，先遍历完同级元素后再遍历下一节是更合理的做法
    - 只要队列不空，迭代其中的元素，检查其是否是完全平方数，是则直接返回，不是则减去完全平方数，得到新余数，添加到队列当中，以进行下一层迭代

# Useful method

## Judge valid parenthesis

```c++
bool check(std::string& ans){
    int balance = 0;
    for (char c : ans){
        if (c == '(') balance++;
        else balance--;
        if (balance < 0) return false;
    }
    return balance == 0;
}
```
