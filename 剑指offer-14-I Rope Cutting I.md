给你一根长度为 `n` 的绳子，请把绳子剪成整数长度的 `m` 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 `k[0],k[1]...k[m-1]` 。请问 `k[0]*k[1]*...*k[m-1]` 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。



**Example:**
```
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1

输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
```

## Idea - I Mathematics

下面有图

![](http://r.photo.store.qq.com/psc?/V50VqFfH2A6OlZ2gWBDL0uxzNK4WmFgm/TmEUgtj9EK6.7V8ajmQrEMaKYGgFOh3O8Y3xGT.bwxh9E7OOsrwEm1T1o7A3suSmpRrBXGDxfc42QH2jrVUoHmkXbFMopR*.kwSH.458Dcc!/r)

**拆分规则**

- 最优： 3 。把数字 n 可能拆为多个因子 3 ，余数可能为 0, 1, 2三种情况
- 次优： 2 。若余数为 2 ；则保留，不再拆为 1 + 1
- 最差： 1 。若余数为 1 ；则应把一份 3 + 1 替换为 2 + 2，因为 2 x 2 > 3 × 1

**算法流程**

1. 当 n ≤ 3 时，按照规则应不拆分，但由于题目要求必须拆分，因此必须拆出一个因子 1 ，即返回 n - 1 。
2. 当 n > 3 时，求 n 除以 3 的 整数部分 a 和 余数部分 b （即 n = 3a + b ），并分为以下三种情况：
   - 当 b = 0 时，直接返回 3 ^ a
   - 当 b = 1 时，要将一个 1 + 3 转换为 2 + 2，因此返回 3 ^ (a - 1) x 4
   - 当 b = 2 时，返回 3 ^ a x 2

## Solution - I Mathematics

### java

```python
class Solution {
    public int integerBreak(int n) {
        if (n <= 3) return n - 1;
        
        int a = n / 3, b = n % 3;
        
        if (b == 0) return (int)Math.pow(3, a);
        if (b == 1) return (int)Math.pow(3, a-1)*4;
        return (int)Math.pow(3, a)*2;
    }
}
```
执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：35 MB, 在所有 Java 提交中击败了89.32%的用户

### c++

```c++
class Solution {
public:
    int cuttingRope(int n) {
        if (n <= 3) return n - 1;
        
        int a = n / 3, b = n % 3;
        
        if (b == 0) return pow(3, a);
        if (b == 1) return pow(3, a-1)*4;
        return pow(3, a)*2;
    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：6.1 MB, 在所有 C++ 提交中击败了78.85%的用户

**复杂度分析**

- 时间复杂度 O(1) ： 仅有求整、求余、次方运算。
  - 求整和求余运算：查阅资料，提到不超过机器数的整数可以看作是 O(1)；
  - 幂运算：查阅资料，提到浮点取幂为 O(1)。
- 空间复杂度 O(1) ： a 和 b 使用常数大小额外空间。

或写为：

```c++
class Solution {
public:
    int cuttingRope(int n) {
        if (n <= 3) return n - 1;
        
        int result  = 1;

        while (n > 4){
            result *= 3;
            n -= 3;
        }
        result *= n;

        return result;
    }
};
```

## Idea - II DP

- 明确 `dp[i] `的含义 :  `dp[i]` 表示分拆数字 `i`，可以得到的最大乘积。

- 初始化 : 初始化 `dp[i] = i`，这里的初始化不是为了让 `i` 的最大乘积是 `dp[i]` ，而是为了做递推公式的时候，`dp[i] `可以表示 `i` 这个数字，好用来做乘法。

- 递归公式 : `dp[i]`的最大乘积一定是 `dp[j]` 的最大乘积乘 `dp[i - j]` 的最大乘积，遍历 `j`，取 `dp[i] `的最大值

  `dp[i] = max(dp[i], dp[j] * dp[i - j])`

## Solution - II DP

### c++

```c++
class Solution {
public:
    int cuttingRope(int n) {
        if (n < 4) return n-1;

        int dp[n+1];
        for (int i = 0; i <= n; i++)
            dp[i] = i;
        
        for (int i = 4; i <= n; i++)
            for (int j = 2; j <= (int)i/2; j++)
                dp[i] = max(dp[i], dp[j]*dp[i-j]);

        return dp[n];
    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：5.8 MB, 在所有 C++ 提交中击败了97.55%的用户
