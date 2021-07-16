# Questions List

[Questions](https://github.com/Noba1anc3/Leetcode/blob/master/Questions.md)

# Recruit Information

[Recruit Information](https://github.com/Noba1anc3/Leetcode/blob/master/Recruit.md)

# Data Structure

## Queue

> 基于队列的算法最重要的就是[广度优先搜索](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20BFS.md)和基于BFS的[拓扑排序](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Topological%20Sort.md)

### Questions

#### Basics

- [**279. 求组成数字需要的最少完全平方数**](https://github.com/Noba1anc3/Leetcode/blob/master/279%20Perfect%20Squares.md)
  - 因要找到最少的完全平方数，根据贪心算法思想，遍历完同层元素后再遍历下一层是合理的做法
  - 只要队列不空，迭代其中的元素，检查其是否是完全平方数，是则直接返回，不是则减去完全平方数，得到新余数，添加到队列当中，以进行下一层遍历

- [**752. 绕开禁区解锁**](https://github.com/Noba1anc3/Leetcode/blob/master/752%20Open%20the%20Lock.md)
  - 将初始状态压入队列，进行BFS搜索。只要队列不空，取出队首
    - 如果是结果，返回步数
    - 如果在不可走位置列表中，continue
    - 计算其4*2个邻居，如果不在走过的位置中，就加入走过的位置，并压入队列

#### Tree

- [261. 判断一系列**无向边**能否组成一棵树](https://github.com/Noba1anc3/Leetcode/blob/master/261%20Graph%20Valid%20Tree.md)

- [**662. 二叉树的最大宽度**](https://github.com/Noba1anc3/Leetcode/blob/master/662%20Maximum%20Width%20of%20Binary%20Tree.md)
- [**剑指Offer-55. 二叉树的深度**](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-55%20Depth%20of%20Binary%20Tree.md)

#### Graph

- [207. 给定课程学习先决条件，判断是否可行](https://github.com/Noba1anc3/Leetcode/blob/master/207%20Course%20Schedule.md)
- [210. 给定课程学习先决条件，返回拓扑序列](https://github.com/Noba1anc3/Leetcode/blob/master/210%20Course%20Schedule%20II.md)
- [**329. 矩阵中的最长递增路径**](https://github.com/Noba1anc3/Leetcode/blob/master/329%20Longest%20Increasing%20Path%20in%20a%20Metrix.md)

## Stack

### Questions

#### Stack-self

- [**155. 最小栈**](https://github.com/Noba1anc3/Leetcode/blob/master/155%20Min%20Stack.md)
  - A栈存储数字，B栈存储对应数字的最小值，初始化为最大值
  - 压栈时，A栈压数字，B栈压入 数字与B栈栈顶的最小值
  - 弹栈时，两栈同步弹出
- [**剑指Offer-09. 用两个栈实现队列**](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/submissions/)
  - 入队
    - 压入1号栈
  - 出队
    - 2号栈不空，弹出2号栈顶
    - 2号栈空，将1号栈逐一弹出并压入2号栈
    - 2号栈不空，弹出2号栈顶
    - 2号栈空，返回错误

#### Monotonous Stack

- [**739. 根据天气列表，得出每一天需要等几天能等来更暖和的天气**](https://github.com/Noba1anc3/Leetcode/blob/master/739%20Daily%20Temperatures.md)
  - 维护一个存储下标的单调栈，栈底到栈顶的下标对应温度依次递减
  - 如果一个下标在单调栈中，说明尚未找到更高天气的下标
  - 正向遍历温度列表。对于温度列表中的每个元素 `T[i]`
    - 如果栈不为空，则比较栈顶元素 `prevIndex` 对应的温度 `T[prevIndex]` 和当前温度 `T[i]`
    - 如果 `T[i] > T[prevIndex]`，则将 `prevIndex` 移除，并将 `prevIndex` 对应的等待天数赋为 `i - prevIndex`
    - 重复上述操作直到栈为空或者栈顶元素对应的温度大于等于当前温度
    - 将 `i` 进栈

#### String

- [020. 判断是否为有效的括号串](https://github.com/Noba1anc3/Leetcode/blob/master/020%20Valid%20Parentheses.md)
  - 长度为奇数可直接返回错误
  - 构建右括号到左括号的哈希映射以加速
  - 遇到右括号
    - 栈空或栈顶和右括号对应的左括号不一致则返回错误
    - 弹出栈顶
  - 遇到左括号
    - 压入栈
  - 返回栈是否为空
- [125. 判断是否是回文字符串](https://github.com/Noba1anc3/Leetcode/blob/master/125%20Valid%20Palindrome.md)
  - 遍历两次字符串
  - 第一次遍历时，将所有字母数字压入栈中
  - 第二次遍历时，比较当前元素与栈顶元素，如果相同则弹出
- [150. 求逆波兰表达式的值](http://github.com/Noba1anc3/Leetcode/edit/master/150%20Evaluate%20Reverse%20Polish%20Notation.md)
  - 遍历字符串
  - 如果是数字就压入栈中
  - 如果是运算符就取出栈顶和次栈顶，按运算符运算后压入栈中

#### Tree

- [**094. 二叉树中序遍历**](https://github.com/Noba1anc3/Leetcode/blob/master/094%20Binary%20Tree%20Inorder%20Traversal.md)
  - 将白色根结点压入栈中
  - 只要栈不空，每次弹出栈顶，如果是空指针则continue
  - 如果是白色，依次将白色右结点，灰色本结点与白色左结点压入栈中
  - 如果是灰色，将结点值加到中序序列中
- [144. 二叉树前序遍历](https://github.com/Noba1anc3/Leetcode/blob/master/144%20Binary%20Tree%20Preorder%20Traversal.md)
- [145, 二叉树后序遍历](https://github.com/Noba1anc3/Leetcode/blob/master/145%20Binary%20Tree%20Postorder%20Traversal.md)

#### Graph

- [拓扑排序](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Topological%20Sort.md)
  - **基于DFS算法**
  - 在完成对邻结点的遍历后将结点压入栈
  - 栈顶到栈底的序列即为拓扑排序
- [210. 给定课程学习先决条件，返回拓扑序列](https://github.com/Noba1anc3/Leetcode/blob/master/210%20Course%20Schedule%20II.md)

## Heap

> 基于最小堆的两个重要的图算法是求最小生成树的[Prim算法](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Prim.md)和求单源最短路径的[Dijkstra算法](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Dijkstra.md)

### Questions

#### Basics

- [**253. 需要几间会议室**](https://github.com/Noba1anc3/Leetcode/blob/master/253%20Meeting%20Rooms%20II.md)
  - 按开始时间排序
  - 遍历会议，按结束时间进入最小堆
    - 开始时间晚于堆顶结束时间则弹出堆顶
    - 无论是否弹出，将当前会议加入堆中
  - 返回堆的大小
- [1094. 出租车能否坐下所有乘客](https://github.com/Noba1anc3/Leetcode/edit/master/1094%20Car%20Pooling.md)
  - **简单方法**
    - 乘客个数变化只发生在上下车时间
    - 遍历所有路程中间点，如果容量为负，则不能坐下
  - 最小堆
    - 按上客地点排序
    - 遍历行程，按下客地点进入最小堆
      - 上客地点远于堆顶下客地点则弹出堆顶，恢复容量
      - 容量减去当前行程的人数，如果为负，则不能坐下
      - 无论是否弹出，将当前行程加入堆中
- [**剑指Offer-40. 最小的k个数**](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-40%20Minimum%20K.md)

#### Tree

- [1167. 拼接火柴棍的最小代价](https://github.com/Noba1anc3/Leetcode/blob/master/1167%20Minimum%20Cost%20to%20Connect%20Sticks.md)
  - 基于最小堆的哈夫曼树

#### Graph

- [743. 全网收到信号需要的时间](https://github.com/Noba1anc3/Leetcode/blob/master/743%20Network%20Delay%20Time.md)

## Map

> 对Map(Hash, Dict) 的使用在于**构建映射关系**，加速搜索过程。

### Questions

#### Integer

- [001. 返回列表中两下标，使对应元素之和等于指定target](https://github.com/Noba1anc3/Leetcode/blob/master/001%20Two%20Sum.md)
  - 第一次遍历：**构建数字到下标的倒排映射**
  - 第二次遍历：如果target - i是哈希表的key且value不等于i，则返回i，j
  - 如果是重复元素，加到哈希表时后一次出现的元素会覆盖前面的写入，因此不会有问题
- [169. 求数组中出现次数超过一半的数字](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-39%20Majority%20Element.md)
  - 计算数字需要出现的最少次数
    - 数组长度为偶数：半长
    - 数组长度为奇数：半长+1
  - **构建数字到出现个数的映射**

#### String

- [017. 九键打字的字母组合](https://github.com/Noba1anc3/Leetcode/blob/master/017%20Letter%20Combination%20of%20a%20Phone%20Number.md)
  - **构建从数字到字符串的映射**
- [020. 判断是否为有效括号串](https://github.com/Noba1anc3/Leetcode/blob/master/020%20Valid%20Parentheses.md)
  - **构建从右括号到左括号的映射**
- [242. 判断两个字符串是否为字母异位词](https://github.com/Noba1anc3/Leetcode/blob/master/242%20Valid%20Anagram.md)
  - 如果长度不相等，直接返回
  - **构建字母与其出现次数的映射**
  - 遍历两个字符串，**A串的字母次数增长，B串的字母次数下降**
  - 遍历映射表，如值不为0，返回错误

#### Tree

- [**105. 二叉树前中序恢复建树**](https://github.com/Noba1anc3/Leetcode/blob/master/105%20Construct%20Binary%20Tree%20from%20Preorder%20and%20Inorder.md)

  - **构建中序序列的值到下标的倒排映射**

  - 递归构建左右子树

    - 如果前序序列左下标超过右下标，返回nullptr

    - ```c++
      TreeNode* root = new TreeNode(preorder[preorder_root])
      ```

    - 用根据倒排得到的中序`root`减去中序左下标，得到左子树元素个数`left_subtree_size`

    - 递归构建左子树`（pre_left + 1, pre_left + left_subtree_size, in_left, in_root - 1)`

    - 递归构建右子树`（pre_left + left_subtree_size + 1, pre_right, in_root + 1, in_right)`

## Set

> 一般使用基于哈希表的无序的unordered_set, 其作用在于
>
> - 有效**降低搜索时间复杂度**：`O(n) -> O(1)`
> - 实现去重
>
> 对于Set这一数据结构，其独特点在于
>
> - 自带**排序**机制
> - 搜索时间复杂度为 `O(log n)`

### Questions

- [**003. 求字符串的无重复字符最长字串长度**](https://github.com/Noba1anc3/Leetcode/blob/master/003%20Longest%20Substring.md)

  - 使用set数据结构的原因
    1. 加速判断当前字符是否在滑动窗口的查找过程
    2. 在需要将滑动窗口左沿右移时，便于删除左沿元素

- [**128. 最长连续序列**](https://github.com/Noba1anc3/Leetcode/blob/master/128%20Longest%20Consecutive%20Sequence.md)

  - 用unordered_set存储vector所有元素(去重 + 降低搜索时间复杂度)

  - 遍历set中每个元素，如果**找不到其前一个元素**（若能找到，这次计算将是冗余的）

    - ```c++
      while (nums_set.count(++curNum)) curLength++;
      ```

- [**349. 求两个数组的交集**](https://github.com/Noba1anc3/Leetcode/blob/master/349%20Intersection%20of%20Two%20Arrays.md)

  - 遍历两数组，将其元素分别insert到两个unordered_set当中（为了去重）
  - **遍历小set中的元素**，如果在大set中能找到该元素，则将其加入返回vector当中

- [剑指Offer-03. 找出数组当中任意一个重复数字](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-03%20Duplicate%20num%20in%20array.md)

- [剑指Offer-56-I. 找出数组中唯二只出现了一次的数字，其他数字均出现两次](http://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-56-I%20The%20number%20of%20occurrences%20of%20a%20number%20in%20an%20array.md)

  - 遍历数组
  - 如果set中有该数字，删除该数字
  - 如果set中没有该数字，插入该数字

## Union-Find Set

![](http://r.photo.store.qq.com/psc?/V50VqFfH2A6OlZ2gWBDL0uxzNK4WmFgm/TmEUgtj9EK6.7V8ajmQrEFy3QO5ubD6pAPAzUY7otgbBuBb34JPwThL1qeVkpgW0Zg2qr*9MnrqkinYmqbYWd4dtlqbc0YpAWVYUGtWGSQ0!/r)

![](http://r.photo.store.qq.com/psc?/V50VqFfH2A6OlZ2gWBDL0uxzNK4WmFgm/TmEUgtj9EK6.7V8ajmQrEKQE0fRjdyuxRXmgfj3VdB.j2m4ewQFksQKXlDuzB8tdv6dLpKYcrfKbB*6h44qPE9OVqbzIIVCkkyHx3FONV*w!/r)

### Implementation

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
        if (height[ROOT1] == height[ROOT2])
            height[ROOT2]++;
        parent[ROOT1] = ROOT2;
    }
    else
        parent[ROOT2] = ROOT1;
}
```

### Questions

#### Basics

- [721. 账号合并](https://github.com/Noba1anc3/Leetcode/blob/master/721%20Accounts%20Merge.md)
  - 并查集的key为每个账户的下标
  - 建立邮件set和邮件到人名的映射
- [990. 等式可满足性](https://github.com/Noba1anc3/Leetcode/blob/master/990%20Satisfiability%20of%20Equality%20Equations.md)
  - 建立26个字母的并查集
  - 对相等符号连接的字母合并
  - **判断不等符号连接的字母是否是一个key**

#### Tree

- [261. 判断一系列**无向边**能否组成一棵树](https://github.com/Noba1anc3/Leetcode/blob/master/261%20Graph%20Valid%20Tree.md)
  - 首先判断边的个数加1是否等于结点的个数
  - 如若并查集合并时根相同，则不能组成一棵树

#### Matrix

- [**130. 填充被围绕的区域**](https://github.com/Noba1anc3/Leetcode/blob/master/130%20Surrounded%20Regions.md)
  - 对所有`'O'`元素生成并查集并合并
  - 对四条边的`'O'`元素，找到其并查集的key，加入set
  - 对所有中间`'O'`元素，如果不在set中，则改为`'X'`
- [**200. 岛屿数量**](https://github.com/Noba1anc3/Leetcode/blob/master/200%20Number%20of%20Islands.md)
  - 生成每个陆地的并查集
  - 对相邻陆地进行合并
  - 查找最后有几个root

#### Graph

- [Kruskal生成最小生成树MST](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Kruskal.md)
- [323. 无向图中连通分量的个数](https://github.com/Noba1anc3/Leetcode/blob/master/323%20Number%20of%20Connected%20Components%20in%20an%20Undirected%20Graph.md)

- [547. 省份的个数](https://github.com/Noba1anc3/Leetcode/blob/master/547%20Number%20of%20Provinces.md)
  - 与上题一致
- [684. 寻找树中的冗余边](https://github.com/Noba1anc3/Leetcode/blob/master/684%20Redundant%20Connection.md)
  - 如果**一次union操作的两个root的key相同**，则找到该边

## Linked-List

### Questions

- [002. 两链表求和](https://github.com/Noba1anc3/Leetcode/blob/master/002%20Add%20Two%20Numbers.md)
  - 每次求和时如果超过10，新结点为求和余10的结果，并将进位设为1；
  - 下次求和时如果有进位，则将求和结果+1
  - 如果做完最终运算，仍有进位，创建新结点并设为1
  - 结果新链表设置一个头节点，一个移动结点。移动结点不断后移，结束后返回头节点。
- [021. 合并两个链表](https://github.com/Noba1anc3/Leetcode/blob/master/021%20Merge%20Two%20Sorted%20Lists.md)
  - 双指针向前推进
  - 双指针迭代结束后，如果`l1`为空，合并链表迭代器的后继指向`l2`，否则指向`l1`
- [**148. 排序链表**](https://github.com/Noba1anc3/Leetcode/blob/master/148%20Sort%20List.md)
  - 自顶向下
    - 通过**快慢指针法**找到链表中点，并在中间断链，返回后半段链表的头指针
    - 通过递归法进行归并排序，合并方法见上题👆
    - 递归函数的返回条件为当前结点或其后继结点为空
  - **自底向上**
    - 外层循环遍历当前子链表的长度`subLength`，从1开始，每次放大2倍（因为后面会合并）
      - 每次进入外循环时，使用`pre`和`cur`双指针用于整理过去和探索未来，分别初始化为链表的头结点指针和有效结点指针
      - 内层循环判断只要cur指针不为空指针即可进入（尚未到达链尾）
        - 使用两次断链方法，得到两个长度为`subLength`的子链表
        - 通过第二次断链，`cur`指针会指向第二个子链表的后继结点
        - 将两部分merge的结果接在`pre`指针的后面，而后更新`pre`指针到当前指针的末尾
- [206. 反转链表](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-24%20Reverse%20LinkedList.md)
  - 基于双指针的局部方向反转
- [剑指Offer-18. 删除链表指定元素](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-18%20Delete%20element%20in%20linkList.md)
  - 首先判断头结点的值是否为指定元素，如是，返回`head->next`
  - 只要头结点的后继结点的值不为指定元素，则不断后移头结点
  - 找到后继结点为指定元素后，将该结点的后继结点赋值为指定元素结点的后继结点

## Tree

### Questions

#### Tree-self

- [261. 判断一系列**无向边**能否组成一棵树](https://github.com/Noba1anc3/Leetcode/blob/master/261%20Graph%20Valid%20Tree.md)

  - 首先判断边的个数加1是否等于结点的个数
  - 如若并查集合并时根相同，则不能组成一棵树

- [**662. 求二叉树宽度**](https://github.com/Noba1anc3/Leetcode/blob/master/662%20Maximum%20Width%20of%20Binary%20Tree.md)

  - ```c++
    pair<TreeNode*, pair<int, unsigned long long>> ROOT(root, level_id)
    ```

  - 若左右儿子不空，则将左右儿子压入队列，层数+1，id分别为父结点的2倍, 2倍+1

  - **每次进入下一层的时候，更新left的id，用于计算宽度**

- [**剑指Offer-55. 求二叉树深度**](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-55%20Depth%20of%20Binary%20Tree.md)

  - ```c++
    pair<TreeNode*, int>(T, height)
    ```

  - 若左右儿子不空，则将左右儿子压入队列，高度+1

#### Traverse

- [**094. 二叉树中序遍历**](https://github.com/Noba1anc3/Leetcode/blob/master/094%20Binary%20Tree%20Inorder%20Traversal.md)

  - 将根结点与白色压入栈中
  - 只要栈不空，每次弹出栈顶，如果是空指针则continue
  - 如果是白色，依次将白色右结点，灰色本结点与白色左结点压入栈中
  - 如果是灰色，将结点值加到中序序列中

- [144. 二叉树前序遍历](https://github.com/Noba1anc3/Leetcode/blob/master/144%20Binary%20Tree%20Preorder%20Traversal.md)

- [145. 二叉树后序遍历](https://github.com/Noba1anc3/Leetcode/blob/master/145%20Binary%20Tree%20Postorder%20Traversal.md)

- [**105. 二叉树前中序恢复建树**](https://github.com/Noba1anc3/Leetcode/blob/master/105%20Construct%20Binary%20Tree%20from%20Preorder%20and%20Inorder.md)

  - 构建中序序列的值到下标的倒排映射map

  - 递归构建左右子树

    - 如果前序序列左下标超过又下标，返回空指针

    - ```c++
      TreeNode* root = new TreeNode(preorder[preorder_root])
      ```

    - 用根据倒排得到的中序root减去中序左下标，得到左子树元素个数

    - 递归构建左子树`（pre_left + 1, pre_left + left_subtree_size, in_left, in_root - 1)`

    - 递归构建右子树`（pre_left + left_subtree_size + 1, pre_right, in_root + 1, in_right)`

#### Huffman Tree

![](http://r.photo.store.qq.com/psc?/V50VqFfH2A6OlZ2gWBDL0uxzNK4WmFgm/TmEUgtj9EK6.7V8ajmQrEIkdCy4oUKtQ*cknIk3uZnMsfHM8thT0qlo2pn3VR0S18LV8YynYANQ1n0NHxm5pZslchCZ4b*Ft*DVtFKnGofA!/r)

- [1167. 拼接火柴棍的最小代价](https://github.com/Noba1anc3/Leetcode/blob/master/1167%20Minimum%20Cost%20to%20Connect%20Sticks.md)
  - 基于最小堆的哈夫曼树

## Graph

### Breath First Search

#### Implementation

可以加入到遍历过程中的其他信息：`dist`, `pred`, `color`

- 只要队列不空
  - 取出队首作为当前结点
  - 遍历其白色邻结点
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

### Depth First Search

#### Implementation

可以加入到遍历过程中的其他信息：`start`，`finish`，`pred`，`color`

- 当前结点颜色修改为灰色
  - 遍历其白色邻结点，递归进入该结点
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

- [**329. 矩阵中的最长递增路径**](https://github.com/Noba1anc3/Leetcode/blob/master/329%20Longest%20Increasing%20Path%20in%20a%20Metrix.md)

  - 将矩阵看成有向图，相邻单元格之间存在较小值指向较大值的有向边，问题转化为有向图寻找最长路径

  - 用一个矩阵作为DFS结果的缓存矩阵，将计算好的DFS结果保存下来

  - 对每个没有缓存的结点，计算其四方向邻结点的DFS

    - ```c++
      memo[row][col] = max(memo[row][col], dfs(matrix, newRow, newColumn, memo) + 1);
      ```

### Strongly Connected Components

#### Implementation

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

- [323. 无向图中连通分量的个数](https://github.com/Noba1anc3/Leetcode/blob/master/323%20Number%20of%20Connected%20Components%20in%20an%20Undirected%20Graph.md)
- [547. 省份的个数](https://github.com/Noba1anc3/Leetcode/blob/master/547%20Number%20of%20Provinces.md)
  - 与上题一致

- [684. 寻找树中的冗余边](https://github.com/Noba1anc3/Leetcode/blob/master/684%20Redundant%20Connection.md)
  - 如果**一次union操作的两个root的key相同**，则找到该边

### Minimum Spanning Tree

![](http://r.photo.store.qq.com/psc?/V50VqFfH2A6OlZ2gWBDL0uxzNK4WmFgm/TmEUgtj9EK6.7V8ajmQrEBQN2l.IIpK8EcEWD.mOcOTQlRK8*V56pNguDzWpnRYLSjqwD5e4fqoHw5RA7GqBWEDUYy1mX7fTtY4CXGZ.NrM!/r)

#### Implementation

##### [Prim](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Prim.md)

> Priority Queue Based

![](http://r.photo.store.qq.com/psc?/V50VqFfH2A6OlZ2gWBDL0uxzNK4WmFgm/TmEUgtj9EK6.7V8ajmQrEKnMEBfDUZvZ2HSunq7PInwYcYSfuuWmgAK9izhyP3rC8sck.mKscowxAwqXMyo5QfzApkuuIIw7kpkgEovJNeU!/r)

- 构建边和权重vector时要将边的两个端点都赋值
- 基于 pair<key, vertex> 构建最小堆（优先队列）
- 只要队列不空
  - 弹出队顶结点作为当前结点
  - 对其每一个 **边权重小于其key**的 **白色**邻结点
    - 修改其key为边权重
    - 将邻结点的pair压入队列
  - 将当前结点的颜色改为黑色

##### [Kruskal](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Kruskal.md)

> Union-Find Set Based

![](http://r.photo.store.qq.com/psc?/V50VqFfH2A6OlZ2gWBDL0uxzNK4WmFgm/TmEUgtj9EK6.7V8ajmQrEIAXtRZjacc5ITbgkTl377nd3ithzbPfIQAlED3LRd8KwpTlf4vOk0xqy1fBINca7odfWrQyhcBBz4pWjWAoI84!/r)

### Single Source Shortest Path

#### Implementation

##### [Dijkstra](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Dijkstra.md)

> Priority Queue Based

![](http://r.photo.store.qq.com/psc?/V50VqFfH2A6OlZ2gWBDL0uxzNK4WmFgm/TmEUgtj9EK6.7V8ajmQrEHphHvi5bErf*H6dfYFOJ1cGugzyC2oZIjXk3ZPKWrfO.aJV*NEbr*izyEwyc6IwjjDFThdrdhwyuNvMGcer*ok!/r)

- 基于 pair<dist, vertex> 构建最小堆（优先队列）
- 只要队列不空
  - 弹出队顶结点作为当前结点
    - 对其每个 **边权重 + 当前结点的距离 小于其自身距离**的 **白色**邻结点
      - 修改邻结点的距离
      - 将邻结点的pair推进队列
  - 将当前结点的颜色改为黑色

##### [Bellman-Ford](https://github.com/Noba1anc3/Leetcode/blob/master/Bellman_Ford.md)

![](http://r.photo.store.qq.com/psc?/V50VqFfH2A6OlZ2gWBDL0uxzNK4WmFgm/TmEUgtj9EK6.7V8ajmQrEITMgCJbeCIg7E2dx2akan5AaI9RojufYfQ0CNadGpIZp.qNbU7SR40pSYgb4uNaiANXc4nZY9gjwtsFCIz1rTE!/r)

检查有向图中是否存在负权重环

#### Questions

- [743. 全网收到信号需要的时间](https://github.com/Noba1anc3/Leetcode/blob/master/743%20Network%20Delay%20Time.md)

### [Topological Sort](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Topological%20Sort.md)

#### Implementation - Queue Based

- 构建图和结点入度列表
- 将所有入度为0的结点压入队列
- 只要队列不空
  - 取出队首，压入拓扑序列
  - 遍历所有该结点指向的结点
    - 入度减一
    - 如果入度变为0，压入队列

#### Implementation - Stack Based

- 需要加入有环的判断，否则算法无法停止
- 在完成对邻结点的遍历后，将源结点压入栈
- 最后从栈顶到栈底的序列即为DFS序列

#### Questions

- [207. 给定课程学习先决条件，判断能否可行](https://github.com/Noba1anc3/Leetcode/blob/master/207%20Course%20Schedule.md)
  - 拓扑排序序列长度是否等于结点个数 或
  - DFS判断是否有环
- [210. 给定课程学习先决条件，返回拓扑序列](https://github.com/Noba1anc3/Leetcode/blob/master/210%20Course%20Schedule%20II.md)
  - 拓扑排序 或
  - DFS
- [**329. 矩阵中的最长递增路径**](https://github.com/Noba1anc3/Leetcode/blob/master/329%20Longest%20Increasing%20Path%20in%20a%20Metrix.md)
  - 该问题基于拓扑排序的解决方案与传统拓扑排序不同的地方在于，将**出度为0**的结点入队
  - 首先遍历所有结点，得到出度列表。将所有出度为0的点（边界点，制高点）压入队列。
  - 只要队列不空，逐一遍历处理这一层BFS的结点，每处理一层，level+1，最终返回level层数
  - 对层内每个结点，遍历其四方向的邻结点，如果邻结点小于本结点，将邻结点的出度减1，如果出度减少为0，则压入队列

# Algorithm

![](http://r.photo.store.qq.com/psc?/fef49446-40e0-48c4-adcc-654c5015022c/TmEUgtj9EK6.7V8ajmQrENac6FN7FhHXvwTiuUGGpM2jqYZVoghyokllKN.D3NIQPHFW9GsRZcII5eP5K8G1.hWnMMvSTt8b2kZ1DVT7yrU!/r)

## BFS

### [Algorithm](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20BFS.md)

可以加入到遍历过程中的其他信息：`dist`, `pred`, `color`

- 只要队列不空
  - 取出队首作为当前结点
  - 遍历其白色邻结点
    - 距离：当前结点的距离+1
    - 先驱：设为当前结点
    - 颜色：改为灰色
    - 修改完毕后，压入队列
  - 遍历完成后，将当前结点的颜色改为黑色

### Questions

- [261. 判断一系列**无向边**能否组成一棵树](https://github.com/Noba1anc3/Leetcode/blob/master/261%20Graph%20Valid%20Tree.md)
  - 首先判断边的个数加1是否等于结点的个数
    - 少了一定无法存在孤立点
    - 多了一定存在环路
  - 从0结点遍历一次BFS后仍然有白色结点，意味着有环路+孤立结点 返回false
- [**279. 求组成数字需要的最少完全平方数**](https://github.com/Noba1anc3/Leetcode/blob/master/279%20Perfect%20Squares.md)
  - 因要找到最少的完全平放数，根据贪心算法思想，遍历完同层元素后再遍历下一层是合理的做法
  - 只要队列不空，迭代其中的元素，检查其是否是完全平方数，是则直接返回，不是则减去完全平方数，得到新余数，添加到队列当中，以进行下一层遍历
- [**329. 矩阵中的最长递增路径**](https://github.com/Noba1anc3/Leetcode/blob/master/329%20Longest%20Increasing%20Path%20in%20a%20Metrix.md)
- [752. 绕开禁区解锁](https://github.com/Noba1anc3/Leetcode/blob/master/752%20Open%20the%20Lock.md)
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
- 遍历邻结点时，对于白色结点，将邻结点的pred设为本结点

对于连通图，只需要DFS一个root结点即可；对于非连通图，需要遍历所有白色结点的DFS

对于有向图，在DFS遍历邻结点时，如果遇到灰色结点代表有回路

### Questions

- [200. 岛屿数量](https://github.com/Noba1anc3/Leetcode/blob/master/200%20Number%20of%20Islands.md)

  - 遍历每个点，如果是岛屿就开始DFS，结束时岛屿个数+1
  - DFS时判断该格子出界或非1返回，如未返回则修改该格子为2

- [261. 判断一系列**无向边**能否组成一棵树](https://github.com/Noba1anc3/Leetcode/blob/master/261%20Graph%20Valid%20Tree.md)

  - 首先判断边的个数加1是否等于结点的个数
    - 少了一定无法存在孤立点
    - 多了一定存在环路
  - 从0结点遍历一次DFS后仍然有白色结点，意味着有环路+孤立结点 返回false

- [**329. 矩阵中的最长递增路径**](https://github.com/Noba1anc3/Leetcode/blob/master/329%20Longest%20Increasing%20Path%20in%20a%20Metrix.md)

  - 将矩阵看成有向图，相邻单元格之间存在较小值指向较大值的有向边，问题转化为有向图寻找最长路径

  - 用一个矩阵作为DFS结果的缓存矩阵，将计算好的DFS结果保存下来

  - 对每个没有缓存的结点，计算其四方向邻结点的DFS

    - ```c++
      memo[row][col] = max(memo[row][col], dfs(matrix, newRow, newColumn, memo) + 1);
      ```

- [463. 岛屿周长](https://github.com/Noba1anc3/Leetcode/blob/master/463%20Island%20Perimeter.md)

  - 遍历每个点，如果是岛屿就开始DFS
  - DFS时判断该格子出界或为海洋返回1，遍历过的格子返回0，如未返回则修改为2
  - 四方向遍历求和

- [547. 省份的个数](https://github.com/Noba1anc3/Leetcode/blob/master/547%20Number%20of%20Provinces.md)

  - 对于所有结点，只要其需要进行DFS计算，省份的个数+1

- [695. 最大岛屿面积](https://github.com/Noba1anc3/Leetcode/blob/master/695%20Max%20Area%20Of%20Islands.md)

  - DFS时判断该格子出界或非1返回0，如未返回则修改该格子为2
  - DFS的递推公式为 1 + ... + ... + ... + ...

- [827. 填海造陆](https://github.com/Noba1anc3/Leetcode/blob/master/827%20Making%20A%20Large%20Island.md)

  - 计算出每个岛屿的面积，将格子修改为面积下标，并创建面积数组存储下标对应的面积
  - 遍历海洋格子，用set存周围陆地格子的面积数组下标，对下标对应的面积求和

## Slide Window

### Questions

- [**003. 求字符串的无重复字符最长字串长度**](https://github.com/Noba1anc3/Leetcode/blob/master/003%20Longest%20Substring.md)
  - 用`left`记录当前滑动窗口的左端点
  - 遍历字符串中的元素
    - 当前字符在set当中：只要该字符还在set当中，将left下标字符删除，left下标+1（窗口左沿右移）
    - 字符不在set当中时，将该字符加入set（窗口右沿右移）
    - 更新最长字串的长度记录为当前最长字串的长度记录和set大小的最大值
- [056. 合并区间](https://github.com/Noba1anc3/Leetcode/blob/master/056%20Merge%20Intervals.md)
  - 对区间列表按起点进行排序
  - 遍历每个区间
    - 如果合并列表为空或合并列表最后一个区间的终点小于当前区间的起点，将该区间加入合并列表
    - 否则，更新合并列表最后一个区间的终点为其与当前区间终点的最大值

## Split-Search

### Questions

- [**004. 寻找两个有序数组的中位数**](https://github.com/Noba1anc3/Leetcode/blob/master/004%20Median%20of%20Two%20Sorted%20Arrays.md)
- [153. 寻找旋转后有序无重复元素数组的最小值](https://github.com/Noba1anc3/Leetcode/blob/master/153%20Find%20Minimum%20in%20Rotated%20Sorted%20Array.md)
  - 如果中间数小于右边的数，把mid赋给right下标（因为mid可能是答案）
  - 反之，把mid+1赋给left下标
  - 最终返回`nums[right]`
- [**154. 寻找旋转后有序有重复元素数组的最小值**](https://github.com/Noba1anc3/Leetcode/blob/master/154%20Find%20Minimum%20in%20Rotated%20Sorted%20Array%20II.md)
  - 在153的基础上，中间数大于右边的数时，把mid+1赋给left下标
  - 如果中间数等于右边的数，right下标-1

## Double Pointer

> 对于vector，能用iterator就不用int型指针，会进一步加速算法耗时

### Questions

#### Head-Head

- [021. 合并两个链表](https://github.com/Noba1anc3/Leetcode/blob/master/021%20Merge%20Two%20Sorted%20Lists.md)
  - 双指针向前推进
  - 双指针迭代结束后，如果`l1`为空，合并链表迭代器的后继指向`l2`，否则指向`l1`
- [**349. 求两个数组的交集**](https://github.com/Noba1anc3/Leetcode/blob/master/349%20Intersection%20of%20Two%20Arrays.md)
  - 利用**set自带的排序机制**，将两个列表的元素insert到两个set当中
  - 生成两个迭代器分别在两个set中前进
    - 如果迭代器1的值小于迭代器2的值，迭代器1++
    - 反之，迭代器2++
    - 如果二者相等，将值加入交集中，两迭代器均++
- [350. 求两个数组的交数组](https://github.com/Noba1anc3/Leetcode/blob/master/350%20Intersection%20of%20Two%20Arrays%20II.md)
- [392. 判断A串是否为B串的子序列](https://github.com/Noba1anc3/Leetcode/blob/master/392%20Is%20Subsequence.md)
  - 如两指针值匹配，后移第一个指针
  - 无论是否匹配上，后移第二个指针

#### Head-Tail

- [125. 判断是否是回文字符串](https://github.com/Noba1anc3/Leetcode/blob/master/125%20Valid%20Palindrome.md)
  - 两指针分别位于字符串的首尾  `while(i < j)`
  - 如果`i`和`j`不是字母数字，且不会发生错位，则向中间靠拢
  - 如果双指针对应的小写字母一样，`i++`，`j--`，否则返回错误
  - 跳出while循环，返回true
- [**215. 第K大的数**](https://github.com/Noba1anc3/Leetcode/blob/master/215%20Kth%20Largest%20Element%20in%20an%20Array.md)
  - 内层`partition`可用首尾双指针

#### Fast-Low

- [**148. 排序链表**](https://github.com/Noba1anc3/Leetcode/blob/master/148%20Sort%20List.md)
  - 快慢双指针确定链表中点

#### Cur-Pre

- [**215. 第K大的数**](https://github.com/Noba1anc3/Leetcode/blob/master/215%20Kth%20Largest%20Element%20in%20an%20Array.md)
  - 内层`partition`可用前后双指针

- [剑指Offer-24. 反转链表](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-24%20Reverse%20LinkedList.md)
  - 前后双指针

#### Diffuse

- [005. 最长回文子串](https://github.com/Noba1anc3/Leetcode/blob/master/005%20Longest%20Palindromic%20Substring.md)
  - 从字符串每个位置开始，进行奇数长度和偶数长度回文串的双指针中心扩散查找

## Sort

[912. 排序数组](https://github.com/Noba1anc3/Leetcode/blob/master/912%20Sort%20an%20Array.md)

![](http://r.photo.store.qq.com/psc?/fef49446-40e0-48c4-adcc-654c5015022c/TmEUgtj9EK6.7V8ajmQrEAc33.VcF06TJP6HI3KS6o*QBdymPx7ngVeDVYG0T3pssqi.1hGwseWXm11JhZdmvYmGUpfJ1ka2iCJrU2hiooU!/r)

### Quick Sort

#### Algorithm

- [**QuickSort**](https://github.com/Noba1anc3/Leetcode/blob/master/QuickSort.md)

#### Questions

- [**075. 将012的乱序数组恢复顺序**](https://github.com/Noba1anc3/Leetcode/blob/master/075%20Sort%20Colors.md)
  - 初始化零指针为-1，二指针为数组长度
  - 初始化遍历指针为0，只要其小于二指针
    - 如果下标元素为0，交换下标元素与前移零指针元素，遍历指针前移
    - 如果下标元素为1，遍历指针前移
    - 如果下标元素为2，交换该元素与后移二指针元素

- [**215. 第K大的数**](https://github.com/Noba1anc3/Leetcode/blob/master/215%20Kth%20Largest%20Element%20in%20an%20Array.md)
  - 外层
    - 迭代法
      - 更快速（只能用在单侧排序，无法用于完整的快排算法）
      - 根据每次pivot，k和数组大小之间的关系来修改left或right指针
    - 递归法
      - 更好写
      - **只对一侧进行递归快排**
      - 根据每次pivot，k和快排右端点之间的关系来修改快排的左右端点及K的大小
  - 内层`partition`
    - 前后双指针
    - 首尾双指针
    - **随机快排**：`int pivotIndex = rand() % (right - left + 1) + left;`
- [**剑指Offer-40. 最小的K个数**](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-40%20Minimum%20K.md)

### Heap Sort

#### Algorithm

- [**Heap Sort**](https://github.com/Noba1anc3/Leetcode/blob/master/HeapSort.md)
  - Based on List

#### Questions

- [**253. 需要几间会议室**](https://github.com/Noba1anc3/Leetcode/blob/master/253%20Meeting%20Rooms%20II.md)
  - 按开始时间排序
  - 遍历会议，按结束时间进入最小堆
    - 开始时间晚于堆顶结束时间则弹出堆顶
    - 无论是否弹出，将当前会议加入堆中
  - 返回堆的大小

- [**剑指Offer-40. 最小的k个数**](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-40%20Minimum%20K.md)
  - 用最大堆存储前K个数
  - 后面的数只要比堆顶小，就弹出堆顶，将当前数插入堆中

### Merge Sort

#### Algorithm

- [**Merge Sort**](https://github.com/Noba1anc3/Leetcode/blob/master/MergeSort.md)

#### Questions

- [**148. 排序链表**](https://github.com/Noba1anc3/Leetcode/blob/master/148%20Sort%20List.md)

### Topological Sort

#### Questions

- [207. 给定课程学习先决条件，判断能否可行](https://github.com/Noba1anc3/Leetcode/blob/master/207%20Course%20Schedule.md)
  - 拓扑排序序列长度是否等于结点个数 或
  - DFS判断是否有环
- [210. 给定课程学习先决条件，返回拓扑序列](https://github.com/Noba1anc3/Leetcode/blob/master/210%20Course%20Schedule%20II.md)
  - 拓扑排序 或
  - DFS

### Count Sort

#### Algorithm

- [计数排序](https://github.com/Noba1anc3/Leetcode/blob/master/Sort%20-%20Count%20Sort.md)

### Others

#### Questions

- [对字典按值排序](https://github.com/Noba1anc3/Leetcode/blob/master/Sort%20dict%20by%20value.md)
  - 将dict转成vector
  - 实现cmp函数，对vector排序
- [056. 合并区间](https://github.com/Noba1anc3/Leetcode/blob/master/056%20Merge%20Intervals.md)
  - 对区间列表按起点进行排序
  - 遍历每个区间
    - 如果合并列表为空或合并列表最后一个区间的终点小于当前区间的起点，将该区间加入合并列表
    - 否则，更新合并列表最后一个区间的终点为其与当前区间终点的最大值
- [057. 插入区间](https://github.com/Noba1anc3/Leetcode/blob/master/057%20Insert%20Interval.md)
  - 遍历每个区间
    - 如果区间与待插入的区间没有交集，直接加入到结果当中
    - 如果有交集，更新待插入区间的左右端点
  - 将待插入区间加入结果当中，对结果进行排序
- [242. 判断两个字符串是否为字母异位词](https://github.com/Noba1anc3/Leetcode/blob/master/242%20Valid%20Anagram.md)
  - 排序后判断是否相同
- [252. 判断一个人能否参加全部会议](https://github.com/Noba1anc3/Leetcode/blob/master/252%20Meeting%20Rooms.md)
- [349. 求两个集合的交集](https://github.com/Noba1anc3/Leetcode/blob/master/349%20Intersection%20of%20Two%20Arrays.md)
- [350. 求两个数组的交数组](https://github.com/Noba1anc3/Leetcode/blob/master/350%20Intersection%20of%20Two%20Arrays%20II.md)
- [976. 三角形的最大周长](https://github.com/Noba1anc3/Leetcode/blob/master/976%20Largest%20Perimeter%20Triangle.md)
- [1403. 元素和超过数组一半的最短最大子序列](https://github.com/Noba1anc3/Leetcode/blob/master/1403%20Minimum%20Subsequence%20in%20Non-Increasing%20Order.md)
- [**剑指Offer-45. 数组元素能组成的最小数字串**](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-45%20MinNumofArrayStr.md)
  -  重构比较函数，组合` s1` 和 `s2` ，如果 `s1 + s2 < s2 + s1`，那么 `s1` > `s2` 
- [剑指Offer-51. 数组的逆序对数](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-51%20Reverse%20Num.md)

## Backtrack

### Algorithm

> 做题的时候，建议先画**树形图** ，画图能帮助我们想清楚递归结构，想清楚如何剪枝。
>
> 在画图的过程中思考清楚：
>
> - 分支如何产生；
>
> - 题目需要的解在哪里？是在叶子结点、还是在非叶子结点、还是在从跟结点到叶子结点的路径？
>
> - 哪些搜索会产生不需要的解？
>
> 如果在浅层就知道这个分支不能产生需要的结果，应该提前剪枝，剪枝的条件是什么，代码怎么写？

控制回溯进程

- 如果每次前进的内容不同（输入法）
  - 指针移动
  - 每次回溯，指针+1
- 如果每次前进的内容相同（生成括号）
  - 判断总长度是否满足要求即可
  - 每次回溯，判断长度是否达到要求
- 每次进入回溯函数，先判断指针是否到头或解满足要求，如到头或满足要求，则把解存到解集，并返回
- push进当前解 -> 回溯 -> pop当前解

### Questions

- [017. 九字输入法可能的字母组合](https://github.com/Noba1anc3/Leetcode/blob/master/017%20Letter%20Combination%20of%20a%20Phone%20Number.md)

  - ```c++
    void backtrack(std::string& digits, int start)
    ```

  - 如果当前下标达到`digits` 长度，将解加入解集

  - 遍历当前下标对应数字对应的字母，进行回溯

- [022. 生成括号](https://github.com/Noba1anc3/Leetcode/blob/master/022%20Generate%20Parenthesis.md)

  - 除回溯函数，要写check函数，判断字符串是否符合要求

  - **回溯剪枝**

    - ```c++
      void backtrack(int length, int left, int right)
      ```

    - 如果左括号个数少于length，生成左括号

    - 如果右括号个数少于左括号，生成右括号

- [037. 填数独](https://github.com/Noba1anc3/Leetcode/blob/master/037%20Sudoku%20Solver.md)

  - 找到当前数独中缺少的数字个数

  - 根据缺少个数进行回溯

  - ```c++
    void backtrack(std::vector<std::vector<char>>& board)
    ```

    1. 对行列进行遍历，找到每一个缺数的位置
    2. 遍历1-9，如果该位置可以被填入数字，填入数字，缺少个数-1
    3. 递归回溯，寻找下一个空位置
    4. 从回溯函数返回时，如果缺少个数为0，返回
    5. 否则，将该位置的数字去掉，缺少个数复原+1

- [039. 数组内元素可无限使用，组成一个和](https://github.com/Noba1anc3/Leetcode/blob/master/039%20Combination%20Sum.md)

  - ```c++
    void backtrack(std::vector<int>& candidates, int target, int index)
    ```

  - 如果`target`小于0，返回

  - 如果`target`等于0，将解加入解集，返回

  - 回溯时从index开始遍历，递归调用回溯函数时，下标保持不变

- [040. 数组内元素只能用一次，组成一个和](https://github.com/Noba1anc3/Leetcode/blob/master/040%20Combination%20Sum%20II.md)

  - 在039的基础上
    - 在回溯之前，**对数组进行排序**
    - 回溯过程中加入**剪枝**，如果当前下标超过index且当前元素与上一元素相等，则continue
    - 递归调用回溯函数时，**下标不再保持不变，而是加1**

- [046. 集合内元素的全排列](https://github.com/Noba1anc3/Leetcode/blob/master/046%20Permutations.md)

  - ```c++
    void backtrack(vector<int>& nums)
    ```

  - ```c++
    vector<int> newlist = nums;
    newlist.erase(newlist.begin() + i);
    ```

  - 如果列表为空，将解加入解集

- [047. 集合内元素的unique全排列](https://github.com/Noba1anc3/Leetcode/blob/master/047%20Permutation%20II.md)

  - 在046的基础上进行**剪枝**：如果当前元素和上一个元素一样，就continue

- [051. N皇后](https://github.com/Noba1anc3/Leetcode/blob/master/051%20N-Queens.md)

  - ```c++
    void backtrack(int row)
    ```

  - 如果row与总行数相同，将解加入解集

  - 遍历当前行下的每一列，进行检查（当前位置为0且不会被攻击）

  - 递归回溯前，将解加入解集，并进行落子（其所在行列和两条斜线改为-1，该位置设为1）

  - 递归回溯前，将解退出解集，并进行退子（其所在行列和两条斜线改为0，该位置设为0）

- [060. 求1到N元素的第K个全排列](https://github.com/Noba1anc3/Leetcode/blob/master/060%20Permutation%20Sequence.md)

  - 计算N-1的阶乘，得到第K个全排列的首数字和其是剩余数字的第几个全排列

    - ```c++
      int x = factorial(n-1);
      int a = (k - 1) / x + 1;
      int b = k % x == 0 ? x : k % x;
      ```

  - 对剩余数字回溯计算全排列

  - 直接找到指定枝叶

- [077. 1到N当中的数字 由k个元素构成的所有组合](https://github.com/Noba1anc3/Leetcode/blob/master/077%20Combinations.md)

  - ```c++
    void backtrack(int max, int cur, int width)
    ```

  - N为max，K为width，cur初始为1

  - 如果`width`为0，将解加入解集

  - 每次递归回溯时，max不变，初始数字cur+1，width-1

- [078. 求不含重复元素集合的所有子集](https://github.com/Noba1anc3/Leetcode/blob/master/078%20Subsets.md)

  - ```c++
    void backtrack(vector<int>& nums, int index)
    ```

  - 将回溯过程中所有尚未完成的解都加到解集中

  - 最后加空集合{}

- [090. 求无重复元素的集合的所有子集](https://github.com/Noba1anc3/Leetcode/blob/master/078%20Subsets.md)

  - 在078的基础之上
    - 在回溯之前，**对数组进行排序**
    - 回溯过程中加入**剪枝**，如果当前下标超过index且当前元素与上一元素相等，则continue

- [093. 还原IP地址](https://github.com/Noba1anc3/Leetcode/blob/master/093%20Restore%20IP%20Addresses.md)

  - 对切成的段数和当前下标进行回溯

  - ```c++
    void backtrack(std::string& s, int piece, int index)
    ```

    - 如果段数为4或下标到字符串长度则返回
      - 如果两个条件都满足，将当前解添加到解集当中
    - 遍历时从1位到3位
      - 如果index+位数超过字符串长度，返回
      - 如果index位字母为0且长度不为1，返回
      - 如果长度为3且index到i的字串超过255，返回
      - 在当前解中加该子串，并添加一个`'.'`
      - 递归回溯，段数+1，index+位数

- [494. 用正负数组内的数字组成目标和](https://github.com/Noba1anc3/Leetcode/blob/master/494%20Target%20Sum.md)

  - ```c++
    void backtrack(vector<int>& nums, int target, int index, int sum)
    ```

  - 如果`index`遍历到`nums`长度则返回

    - 如果`sum`与`target`相等，答案数+1

  - 对下一位置的 `sum + nums[index]` 及 `sum + nums[index]` 进行回溯

- [784. 字母与数字的大小写全排列](https://github.com/Noba1anc3/Leetcode/blob/master/784%20Letter%20Case%20Permutation.md)

  - ```c++
    void backtrack(string& S, int i)
    ```

  - 如果i遍历到了字符串长度则返回

  - 如果是小写字母，将其大写字母拼到解当中，进行递归回溯

  - 如果是大写字母，将其小写字母拼到解当中，进行递归回溯

  - 大小写字母回溯结束后，再进行一次递归回溯，针对原大小写

- [剑指Offer-17. 输出1到最大的N位数](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-17%20Print%20N-bit%20Numbers.md)

  - ```c++
    void backtrack(int width)
    ```

  - 如果width减到0，则返回

  - 从0到9进行递归回溯

## Divide & Conquer

> 与回溯法类似，需要设计递归函数。递归的返回条件十分重要，否则将陷入无穷的递归当中。
>
> 常规方法：设计一递归方法，首先指明返回条件，而后将数组分为左右两个子数组，递归处理两部分子数组，单独处理整个数组，得到三部分结果后，基于一定规则将三部分合并。
>
> - Divide
>   - Dividing a given problem into two or more subproblems
> - Conquer
>   - Solving each subproblem (directly if small enough or recursively)
> - Combine
>   - Combining the solutions of the subproblems into a global solution

### Questions

- [**053. 求数组的最大和连续子数组**](https://github.com/Noba1anc3/Leetcode/blob/master/053%20Maximum%20Contiguous%20Subarray.md)
  - Conquer
    - 递归函数的返回条件为数组的长度为1
    - 问题的解是左半数组，右半数组和整个数组三部分解的最大值
  - Combine
    - 从中间向两边进行扩散，不断更新左右两侧的最大和，返回两侧最大和的和
  - `O(n) = 2 * O(n/2) + O(n) = O(n log n)`
- 快速排序
  - [215. 第K大的数](https://github.com/Noba1anc3/Leetcode/blob/master/215%20Kth%20Largest%20Element%20in%20an%20Array.md)
  - [912. 排序数组](https://github.com/Noba1anc3/Leetcode/blob/master/912%20Sort%20an%20Array.md)
  - [剑指Offer-40. 最小的K个数](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-40%20Minimum%20K.md)
- [**剑指Offer-51. 数组的逆序对数**](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-51%20Reverse%20Num.md)
  - Conquer
    - 递归函数的返回条件为数组的长度小于等于1
    - 问题的解是左半数组，右半数组和对这两个子数组计算逆序对数三部分解的和
  - Combine
    - 排序两数组
    - 使用首-首双指针向前推进
      - 如果A数组元素小于B数组元素，A指针前移
      - 反之，逆序对数增加A数组长度与A指针的差，B指针前移

## Greedy

![](http://m.qpic.cn/psc?/V50VqFfH2A6OlZ2gWBDL0uxzNK4WmFgm/TmEUgtj9EK6.7V8ajmQrEJAetLW06Ii0SiR01HiR9VQaxNgpyr9qwL68YV902EjL5dXWIz3UUs7zUaI098EAzb3aqGSVlnK42OY6a7mGUoc!/b&bo=4wV*AuMFfwIDKQw!&rf=viewer_4)

### Questions

- [最小生成树 - Prim算法](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Prim.md)
  - 每次贪心选择与当前顶点集最近的结点，将两顶点之间的边加入最小生成树
- [单源最短路径 - Dijkstra算法](https://github.com/Noba1anc3/Leetcode/blob/master/Graph%20-%20Dijkstra.md)
  - 每次贪心选择距离源点最近的结点，松弛与其相邻结点的距离
- [部分背包](https://github.com/Noba1anc3/Leetcode/blob/master/Greedy%20-%20Fractional%20Knapsack.md)
  - 每次贪心选择单位重量价值最高的物品放入背包
- [**045. 最少几步能从起点跳到终点**](https://github.com/Noba1anc3/Leetcode/blob/master/045%20Jump%20Game%20II.md)
  - 初始化搜索区间的起点与终点，步数和最远距离为0（区间搜索）
  - 只要搜索区间终点尚未达到数组终点
    - 计算搜索区间内可以达到的最远距离
    - 将搜索区间起点改为上次搜索区间终点的下一位置，搜索区间终点改为上次搜索得到的最远可达距离，同时将搜索步数+1
- [**055. 能否从起点跳到终点**](https://github.com/Noba1anc3/Leetcode/blob/master/055%20Jump%20Game.md)
  - 遍历列表中的下标（点搜索）
    - 如果最远可达距离超过该下标，根据该下标及其数字更新最远可达距离
      - 如果新的最远可达距离超过终点，则可抵达终点
    - 否则，返回不可抵达
- [122. 多次买卖股票的最大利润](https://github.com/Noba1anc3/Leetcode/blob/master/122%20Best%20Time%20to%20Buy%20and%20Sell%20Stock%20II.md)
  - 遍历每一对前后股票的价格
  - 如果价格差为正，将其加入到最大利润中
- [253. 需要几间会议室](https://github.com/Noba1anc3/Leetcode/blob/master/253%20Meeting%20Rooms%20II.md)
  - 每次贪心进入结束时间小于本次会议开始时间的所有会议室中最早结束的一间
- [279. 求组成数字需要的最少完全平方数](https://github.com/Noba1anc3/Leetcode/blob/master/279%20Perfect%20Squares.md)
  - DFS Based

    - 对组成数字的完全平方数的个数由少到多进行贪心

  - BFS Based
    - 根据贪心算法的思想，先遍历完同层元素后再遍历下一层是更合理的做法
- [**435. 删除最少的区间使剩下区间无重叠**](https://github.com/Noba1anc3/Leetcode/blob/master/435%20Non-overlapping%20Intervals.md)
  - 活动选择问题：删除最少的区间相当于留下最多的区间
  - 对区间按结束时间进行排序
  - 遍历区间，如果当前区间开始时间晚于上个活动结束时间，更新活动结束时间
  - 否则，待删除区间数 +1
- [**452. 扎破气球所需要的最少数量的箭**](https://github.com/Noba1anc3/Leetcode/blob/master/452%20Minimum%20Number%20of%20Arrays%20to%20Burst%20Balloons.md)
  - 活动选择问题
  - 对区间按结束时间进行排序
  - 遍历区间，如果当前区间开始时间晚于上个活动结束时间，更新活动结束时间，箭数 +1
- [743. 全网收到信号需要的时间](https://github.com/Noba1anc3/Leetcode/blob/master/743%20Network%20Delay%20Time.md)
  - 基于Dijkstra算法
- [976. 三角形的最大周长](https://github.com/Noba1anc3/Leetcode/blob/master/976%20Largest%20Perimeter%20Triangle.md)
  - 每次贪心检查最长的三条边能否组成三角形
- [1167. 拼接火柴棍的最小代价](https://github.com/Noba1anc3/Leetcode/blob/master/1167%20Minimum%20Cost%20to%20Connect%20Sticks.md)
  - 每次贪心选择最短的火柴棍进行拼接
- [1403. 元素和超过数组一半的最短最大子序列](https://github.com/Noba1anc3/Leetcode/blob/master/1403%20Minimum%20Subsequence%20in%20Non-Increasing%20Order.md)
  - 每次贪心选择最大的数字，检查是否满足要求
- [剑指Offer-14-II. 在和固定时，将乘积最大化](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-14-II%20Rope%20Cutting%20II.md)
  - 每次贪心选择将和拆分为3的因子

## Dynamic Programming

### Questions

#### 0-1 Knapsack

**Original :** [0-1背包](https://github.com/Noba1anc3/Leetcode/blob/master/DP%20-%2001%20Knapsack.md)

- 状态
  - 放入几个物品
  - 背包容量大小
- 选择
  - 是否将当前物品装入背包

- 状态数组
  - `int V[n+1][w+1] = 0`
  - `V[i][w]` : 使用背包中前`i`个物品，在限制容量为`w`时，可以获得的最大价值
- 状态初始化
  - `V[0][w] = 0`
- 状态转移
  - `V[i][w] = max(V[i-1][w], vi + V[i-1][w - wi])`

**Varieties**

- [**加权活动选择**](https://github.com/Noba1anc3/Leetcode/blob/master/DP%20-%20Weighted%20Activity%20Selection.md)

  - 将活动按结束时间排序，构建结束时间早于当前活动开始时间的最晚活动列表`p`

  - 状态

    - 参加前几个活动

  - 选择

    - 是否参加当前活动

  - 状态数组

    - `int dp[n+1] = 0`  
    - `dp[i]` : 在前`i`个活动中进行参加可以获得的最大收益

  - 状态初始化

    - `dp[0] = 0`

  - 状态转移

    - ```c++
      dp[i] = max(dp[i-1], Activity_i.weight + dp[p[i]])
      ```

- [**416. 判断能否分割等和子集**](https://github.com/Noba1anc3/Leetcode/blob/master/416%20Partition%20Equal%20Subset%20Sum.md)

  - 如果数组和为奇数，最大值超过数组和的一半或数组长度为1，均返回false
  - Phase I
    - 状态
      - 选用前几个数字
      - 可以获得的数字和
    - 选择
      - 是否选用当前数字
    - 状态数组
      - `vector<vector<bool>> dp(n, vector<bool>(sum_of_array/2 + 1, false)`
      - `dp[i][j]` : 选用前`i`个数字，是否可以获得值为`j`的和
    - 状态初始化
      - `dp[0][nums[0]] = true; dp[i][0] = true `
    - 状态转移
      - `dp[i][j] = dp[i-1][j] || dp[i-1][j - num]`
    - 备注
      - 如果提前使`target`得到了满足，可直接返回true
  - Phase II
    - 状态
      - 可以获得的数字和
    - 选择
      - 是否选用当前数字
    - 状态数组
      - `vector<bool> dp(sum_of_array/2 + 1, false)`
      - `dp[j]` : 是否可以获得值为`j`的和
    - 状态初始化
      - `dp[0] = true `
    - 状态转移
      - `dp[j] |= dp[j - num]`
    - 备注
      - 计算顺序：与一阶段不同，此时的`j`从`target`逆序向当前`num`进行遍历
      - 如果提前使`target`得到了满足，可直接返回true

- [**474. 限制0和1个数时，可以装下的最多字符串数目**](https://github.com/Noba1anc3/Leetcode/blob/master/474%20Ones%20and%20Zeroes.md)

  - Phase I
    - 状态
      - 选用前几个字符串
      - 背包中装0的数量
      - 背包中装1的数量
    - 选择
      - 是否将当前字符串装入背包
    - 状态数组
      - `int dp[n+1][m+1][n+1] = 0`
      - `dp[i][j][k]` : 使用前`i`个字符串，背包容量为`j`个0和`k`个1时，可以容纳的最多字符串
    - 状态初始化
      - `dp[0][i][j] = 0; dp[i][0][0] = 0 `
    - 状态转移
      - `dp[i][j][k] = max(dp[i][j][k], 1 + dp[i-1][j - num_of_zero][k - num_of_one])`
  - Phase II
    - 状态
      - 背包中装0的数量
      - 背包中装1的数量
    - 选择
      - 是否将当前字符串装入背包
    - 状态数组
      - `int dp[m+1][n+1] = 0`
      - `dp[j][k]` : 背包容量为`j`个0和`k`个1时，可以容纳的最多字符串
    - 状态初始化
      - `dp[j][k] = 0 `
    - 状态转移
      - `dp[j][k] = max(dp[j][k], 1 + dp[j - num_of_zero][k - num_of_one])`

- [**494. 用正负数组内的数字组成目标和**](https://github.com/Noba1anc3/Leetcode/blob/master/494%20Target%20Sum.md)

  - 状态
    - 选用前几个数字
    - 目标和的大小
  - 选择
    - 对当前数字加或是减
  - 状态数组
    - `int dp[n][2*sum+1] = 0`
    - `dp[i][j]` : 用前`i`个数字组成`j`目标的方法数
  - 状态初始化
    - `dp[0][sum+nums[0]] += 1; dp[0][sum-nums[0]] += 1`
  - 状态转移
    - ` dp[i][j] += (dp[i-1][j-nums[i]] + dp[i+1][j+nums[i]])`
  - 备注
    - 返回值为`dp[n-1][sum+target]`  原因在于: `sum`的左半部为负，右半部为正，初始化`sum`而不是0也是同理

- [**518. 硬币可重复使用，求组成金额有多少种组合**](https://github.com/Noba1anc3/Leetcode/blob/master/518%20Coin%20Change%202.md)

  - 此题应类比下方斐波那契系列[070爬楼梯问题](https://github.com/Noba1anc3/Leetcode/blob/master/070%20Climbing%20Stairs.md)

  - Phase I

    - 状态
      - 选用前几个物品
      - 待凑的金额大小
    - 选择
      - 当前硬币是否用来凑金额
    - 状态数组
      - `int dp[n+1][amount+1] = 0`
      - `dp[i][j]` : 使用前`i`种硬币，待凑金额为`j`时的凑法个数
    - 状态初始化
      - `dp[0][i] = 0; dp[i][0] = 1 `
    - 状态转移
      - `dp[i][j] = dp[i-1][j] + dp[i][j - coin]`
    - 备注
      - 此题不同于其他0-1背包题目，在状态转移时不使用i-1而是i. 设想，如果要计算`dp[1][2]`, 按照此前题目的方法，`dp[1][2] = dp[0][2] + dp[0][2 - coin]`，显然并不合理。

  - Phase II

    - 状态

      - 待凑的金额大小

    - 选择

      - 当前硬币是否用来凑金额

    - 状态数组

      - `int dp[amount+1] = 0`
      - `dp[j]` : 待凑金额为`j`时的凑法个数

    - 状态初始化

      - `dp[i] = 0; dp[0] = 1 `

    - 状态转移

      - `dp[j] += dp[j - coin]`

    - 备注

      - 计算顺序：先coin再amount，计算组合数；先amount再coin计算的是排列数（爬楼梯问题） 

      - 二维dp的组合数问题和排列数问题都可以交换嵌套的循环，因为子问题不会变化；一维的dp组合数问题和排列数问题不可以交换嵌套的循环，因为会改变子问题； 一维的dp组合数问题，交换嵌套的循环，子问题会变成排列数问题，计算顺序会从先列后行变为先行后列；反之亦然。

      - | 1    | 1    | 1    | 1    | 1    | 1    |
        | ---- | ---- | ---- | ---- | ---- | ---- |
        | 1    | 1    | 2    | 2    | 3    | 3    |
        | 1    | 1    | 2    | 2    | 3    | 4    |

- [面试题 17.16 只能做间隔选择时，怎样最大化收益](https://github.com/Noba1anc3/Leetcode/blob/master/%E9%9D%A2%E8%AF%95%E9%A2%98%2017.16%20The%20Masseuse.md)

  - 状态
    - 选择前几个活动
  - 选择
    - 是否选择当下的活动
  - 状态数组
    - `int dp[n+1] = 0`
    - `dp[i]` : 从前`i`个活动中做间隔选择可以获得的最大收益
  - 状态初始化
    - `dp[0] = 0, dp[1] = nums[0] `
  - 状态转移
    - `dp[i] = max(dp[i-2] + nums[i-1], dp[i-1]`
  - 备注
    - 可以像爬楼梯一样用三个变量代替dp数组，降低空间复杂度

#### Rod Cutting

**Original :** [切割钢条](https://github.com/Noba1anc3/Leetcode/blob/master/DP%20-%20Rod%20Cutting.md)

- 状态
  - 钢条被切割的长度
- 选择
  - 在哪个点进行钢条切割

- 状态数组
  - `int r[n+1] = 0`
  - `r[i]` : 长度为i的钢条可以卖出的最大价格
- 状态初始化
  - `r[0] = 0`
- 状态转移
  - `r[j] = for i from 1 to j : max(p[i] + r[j - i])`

**Varieties**

- [343. 在和固定时，将乘积最大化](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-14-I%20Rope%20Cutting%20I.md)

  - 状态

    - 数字和的大小

  - 选择

    - 当和固定时，在哪个点将和拆分成两个数字

  - 状态数组

    - `int dp[n+1] = 0`
    - `dp[i]` : 和为`i`时，可以获得的最大乘积 

  - 状态初始化

    - `dp[i] = i`

  - 状态转移

    - ```c++
      for (int j = 2; j <= (int)i/2; j++)
          dp[i] = max(dp[i], dp[j] * dp[i-j]);
      ```

#### Matrix Multiplication

**Original :** [矩阵乘积](https://github.com/Noba1anc3/Leetcode/blob/master/DP%20-%20Matrix%20Multiplication.md)

- 状态
  - 连乘矩阵串的首尾矩阵下标
- 选择
  - 在哪个矩阵处将矩阵乘积串一分为二

- 状态数组
  - `int m[n+1][n+1] = 0`
  - `m[i][j]` : 从`i`开始到`j`为止的连乘矩阵串所需要的最少乘积次数
- 状态初始化
  - `m[i][i] = 0`
- 状态转移
  - `m[i][j] = for k from i to j-1 : min(m[i][k] + m[k+1][j] + pi-1*pk*pj)`
- 备注
  - 计算顺序：对`j-i`从`1`到`n-1`进行计算

**Varieties**

- [最优平衡二叉树](https://github.com/Noba1anc3/Leetcode/blob/master/DP%20-%20Optimal%20BST.md)
  - 状态
    - zz
  - 选择
    - zz
  - 状态数组
    - `int e[n+1][n+1] = INT_MAX`
  - 状态初始化
    - `e[i][i-1] = qi-1`
  - 状态转移
    - `e[i][j] = for r from i to j : min(e[i][r-1] + e[r+1][j] + w[i][j])`
  - 备注
    - 计算顺序：对`j-i`从0到n-1，i从1到n进行计算
- [**005. 最长回文子串**](https://github.com/Noba1anc3/Leetcode/blob/master/005%20Longest%20Palindromic%20Substring.md)
  - 状态
    - 子串的开始与结束点下标
  - 选择
    - 无
  - 状态数组
    - `bool dp[n][n] = false `
    - `dp[i][j]` : 从`i`开始到`j`为止的子串是否是回文字符串
  - 状态初始化
    - `dp[i][i] = true; dp[i][i+1] = (s[i] == s[i+1])`
  - 状态转移
    - `dp[i][j] = dp[i+1][j-1] && s[i] == s[j]`
  - 备注
    - 计算顺序：对`j-i`从0到n-1，i从0进行计算

#### Longest Common Subsequence

**Original :** [**最长公共子序列**](https://github.com/Noba1anc3/Leetcode/blob/master/1143%20Longest%20Common%20Subsequence.md)

- 状态
  - 两个子串的结束位置
- 选择
  - 如果两字母相同，长度加一；如果两字母不同，长度取两缺失尾字母子序列长度的最大值

- 状态数组
  - `int dp[m+1][n+1] = 0`
  - `dp[i][j]` : 在`i`位置结束的A子串与在`j`位置结束的B子串的最长公共子序列长度 
- 状态初始化
  - `dp[i][0] = 0; dp[0][j] = 0`
- 状态转移
  - `dp[i][j] = dp[i-1][j-1] + 1; dp[i][j] = max(dp[i-1][j], dp[i][j-1])`
- 备注
  - 计算顺序：先行后列

**Varieties**

- [最长公共子串](https://github.com/Noba1anc3/Leetcode/blob/master/DP%20-%20Longest%20Common%20Substring.md)
  - 状态
    - 两个子串的结束位置
  - 选择
    - 如果两字母相同，长度加一；如果两字母不同，长度归零
  - 状态数组
    - `int dp[m+1][n+1] = 0`
    - `dp[i][j]` : 在`i`位置结束的A子串与在`j`位置结束的B子串的最长公共子串长度 
  - 状态初始化
    - `dp[i][0] = 0; dp[0][j] = 0`
  - 状态转移
    - `dp[i][j] = dp[i-1][j-1] + 1; dp[i][j] = 0`
  - 备注
    - 计算顺序：先行后列
- [**最小编辑距离**](https://github.com/Noba1anc3/Leetcode/blob/master/072%20Edit%20Distance.md)
  - 状态
    - 两个子串的结束位置
  - 选择
    - 当前状态的编辑距离为三种子状态编辑距离的最小值
  - 状态数组
    - `int dp[m+1][n+1] = 0`
    - `dp[i][j]` : 在`i`位置结束的A子串与在`j`位置结束的B子串之间的编辑距离
  - 状态初始化
    - `dp[i][0] = i; dp[0][j] = j`
  - 状态转移
    - `dp[i][j] = min(dp[i][j-1] + 1, dp[i-1][j] + 1, dp[i-1][j-1] + c)`
  - 备注
    - 计算顺序：先行后列
- [在矩阵上向右或向下走可以获得的最大价值](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-47%20Maximum%20Value%20of%20Presents.md)
  - 状态
    - 当前坐标
  - 选择
    - 上方和左方的最大值
  - 状态数组
    - `int dp[m+1][n+1] = 0`
    - `dp[i][j]` : 在`[i,j]`位置可以获得的最大价值
  - 状态初始化
    - `dp[i][j] = 0`
  - 状态转移
    - `dp[i][j] = max(dp[i][j-1], dp[i-1][j]) + grid[i-1][j-1]`
  - 备注
    - 利用原始数组降低空间复杂度

#### Fibonacci

**Original :** [剑指Offer-10-I. 求斐波那契数列的第n项](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-10-I%20Fibonacci.md)

- 状态
  - 斐波那契项数
- 选择
  - 当前斐波那契项为前两位斐波那契项的和
- 状态数组
  - `int a, b, c`
  - `c` : 当前的斐波那契项
- 状态初始化
  - `a = 0; b = 1`
- 状态转移
  - `c = a + b; a = b; b = c `
- 备注
  - 如果n小于2，直接返回n

**Varieties**

- [070. 求有几种爬楼梯的方法](https://github.com/Noba1anc3/Leetcode/blob/master/070%20Climbing%20Stairs.md)

  - 与[面试题 08.01 爬楼梯问题](https://github.com/Noba1anc3/Leetcode/blob/master/%E9%9D%A2%E8%AF%95%E9%A2%98%2008.01%20Three%20Steps%20Problem.md)是同样的问题

  - 状态

    - 楼梯层数

  - 选择

    - 爬当前层数的方法数为爬上一层与上两层的方法数之和

  - 状态数组

    - `int a, b, c`
    - `c` : 爬当前层可以有的方法数

  - 状态初始化

    - `a = 1; b = 1; c = 1`

  - 状态转移

    - `c = a + b; a = b; b = c `

  - 备注

    - 假设`a`为第0层，天然有一种方法

    - | 1    | 1    | 1    | 2    | 3    | 5    |
      | ---- | ---- | ---- | ---- | ---- | ---- |
      | 1    | 1    | 2    | 3    | 5    | 8    |
      | 1    | 1    | 2    | 3    | 5    | 9    |

- [279. 求组成数字需要的最少完全平方数](https://github.com/Noba1anc3/Leetcode/blob/master/279%20Perfect%20Squares.md)

  - 状态
    - 待组成的数字
  - 选择
    - 选择为了组成当前数字减去比当前数字小的完全平方数得到的数字所需要的完全平方数的最小值
  - 状态数组
    - `int dp[n+1] = INT_MAX` 
    - `dp[i]` : 组成`i`这一数字需要的最少完全平方数 
  - 状态初始化
    - `dp[0] = 0`
  - 状态转移
    - `dp[i] = for square in squares : min(dp[i], dp[i - square] + 1) `
  - 备注
    - 外层遍历数字，内层遍历平方数列表

- [322. 硬币可重复使用，求组成金额需要的最少数量](https://github.com/Noba1anc3/Leetcode/blob/master/322%20Coin%20Change.md)

  - 状态
    - 待凑成的金额
  - 选择
    - 选择为了凑成当前金额减去比当前金额小的硬币得到的金额所需要的硬币数的最小值
  - 状态数组
    - `int dp[amount+1] = 0`
    - `dp[i]` : 组成`i`金额需要的最少硬币数
  - 状态初始化
    - `dp[i] = amount+1`
    - 用INT_MAX则需要改为float数组
  - 状态转移
    - ` dp[i] = for coin in coins : min(dp[i], dp[i - coin] + 1)`

#### Numbers

- [**053. 求数组的最大和连续子数组**](https://github.com/Noba1anc3/Leetcode/blob/master/053%20Maximum%20Contiguous%20Subarray.md)
  - 状态
    - 数组的某个位置
  - 选择
    - 当前位置数字与当前最大和加当前位置数字的最大值
    - 当前最大和与全局最大和的最大值
  - 状态数组
    - `int curAns, maxAns`
    - `curAns` : 当前位置为止的连续子数组和
    - `maxAns` : 当前位置为止的连续子数组最大和
  - 状态初始化
    - `curAns = 0; maxAns = INT_MIN `
  - 状态转移
    - `curAns = max(curAns + x, x); maxAns = max(curAns, maxAns)`
  - 备注
    - 人生亦是如此，随时都可以选择重新开始hh
- [121. 一次买卖股票可以得到的最大收益](https://github.com/Noba1anc3/Leetcode/blob/master/121%20Best%20Time%20to%20Buy%20and%20Sell%20Stock.md)
  - 状态
    - 数组的某个位置
  - 选择
    - 当前位置价格减全局最低价与最大收益的最大值
    - 当前位置价格与全局最低价的最小值
  - 状态数组
    - `int minPrice, maxProfit `
    - `minPrice` : 当前位置为止的最低价格
    - `maxProfit` : 当前位置为止的最大收益
  - 状态初始化
    - `minPrice = prices[0]; maxProfit = INT_MIN `
  - 状态转移
    - `maxProfit = max(maxProfit, price - minPrice); minPrice = min(minPrice, price)`
- [122. 多次买卖股票可以得到的最大收益](https://github.com/Noba1anc3/Leetcode/blob/master/122%20Best%20Time%20to%20Buy%20and%20Sell%20Stock%20II.md)
  - 状态
    - 数组的某个位置
  - 选择
    - 每一个为正的gap
  - 状态数组
    - `int profit`
    - `profit` : 当前位置为止的最大收益
  - 状态初始化
    - `profit = 0 `
  - 状态转移
    - `profit = max(profit, profit + prices[i] - prices[i-1])`

#### Others

- [**032. 最长有效括号串长度**](https://github.com/Noba1anc3/Leetcode/blob/master/032%20Longest%20Valid%20Parentheses.md)

  - 状态

    - 括号串的位置

  - 选择

    - 如果是左右括号
      - 则当前位置的最长有效长度更新为左括号前一个位置的最长有效长度+2
    - 如果是右右括号且第一个右括号向前回退其有效括号长度的位置处为左括号
      - 则当前位置的最长有效长度更新为第一个右括号的最长有效长度 + 回退到的左括号的上一个括号处的最长有效长度 + 2

  - 状态数组

    - `int dp[n] = 0`
    - `dp[i]` : 到`i`位置为止子串的最长有效括号串长度

  - 状态初始化

    - `dp[i] = 0`

  - 状态转移

    - ```c++
      if s[i] = ')' && s[i-1] = '(':
      	dp[i] = (i >= 2) ? dp[i-2] + 2 : 2;
      if s[i] = ')' && s[i-1] = ')' && i - 1 - dp[i-1] >= 0 && s[i-1-dp[i-1]] = '('
      	dp[i] = (i - dp[i-1] >= 2) ? dp[i-1] + 2 + dp[i-2-dp[i-1]] : dp[i-1] + 2
      ```

## Math

### Questions

- [136. 数组中只出现了一次的数字](https://github.com/Noba1anc3/Leetcode/blob/master/136%20Single%20Number.md)
  - 用0与数组所有元素逐一异或 ^
  - 返回异或的结果

- [169. 计算数组中出现次数超过一半的数字](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-39%20Majority%20Element.md)
  - 首先设定主导数字和票数均为0
  - 遍历所有数字
    - 如果当前票数为0，修改主导数字为当前被遍历到的数字
    - 无论票数是否为0，如果主导数字与当前被遍历的数字相同则票数+1，否则-1
- [292. Nim游戏](https://github.com/Noba1anc3/Leetcode/blob/master/292%20Nim%20Game.md)
  - 只要石头数是4的整数倍，就是对方赢；否则，自己赢。
- [剑指Offer-14-I. 在和固定时，将乘积最大化](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-14-I%20Rope%20Cutting%20I.md)
  - 根据算术几何均值不等式，各个因子相同时，乘积最大
  - 根据对函数求导结果，值为3的因子越多时，乘积越大
  - 当 n ≤ 3 时，按规则应不拆分，由于题目要求必须拆分，必须拆出一个因子 1 ，即返回 n - 1
  - 当 n > 3 时，求 n 除以 3 的 整数部分 a 和 余数部分 b （即 n = 3a + b ），并分为以下三种情况：
    - 当 b = 0 时，直接返回 3 ^ a
    - 当 b = 1 时，要将一个 1 + 3 转换为 2 + 2，因此返回 3 ^ (a - 1) x 4
    - 当 b = 2 时，返回 3 ^ a x 2
- [剑指Offer-14-II. 在和固定时，将乘积最大化](https://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-14-II%20Rope%20Cutting%20II.md)
  - 只要n大于4，每次减3，每次将结果 × 3 并对1e9+7取余
  - 将结果 × n，对1e9+7取余
- [剑指Offer-56-I. 找出数组中唯二只出现了一次的数字，其他数字均出现两次](http://github.com/Noba1anc3/Leetcode/blob/master/%E5%89%91%E6%8C%87offer-56-I%20The%20number%20of%20occurrences%20of%20a%20number%20in%20an%20array.md)
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
    
// 求绝对值
abs(integer)
    
// 1e9+7这个数满足[0, 1e9+7)之内的数字相加不超int，相乘不超long long，是个质数
```

### struct

```c++
struct Edge{
    int vertex1;
    int vertex2;
    int weight;
};

// 创建结构体数组
struct Item items[n];

// 对结构体数组进行排序
sort(items, items + n)
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

### list

```c++
memset(dp, 0, sizeof(dp));

// 求列表长度
sizeof(steps) / sizeof(int);
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

// 将B vector插入到A vector的后面
std::vector.insert(A.end(), B.begin(), B.end())

// vector大小
std::vector.size()

// vector调整大小
std::vector.resize(integer)
    
// vector初始化
std::vector<type> vector2(vector1.begin(), vector1.end())
std::vector<std::vector<type>> vector(n, std::vector<type>())
std::vector<std::vector<type>> vector(n, std::vector<type>(n, INT_MAX))
    
// 交换vector元素位置
swap(std::vector[i], std::vector[j])

// vector反转
reverse(std::vector.begin(), std::vector.end())
    
// 求vector元素和
accumulate(nums.begin(), nums.end(), 0)
    
// 求vector最大元素
// max_element本身返回的是迭代器
*max_element(nums.begin(), nums.end())
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
    
// 遍历map的所有key
unordered_map<int, int>::iterator it1 = M.begin();
while (it1 != M.end()){ int key = it1->first; }
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
 
// 倒序遍历数组时
while (it != nums.begin() - 1)
    
// 可以在取值的同时调整指针
*it++
*it--
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
    
// 引用传递不需要调用构造函数去构造函数的局部变量
&s
```

## python

### list

```python
# 按照元素的首元素进行排序
list.sort(key = lambda x : x[0])

# 倒序排序
list.sort(reverse = True)

# 寻找list中有几个count元素
list.count(element)

# 寻找list中，从index下标开始，value所在的下标
list.index(value, index)

# 求数组内元素和
sum(list)
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

### set

```python
lookup = set()
set.remove(element)
set.add(element)
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
