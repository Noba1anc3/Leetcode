## C++

### struct

```c++
struct Edge{
	int vertex1;
	int vertex2;
    int weight;
};
```

### char

```
'c' + 3 = 'f'
'c' - 'a' = 2
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
```

### map & unordered_map

```c++
std::unordered_map<char, string> mapping = {
	{'2', "abc"}, {}, {}
}
std::unordered_map.at(key)
```

### priority_queue

```c++
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> Q;
```

### other

```c++
INT_MAX
max(integer a, integer b)
NULL
```

## Graph

### SCC

- 求所有SCCs
  - 建图
  - 反转图
  - 求DFS序列
  - 反转DFS序列
  - 根据反转DFS序列在图上进行DFS，得到SCCs
- 只需要SCC个数
  - DFS遇到WHITE节点就+1
  - 并查集

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

## Useful method

### Judge valid parenthesis

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

