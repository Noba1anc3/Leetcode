Given a positive integer ```n```, find the least number of perfect square numbers (for example, ```1, 4, 9, 16, ...```) which sum to ```n```.

**Example:**
```
Input: n = 12
Output: 3 
Explanation: 12 = 4 + 4 + 4.

Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
```

## Solution
### 方法一：暴力枚举法

这个问题要求我们找出由完全平方数组合成给定数字的最小个数。我们将问题重新表述成：

给定一个完全平方数列表和正整数 ```n```，求出完全平方数组合成 ```n``` 的组合，要求组合中的解拥有完全平方数的最小个数。

注：可以重复使用列表中的完全平方数。

从上面对这个问题的叙述来看，它似乎是一个组合问题，对于这个问题，一个直观的解决方案是使用暴力枚举法，我们枚举所有可能的组合，并找到完全平方数的个数最小的一个。

我们可以用下面的公式来表述这个问题：

numSquares(n) = min(numSquares(n-k) + 1) ∀k ∈ square numbers

从上面的公式中，我们可以将其转换为递归解决方案。

```python
class Solution(object):
    def numSquares(self, n):
        square_nums = [i**2 for i in range(1, int(math.sqrt(n))+1)]

        def minNumSquares(k):
            """ recursive solution """
            # bottom cases: find a square number
            if k in square_nums:
                return 1
            min_num = float('inf')

            # Find the minimal value among all possible solutions
            for square in square_nums:
                if k < square:
                    break
                new_num = minNumSquares(k-square) + 1
                min_num = min(min_num, new_num)
            return min_num

        return minNumSquares(n)
```
### 方法二：动态规划

使用暴力枚举法会超出时间限制的原因很简单，因为我们重复的计算了中间解。我们以前的公式仍然是有效的。我们只需要一个更好的方法实现这个公式。

解决递归中堆栈溢出的问题的一个思路就是使用动态规划（DP）技术，该技术建立在重用中间解的结果来计算终解的思想之上。

#### 算法

- 几乎所有的动态规划解决方案，首先会创建一个一维或多维数组 DP 来保存中间子解的值，以及通常数组最后一个值代表最终解。
- 在主要步骤中，我们从数字 ```1``` 循环到 ```n```，计算每个数字 ```i``` 的解（即 ```numSquares(i)```）。每次迭代中，我们将 ```numSquares(i)``` 的结果保存在 ```dp[i]``` 中。
- 在循环结束时，我们返回数组中的最后一个元素作为解决方案的结果。

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWMubGVldGNvZGUtY24uY29tL0ZpZ3VyZXMvMjc5LzI3OV9kcC5wbmc?x-oss-process=image/format,png)

python

```python
class Solution(object):
    def numSquares(self, n):
        square_nums = [i**2 for i in range(0, int(math.sqrt(n))+1)]
        
        dp = [float('inf')] * (n+1)
        dp[0] = 0
        
        for i in range(1, n+1):
            for square in square_nums:
                if i < square:
                    break
                dp[i] = min(dp[i], dp[i-square] + 1)
        
        return dp[-1]
```

c++

```c++
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n+1, INT_MAX);
        dp[0] = 0;

        vector<int> squares;
        for (int i = 1; i <= sqrt(n); i++)
            squares.push_back(pow(i, 2));
        
        for (int i = 1; i <= n; i++){
            for (const int& square : squares){
                if (i < square) break;
                dp[i] = min(dp[i], dp[i - square] + 1);
            }
        }
        
        return dp[n];
    }
};
```

#### 复杂度分析
时间复杂度：O(n⋅ sqrt(n))，在主步骤中，我们有一个嵌套循环，其中外部循环是 n 次迭代，而内部循环最多需要 sqrt{n} 迭代。  
空间复杂度：O(n)，使用了一个一维数组 dp。

#### 提交
python

执行用时：5780 ms, 在所有 Python3 提交中击败了17.56%的用户

内存消耗：13.4 MB, 在所有 Python3 提交中击败了76.21%的用户

c++

执行用时：252 ms, 在所有 C++ 提交中击败了28.07%的用户

内存消耗：9.1 MB, 在所有 C++ 提交中击败了20.48%的用户

### 方法三：贪心枚举

递归解决方法为我们理解问题提供了简洁直观的方法。我们仍然可以用递归解决这个问题。为了改进上述暴力枚举解决方案，我们可以在递归中加入贪心。我们可以将枚举重新格式化如下：

从一个数字到多个数字的组合开始，一旦我们找到一个可以组合成给定数字 ```n``` 的组合，那么我们可以说我们找到了最小的组合，因为我们贪心的从小到大的枚举组合。

为了更好的解释，我们首先定义一个名为 ```is_divided_by(n, count)``` 的函数，该函数返回一个布尔值，表示数字 ```n``` 是否可以被一个数字 ```count``` 组合，而不是像前面函数 ```numSquares(n)``` 返回组合的确切大小。

下面是一个关于函数 ```is_divided_by(n, count)``` 的例子，它对 输入 ```n=5``` 和 ```count=2``` 进行了分解。

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWMubGVldGNvZGUtY24uY29tL0ZpZ3VyZXMvMjc5LzI3OV9ncmVlZHkucG5n?x-oss-process=image/format,png)

