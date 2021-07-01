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
  - 将所有入度为0的结点压入队列
  - 只要队列不空
    - 取出队首，放入拓扑排序序列结果
    - 遍历所有该结点指向的结点
      - 入度减一
      - 如果入度变为0，压入队列
- [**绕过不可走的位置，解开转盘锁**](https://github.com/Noba1anc3/Leetcode/blob/master/752%20Open%20the%20Lock.md)
  - 将初始状态压入队列，进行BFS搜索。只要队列不空，取出队首
    - 如果是结果，返回步数
    - 如果在不可走位置列表中，continue
    - 计算其4*2个邻居，如果不在走过的位置中，就加入走过的位置，并压入队列

## Stack

### Questions

- [拓扑排序](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Topological%20Sort.md)
  - **基于DFS算法**
  - 在完成对邻接点的遍历后将结点压入栈
  - 栈顶到栈底的序列即为拓扑排序
- [**用两个栈实现队列**](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/submissions/)
  - 入队
    - 压入1号栈
  - 出队
    - 2号栈不空，弹出2号栈顶
    - 2号栈空，将1号栈逐一弹出并压入2号栈
    - 2号栈不空，弹出2号栈顶
    - 2号栈空，返回错误
- [判断是否为有效的括号串](https://github.com/Noba1anc3/Leetcode/blob/master/020%20Valid%20Parentheses.md)
  - 长度为奇数可直接返回错误
  - 构建右括号到左括号的哈希映射以加速
  - 遇到右括号
    - 栈空或栈顶和右括号对应的左括号不一致则返回错误
    - 弹出栈顶
  - 遇到左括号
    - 压入栈
  - 返回栈是否为空
- [**二叉树中序遍历**](https://github.com/Noba1anc3/Leetcode/blob/master/094%20Binary%20Tree%20Inorder%20Traversal.md)
  - 将白色根结点压入栈中
  - 只要栈不空，每次弹出栈顶，如果是空指针则continue
  - 如果是白色，依次将白色右结点，灰色本结点与白色左结点压入栈中
  - 如果是灰色，将结点值加到中序序列中
- [二叉树前序遍历](https://github.com/Noba1anc3/Leetcode/blob/master/144%20Binary%20Tree%20Preorder%20Traversal.md)
- [二叉树后序遍历](https://github.com/Noba1anc3/Leetcode/blob/master/145%20Binary%20Tree%20Postorder%20Traversal.md)
- [判断是否是回文字符串](https://github.com/Noba1anc3/Leetcode/blob/master/125%20Valid%20Palindrome.md)
  - 遍历两次字符串
  - 第一次遍历时，将所有字母数字压入栈中
  - 第二次遍历时，比较当前元素与栈顶元素，如果相同则弹出
- [求逆波兰表达式的值](http://github.com/Noba1anc3/Leetcode/edit/master/150%20Evaluate%20Reverse%20Polish%20Notation.md)
  - 遍历字符串
  - 如果是数字就压入栈中
  - 如果是运算符就取出栈顶和次栈顶，按运算符运算后压入栈中
- [**最小栈**](https://github.com/Noba1anc3/Leetcode/blob/master/155%20Min%20Stack.md)
  - A栈存储数字，B栈存储对应数字的最小值，初始化为最大值
  - 压栈时，A栈压数字，B栈压入 数字与B栈栈顶的最小值
  - 弹栈时，两栈同步弹出
- [**根据天气列表，得出每一天需要等几天能等来更暖和的天气**](https://github.com/Noba1anc3/Leetcode/blob/master/739%20Daily%20Temperatures.md)
  - 维护一个存储下标的单调栈，栈底到栈顶的下标对应温度依次递减
  - 如果一个下标在单调栈中，说明尚未找到更高天气的下标
  - 正向遍历温度列表。对于温度列表中的每个元素 `T[i]`
    - 如果栈不为空，则比较栈顶元素 `prevIndex` 对应的温度 `T[prevIndex]` 和当前温度 `T[i]`
    - 如果 `T[i] > T[prevIndex]`，则将 `prevIndex` 移除，并将 `prevIndex` 对应的等待天数赋为 `i - prevIndex`
    - 重复上述操作直到栈为空或者栈顶元素对应的温度大于等于当前温度
    - 将 `i` 进栈

## Heap

### Questions

- [最小生成树 - Prim](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Prim.md)
- [单源最短路径 - Dijkstra](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Dijkstra.md)
- [**最小的k个数**](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-40%20Minimum%20K.md)
- [**需要几间会议室**](https://github.com/Noba1anc3/Leetcode/blob/master/253%20Meeting%20Rooms%20II.md)
  - 按开始时间排序
  - 遍历会议，按结束时间进入最小堆
    - 开始时间晚于堆顶结束时间则弹出堆顶
    - 无论是否弹出，将当前会议加入堆中
  - 返回堆的大小
- [全网收到信号需要的时间](https://github.com/Noba1anc3/Leetcode/blob/master/743%20Network%20Delay%20Time.md)
- [出租车能否坐下所有乘客](https://github.com/Noba1anc3/Leetcode/edit/master/1094%20Car%20Pooling.md)
  - **简单方法**
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

## Map

### Questions

- [返回列表中两下标，使对应元素之和等于指定target](https://github.com/Noba1anc3/Leetcode/blob/master/001%20Two%20Sum.md)

  - 第一次遍历：构建数字到下标的倒排映射
  - 第二次遍历：如果target - i是哈希表的key且value不等于i，则返回i，j
  - 如果是重复元素，加到哈希表时后一次出现的元素会覆盖前面的写入，因此不会有问题

- [九键打字的字母组合](https://github.com/Noba1anc3/Leetcode/blob/master/017%20Letter%20Combination%20of%20a%20Phone%20Number.md)

  - 构建从数字到字符串的映射

- [判断是否为有效括号串](https://github.com/Noba1anc3/Leetcode/blob/master/020%20Valid%20Parentheses.md)

  - 构建从右括号到左括号的映射

- [**二叉树前中序恢复建树**](https://github.com/Noba1anc3/Leetcode/blob/master/105%20Construct%20Binary%20Tree%20from%20Preorder%20and%20Inorder.md)

  - 构建中序序列的值到下标的倒排映射

  - 递归构建左右子树

    - 如果前序序列左下标超过右下标，返回nullptr

    - ```c++
      TreeNode* root = new TreeNode(preorder[preorder_root])
      ```

    - 用根据倒排得到的中序root减去中序左下标，得到左子树元素个数left_subtree_size

    - 递归构建左子树（pre_left + 1, pre_left + left_subtree_size, in_left, in_root - 1)

    - 递归构建右子树（pre_left + left_subtree_size + 1, pre_right, in_root + 1, in_right)

- [求数组中出现次数超过一半的数字](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-39%20Majority%20Element.md)

  - 计算数字需要出现的最少次数
    - 数组长度为偶数：半长
    - 数组长度为奇数：半长+1
  - 构建数字到出现个数的映射

- [判断两个字符串是否为字母异位词]()

  - 如果长度不相等，直接返回
  - 构建字母与其出现次数的映射
  - 遍历两个字符串，**A串的字母次数增长，B串的字母次数下降**
  - 遍历映射表，如值不为0，返回错误

## Set

一般使用基于哈希表的无序的unordered_set, 其作用在于有效降低查找时的时间复杂度。O(n) -> O(1)

### Questions

- [**最长连续序列**](https://github.com/Noba1anc3/Leetcode/blob/master/128%20Longest%20Consecutive%20Sequence.md)

  - 用unordered_set存储vector所有元素(去重 + 降低搜索时间复杂度)

  - 遍历set中每个元素，如果**找不到其前一个元素**（若能找到，这次计算将是冗余的）

    - ```c++
      while (nums_set.count(++curNum)) curLength++;
      ```

- [**求两个数组的交集**](https://github.com/Noba1anc3/Leetcode/blob/master/349%20Intersection%20of%20Two%20Arrays.md)

  - 遍历两数组，将其元素分别insert到两个unordered_set当中（为了去重）
  - **遍历小set中的元素**，如果在大set中能找到该元素，则将其加入返回vector当中

- [找出数组当中任意一个重复数字](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-03%20Duplicate%20num%20in%20array.md)

- [找出数组中唯二只出现了一次的数字，其他数字均出现两次](http://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-56-I%20The%20number%20of%20occurrences%20of%20a%20number%20in%20an%20array.md)

  - 遍历数组
  - 如果set中有该数字，删除该数字
  - 如果set中没有该数字，插入该数字

## Union-Find Set

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

### Questions

- [Kruskal生成最小生成树MST](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Kruskal.md)
- [**填充被围绕的区域**](https://github.com/Noba1anc3/Leetcode/blob/master/130%20Surrounded%20Regions.md)
  - 对所有‘O’元素生成并查集并合并
  - 对四条边的‘O’元素，找到其并查集的key，加入set
  - 对所有中间‘O’元素，如果不在set中，则改为‘X’
- [**岛屿数量**](https://github.com/Noba1anc3/Leetcode/blob/master/200%20Number%20of%20Islands.md)
  - 生成每个陆地的并查集
  - 对相邻陆地进行合并
  - 查找最后有几个root
- [无向图中连通分量的个数](https://github.com/Noba1anc3/Leetcode/blob/master/323%20Number%20of%20Connected%20Components%20in%20an%20Undirected%20Graph.md)
- [省份的个数](https://github.com/Noba1anc3/Leetcode/blob/master/547%20Number%20of%20Provinces.md)
  - 与上题一致
- [寻找树中的冗余边](https://github.com/Noba1anc3/Leetcode/blob/master/684%20Redundant%20Connection.md)
  - 如果**一次union操作的两个root的key相同**，则找到该边
- [账号合并](https://github.com/Noba1anc3/Leetcode/blob/master/721%20Accounts%20Merge.md)
  - 并查集的key为每个账户的下标
  - 建立邮件set和邮件到人名的映射
- [等式可满足性](https://github.com/Noba1anc3/Leetcode/blob/master/990%20Satisfiability%20of%20Equality%20Equations.md)
  - 建立26个字母的并查集
  - 对相等符号连接的字母合并
  - **判断不等符号连接的字母是否是一个key**
- [判断一系列**无向边**能否组成一棵树](https://github.com/Noba1anc3/Leetcode/blob/master/261%20Graph%20Valid%20Tree.md)
  - 首先判断边的个数加1是否等于结点的个数
  - 如若并查集合并时根相同，则不能组成一棵树

## Linked-List

### Questions



## Tree

### Questions

- [求二叉树宽度](https://github.com/Noba1anc3/Leetcode/blob/master/662%20Maximum%20Width%20of%20Binary%20Tree.md)

  - ```c++
    pair<TreeNode*, pair<int, unsigned long long>> ROOT(root, level_id)
    ```

  - 若左右儿子不空，则将左右儿子压入队列，层数+1，id分别为父结点的2倍, 2倍+1

  - **每次进入下一层的时候，更新left的id，用于计算宽度**

- [求二叉树深度](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-55%20Depth%20of%20Binary%20Tree.md)

  - ```c++
    pair<TreeNode*, int>(T, height)
    ```

  - 若左右儿子不空，则将左右儿子压入队列，高度+1

- [**二叉树中序遍历**](https://github.com/Noba1anc3/Leetcode/blob/master/094%20Binary%20Tree%20Inorder%20Traversal.md)

  - 将根结点与白色压入栈中
  - 只要栈不空，每次弹出栈顶，如果是空指针则continue
  - 如果是白色，依次将白色右结点，灰色本结点与白色左结点压入栈中
  - 如果是灰色，将结点值加到中序序列中

- [二叉树前序遍历](https://github.com/Noba1anc3/Leetcode/blob/master/144%20Binary%20Tree%20Preorder%20Traversal.md)

- [二叉树后序遍历](https://github.com/Noba1anc3/Leetcode/blob/master/145%20Binary%20Tree%20Postorder%20Traversal.md)

- [**二叉树前中序恢复建树**](https://github.com/Noba1anc3/Leetcode/blob/master/105%20Construct%20Binary%20Tree%20from%20Preorder%20and%20Inorder.md)

  - 构建中序序列的值到下标的倒排映射map

  - 递归构建左右子树

    - 如果前序序列左下标超过又下标，返回空指针

    - ```c++
      TreeNode* root = new TreeNode(preorder[preorder_root])
      ```

    - 用根据倒排得到的中序root减去中序左下标，得到左子树元素个数

    - 递归构建左子树（pre_left + 1, pre_left + left_subtree_size, in_left, in_root - 1)

    - 递归构建右子树（pre_left + left_subtree_size + 1, pre_right, in_root + 1, in_right)

## Graph

### Breath First Search

#### Algorithm

可以加入到遍历过程中的其他信息：dist, pred, color

- 只要队列不空
  - 取出队首作为当前结点
  - 遍历其白色邻接点
    - 距离：当前结点的距离+1
    - 先驱：设为当前结点
    - 颜色：改为灰色
    - 修改完毕后，压入队列
  - 遍历完成后，将当前结点的颜色改为黑色

对于连通图，只需要BFS一个root结点即可；对于非连通图，需要遍历所有白色结点的BFS.

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

- 首先判断边的个数加1是否等于结点的个数
  - 少了一定无法存在孤立点
  - 多了一定存在环路
- 从0结点遍历一次BFS后仍然有白色结点，意味着有环路+孤立结点 返回false

### Depth First Search

#### Algorithm

可以加入到遍历过程中的其他信息：start，finish，pred，color

- 当前结点颜色修改为灰色
  - 遍历其白色邻接点，递归进入该结点
- 访问完邻结点后，当前结点颜色改黑
- start和finish的触发时间与color变灰及变黑的时间一致

对于连通图，只需要DFS一个root结点即可；对于非连通图，需要遍历所有白色结点的DFS.

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

- [判断一系列**无向边**能否组成一棵树](https://github.com/Noba1anc3/Leetcode/blob/master/261%20Graph%20Valid%20Tree.md)
  - 首先判断边的个数加1是否等于结点的个数
    - 少了一定无法存在孤立点
    - 多了一定存在环路
  - 从0结点遍历一次DFS后仍然有白色结点，意味着有环路+孤立结点 返回false

- [**矩阵中的最长递增路径**](https://github.com/Noba1anc3/Leetcode/blob/master/329%20Longest%20Increasing%20Path%20in%20a%20Metrix.md)

  - 将矩阵看成有向图，相邻单元格之间存在较小值指向较大值的有向边，问题转化为有向图寻找最长路径

  - 用一个矩阵作为DFS结果的缓存矩阵，将计算好的DFS结果保存下来

  - 对每个没有缓存的结点，计算其四方向邻接点的DFS

    - ```c++
      memo[row][col] = max(memo[row][col], dfs(matrix, newRow, newColumn, memo) + 1);
      ```

### Strongly Connected Components

#### Algorithm

> DFS Based

- [求所有SCCs](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20SCC.md)
  - 建图
  - 反转图（边）
  - 求反转图的DFS序列
  - 反转上述DFS序列
  - 根据反转DFS序列，在原图上进行DFS，得到SCCs
- 只需要SCCs的个数
  - DFS遇到WHITE结点就+1
  - 并查集

#### Questions

- [寻找树中的冗余边](https://github.com/Noba1anc3/Leetcode/blob/master/684%20Redundant%20Connection.md)
  - 如果**一次union操作的两个root的key相同**，则找到该边
- [省份的个数](https://github.com/Noba1anc3/Leetcode/blob/master/547%20Number%20of%20Provinces.md)
  - 与上题一致

### Minimum Spanning Tree

#### Algorithm

##### [Prim](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Prim.md)

> Priority Queue Based

- 构建边和权重vector时要将边的两个端点都赋值
- 基于 pair<key, vertex> 构建最小堆（优先队列）
- 只要队列不空
  - 弹出队顶结点作为当前结点
  - 对其每一个 **边权重小于其key**的 **白色**邻接点
    - 修改其key为边权重
    - 将邻接点的pair压入队列
  - 将当前结点的颜色改为黑色

##### [Kruskal](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Kruskal.md)

> Union-Find Set Based

### Single Source Shortest Path

#### [Algorithm - Dijkstra](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Dijkstra.md)

> Priority Queue Based

- 基于pair<dist, vertex>构建最小堆（优先队列）
- 只要队列不空
  - 弹出队顶结点作为当前结点
    - 对其每个 **边权重 + 当前结点的距离 小于其自身距离**的 **白色**邻接点
      - 修改邻接点的距离
      - 将邻接点的pair推进队列
  - 将当前结点的颜色改为黑色

#### Questions

- [无向图中连通分量的个数](https://github.com/Noba1anc3/Leetcode/blob/master/323%20Number%20of%20Connected%20Components%20in%20an%20Undirected%20Graph.md)

- [全网收到信号需要的时间](https://github.com/Noba1anc3/Leetcode/blob/master/743%20Network%20Delay%20Time.md)

### [Topological Sort](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Topological%20Sort.md)

#### Algorithm - Queue Based

- 构建图和结点入度列表
- 将所有入度为0的结点压入队列
- 只要队列不空
  - 取出队首，压入拓扑序列
  - 遍历所有该结点指向的结点
    - 入度减一
    - 如果入度变为0，压入队列

#### Algorithm - Stack Based

- 需要加入有环的判断，否则算法无法停止
- 在完成对邻接点的遍历后，将源结点压入栈
- 最后从栈顶到栈底的序列即为DFS序列

#### Questions

- [给定课程学习先决条件，判断能否可行](https://github.com/Noba1anc3/Leetcode/blob/master/207%20Course%20Schedule.md)
  - 拓扑排序序列长度是否等于结点个数 或
  - DFS判断是否有环
- [给定课程学习先决条件，返回拓扑序列](https://github.com/Noba1anc3/Leetcode/blob/master/210%20Course%20Schedule%20II.md)
  - 拓扑排序 或
  - DFS
- [**矩阵中的最长递增路径**](https://github.com/Noba1anc3/Leetcode/blob/master/329%20Longest%20Increasing%20Path%20in%20a%20Metrix.md)
  - 该问题基于拓扑排序的解决方案与传统拓扑排序不同的地方在于，将**出度为0**的结点入队
  - 首先遍历所有结点，得到出度列表。将所有出度为0的点（边界点，制高点）压入队列。
  - 只要队列不空，逐一遍历处理这一层BFS的结点，每处理一层，level+1，最终返回level层数
  - 对层内每个结点，遍历其四方向的邻结点，如果邻结点小于本结点，将邻结点的出度减1，如果出度减少为0，则压入队列

# Algorithm

## BFS

### Questions

- [绕过不可走的位置，解开转盘锁](https://github.com/Noba1anc3/Leetcode/blob/master/752%20Open%20the%20Lock.md)
  - 将初始状态压入队列，进行BFS搜索。只要队列不空，取出队首。
    - 如果是结果，返回步数
    - 如果在不可走位置列表中，continue
    - 计算其4*2个邻居，如果不在走过的位置中，就加入走过的位置，并压入队列

## DFS

### Algorithm

[DFS](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20DFS.md)

要点：start，finish，pred，color

- 刚访问时color改灰，访问完相邻结点后color改黑
- start和finish的触发时间与color变灰及变黑的时间一致
- 遍历邻接点时，对于白色结点，将邻接点的pred设为本结点

对于连通图，只需要DFS一个root结点即可；对于非连通图，需要遍历所有白色结点的DFS

### Questions

- [岛屿数量](https://github.com/Noba1anc3/Leetcode/blob/master/200%20Number%20of%20Islands.md)
  - 先行后列遍历每个点，如果是岛屿就开始dfs，结束时岛屿个数+1
  - dfs时判断该格子出界或非1返回，不返回则修改该格子为2
- [最大岛屿面积](https://github.com/Noba1anc3/Leetcode/blob/master/695%20Max%20Area%20Of%20Islands.md)
  - dfs时判断该格子出界或非1返回0，不返回则修改该格子为2
  - dfs的递推公式为 1 + ... + ... + ... + ...
- [填海造陆](https://github.com/Noba1anc3/Leetcode/blob/master/827%20Making%20A%20Large%20Island.md)
  - 计算出每个岛屿的面积，将格子修改为面积下标，并创建面积数组存储下标对应的面积
  - 遍历海洋格子，用set存周围陆地格子的面积数组下标，对下标对应的面积求和
- [岛屿周长](https://github.com/Noba1anc3/Leetcode/blob/master/463%20Island%20Perimeter.md)
  - 先行后列遍历每个点，如果是岛屿就开始dfs
  - DFS时判断该格子出界或为海洋返回1，遍历过的格子返回0，不返回则修改为2
  - 四方向遍历求和
- [判断回路]()
  - 在遍历邻接点时，如果是灰色结点代表有回路

## Double Pointer

### Questions

- [验证回文串](https://github.com/Noba1anc3/Leetcode/blob/master/125%20Valid%20Palindrome.md)
  - 两指针分别位于字符串的首尾  while(i < j)
  - 如果i和j不是字母数字，且不会发生错位，则向中间靠拢
  - 如果双指针对应的小写字母一样，i++，j--，否则返回错误
  - 跳出while循环，返回true
- [**求两个数组的交集**](https://github.com/Noba1anc3/Leetcode/blob/master/349%20Intersection%20of%20Two%20Arrays.md)
  - 利用**set自带的排序机制**，将两个列表的元素insert到两个set当中
  - 生成两个迭代器分别在两个set中前进
    - 如果迭代器1的值小于迭代器2的值，迭代器1++
    - 反之，迭代器2++
    - 如果二者相等，将值加入交集中，两迭代器均++

## Backtrack

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

### Questions

- [手机打字的可能组合](https://github.com/Noba1anc3/Leetcode/blob/master/017%20Letter%20Combination%20of%20a%20Phone%20Number.md)

- [生成括号](https://github.com/Noba1anc3/Leetcode/blob/master/022%20Generate%20Parenthesis.md)

  - 除回溯函数，要写check函数，判断字符串是否符合要求

    回溯剪枝

    - 如果左括号个数少于n，生成左括号
    - 如果右括号个数少于左括号，生成右括号

- [填数独](https://github.com/Noba1anc3/Leetcode/blob/master/037%20Sudoku%20Solver.md)

  - 找到当前数独中缺少的数字个数
  - 根据缺少个数进行回溯
    1. 对行列进行遍历，找到每一个缺数的位置
    2. 遍历1-9，如果该位置可以被填入数字，填入数字，缺少个数-1
    3. 进入下一层，递归
    4. 退出时，如果缺少个数为0，返回
    5. 否则，将该位置的数字去掉，缺少个数+1

- [可以无限使用，数组内元素组成一个和](https://github.com/Noba1anc3/Leetcode/blob/master/039%20Combination%20Sum.md)

  - 回溯法讲究还原场景，无论是否得到一个tmp_ans，都需要在加入到ans之后还原现场
  - 无论是非法结果还是得到了一个答案，都需要return

- [一个数只能用一次，数组内元素组成一个和](https://github.com/Noba1anc3/Leetcode/blob/master/040%20Combination%20Sum%20II.md)

- [集合内元素的全排列](https://github.com/Noba1anc3/Leetcode/blob/master/046%20Permutations.md)

- [集合内元素的unique全排列](https://github.com/Noba1anc3/Leetcode/blob/master/047%20Permutation%20II.md)

  - 如果同一层当前元素和上一个元素一样，就continue

- [1到N元素的第K个全排列](https://github.com/Noba1anc3/Leetcode/blob/master/060%20Permutation%20Sequence.md)

  - 计算N-1的阶乘，得到第K个全排列的首数字和其是剩余数字的第几个全排列
  - 对剩余数字回溯计算全排列
  - 直接找到指定枝叶

- [1到N当中的数字 由k个元素构成的所有组合](https://github.com/Noba1anc3/Leetcode/blob/master/077%20Combinations.md)

- [求集合的所有子集](https://github.com/Noba1anc3/Leetcode/blob/master/078%20Subsets.md)

  - 将回溯过程中所有子过程的res都加到ans中
  - 最后加空集合{}

- [求无重复元素的集合的所有子集](https://github.com/Noba1anc3/Leetcode/blob/master/078%20Subsets.md)

- [求集合的所有子集](https://github.com/Noba1anc3/Leetcode/blob/master/090%20Subsets%20II.md)

  - 加一步排序
  - 对同一层，该元素与上一个元素相同的情况，进行剪枝

- [N皇后](https://github.com/Noba1anc3/Leetcode/blob/master/051%20N-Queens.md)

  - 放之前检查（当前位置为0且不会被攻击）
  - 落子
  - 退子

- [还原ip地址](https://github.com/Noba1anc3/Leetcode/blob/master/093%20Restore%20IP%20Addresses.md)

  - 对切成的段数和当前下标进行回溯
    - 如果段数为4或下标到字符串长度则返回
      - 如果两个条件都满足，将该ip添加到结果当中
    - 遍历时从1位到3位
      - 如果index+i超限，返回
      - 如果首字母为0且长度不为1，返回
      - 如果长度为3且index到i的字串超过255，返回
      - ip加该子串

- [大小写全排列](https://github.com/Noba1anc3/Leetcode/blob/master/784%20Letter%20Case%20Permutation.md)

  - 如果是小写字母，改大写
  - 如果是大写字母，改小写
  - 大小写字母回溯结束后，再进行一次回溯，针对原大小写

- [输出1到10的N次方之间的所有数字](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-17%20Print%20N-bit%20Numbers.md)

  - 从0-9对N位进行回溯

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

## Math

### Questions

- [计算数组中出现次数超过一半的数字]()
  - 首先设定主导数字和票数均为0
  - 遍历所有数字
    - 如果当前票数为0，修改主导数字为当前被遍历到的数字
    - 无论票数是否为0，如果主导数字与当前被遍历的数字相同则票数+1，否则-1
- [找出数组中唯二只出现了一次的数字，其他数字均出现两次](http://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-56-I%20The%20number%20of%20occurrences%20of%20a%20number%20in%20an%20array.md)
  - 用0与数组所有元素逐一异或 ^
  - 用1与异或结果求与 &，若结果不为1，则1向左移一位 << 1, 直到结果为1
  - 按照这一位对数字进行分类，值为1的在一组，值为0的在一组
  - 组内分别计算异或，两组异或的结果就是两个只出现了一次的数字

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

int('a') = 97

// 判断是否是字母或数字
isalnum(char)

// 判断是否是数字
isdigit(char)
 
// 判断是否是字母
isalpha(char)
    
// 转成小写字母
tolower(char)
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

std::string -> int
integer I = stoi(string)
    
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

// 删除set中元素
std::set.erase(ele)

// 遍历set
for (std::set<type>::iterator it = set.begin(); it != set.end(); it++)
    value = *it;

// 查找set  两种方法时间复杂度相当
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

// 根据key查找value
std::unordered_map.at(key)
    
// 查找是否有某个key
std::unordered_map.find(key) != std::unordered_map.end()
```

### pair

```c++
std::pair.first
std::pair.second
make_pair(value1, value2)
```

### stack

```
std::stack<type> S;
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

### iterator

```c++
// 生成迭代器
std::set<int>::iterator iter = set.begin()

// 遍历迭代器
while (iter != set.end())

// 取值
*iter

// 迭代器前移
iter++  // 不能用iter+=1
```

### other

```c++
// int 最大值
INT_MAX

// a和b两个整型的最大值
max(integer a, integer b)

// 求幂
pow(integer, 2)

// 空与空指针
NULL
nullptr

// 无符号长长型
unsigned long long
```

## python

### list

```python
# 按照元素的首元素进行排序
list.sort(key = lambda x : x[0])

# 寻找list中有几个count元素
list.count(element)

# 寻找list中，从index下标开始，value所在的下标
list.index(value, index)
```

### dict

```python
# 根据key取value
value = dict.get(key)

# 解决key不存在时的异常
dicts = collections.defaultdict(int) # 括号内为value类型

# 取key, value
dict.keys()
dict.values()
```

### priority_queue

```python
heapq.heappush(list, element)
heapq.heappop(list)
```

### other

```python
maximum: float('inf') / math.inf
```

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