python

```python
class Solution:
    def numSquares(self, n):
        
        def is_divided_by(n, count):
            """
                return: true if "n" can be decomposed into "count" number of perfect square numbers.
                e.g. n=12, count=3:  true.
                     n=12, count=2:  false
            """
            if count == 1:
                return n in square_nums
            
            for k in square_nums:
                if n > k and is_divided_by(n - k, count - 1):
                    return True
            return False

        square_nums = set([i * i for i in range(1, int(n**0.5)+1)])
    
        for count in range(1, n+1):
            if is_divided_by(n, count):
                return count
```

c++
```c++
class Solution {
private:
    set<int> squares;

public:
    bool canDivide(int n, int count) {
        if (count == 1)
            return squares.find(n) != squares.end();
        
        for (const int& i : squares) 
            if (n > i && canDivide(n - i, count - 1))
                return true;
        return false;
    }

    int numSquares(int n) {
        for (int i = 1; i <= sqrt(n); i++)
            squares.insert(pow(i, 2));
        
        for (int i = 1; i <= n; i++)
            if (canDivide(n, i))
                return i;
        
        return 0;
    }
};
```

#### 提交
python

执行用时：68 ms, 在所有 Python3 提交中击败了92.27%的用户

内存消耗：15.5 MB, 在所有 Python3 提交中击败了18.41%的用户

c++

执行用时：16 ms, 在所有 C++ 提交中击败了93.60%的用户

内存消耗：6.9 MB, 在所有 C++ 提交中击败了84.74%的用户

Attention:  
- set相比于list可以降低时间复杂度

### 方法四：贪心＋BFS

正如上述贪心算法的复杂性分析种提到的，调用堆栈的轨迹形成一颗 N 元树，其中每个结点代表 ```is_divided_by(n, count)``` 函数的调用。基于上述想法，我们可以把原来的问题重新表述如下：

给定一个 N 元树，其中每个节点表示数字 n 的余数减去一个完全平方数，我们的任务是在树中找到一个节点，该节点满足两个条件：

(1) 节点的值（即余数）也是一个完全平方数。  
(2) 在满足条件（1）的所有节点中，节点和根之间的距离应该最小。

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWMubGVldGNvZGUtY24uY29tL0ZpZ3VyZXMvMjc5LzI3OV9ncmVlZHlfdHJlZS5wbmc?x-oss-process=image/format.png)

遍历的顺序是 BFS，而不是 DFS（深度优先搜索），这是因为在用尽固定数量的完全平方数分解数字 n 的所有可能性之前，我们不会探索任何需要更多元素的潜在组合。

#### 算法

- 首先，我们准备小于给定数字 ```n``` 的完全平方数列表（即 ```square_nums```）。
- 然后创建 ```queue``` 遍历，该变量将保存所有剩余项在每个级别的枚举。
- 在主循环中，我们迭代 ```queue``` 变量。在每次迭代中，我们检查余数是否是一个完全平方数。如果余数不是一个完全平方数，就用其中一个完全平方数减去它，得到一个新余数，然后将新余数添加到 ```next_queue``` 中，以进行下一级的迭代。一旦遇到一个完全平方数的余数，我们就会跳出循环，这也意味着我们找到了解。

注意：在典型的 BFS 算法中，```queue``` 变量通常是数组或列表类型。但是，这里我们使用 ```set``` 类型，以消除同一级别中的剩余项的冗余。事实证明，这个小技巧甚至可以增加 5 倍的运行加速。

python

```python
class Solution:
    def numSquares(self, n):

        # list of square numbers that are less than `n`
        square_nums = [i * i for i in range(1, int(n**0.5)+1)]
    
        level = 0
        queue = {n}
        while queue:
            level += 1
            #! Important: use set() instead of list() to eliminate the redundancy,
            # which would even provide a 5-times speedup, 200ms vs. 1000ms.
            next_queue = set()
            # construct the queue for the next level
            for remainder in queue:
                for square_num in square_nums:    
                    if remainder == square_num:
                        return level  # find the node!
                    elif remainder < square_num:
                        break
                    else:
                        next_queue.add(remainder - square_num)
            queue = next_queue
        return level
```

执行用时：160 ms, 在所有 Python3 提交中击败了88.13%的用户

内存消耗：15.8 MB, 在所有 Python3 提交中击败了15.46%的用户

c++

```c++
class Solution {
private:
    int level = 0;
    set<int> squares;

public:
    int numSquares(int n) {
        vector<int> remains = {n};

        for (int i = 1; i <= sqrt(n); i++)
            squares.insert(pow(i, 2));
        
        while(!remains.empty()){
            level++;
            vector<int> new_remains;

            for (const int& remain : remains)
                for (const int& square : squares){
                    if (remain == square) return level;
                    else if (remain < square) break;
                    else new_remains.push_back(remain - square);
                }
                
            remains = new_remains;
        }
        
        return 0;
    }
};
```

执行用时：148 ms, 在所有 C++ 提交中击败了63.97%的用户

内存消耗：111.7 MB, 在所有 C++ 提交中击败了4.97%的用户
