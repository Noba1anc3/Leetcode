写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项。斐波那契数列的定义如下：

```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
```

斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。



**Example:**

```
输入：n = 2
输出：1

输入：n = 5
输出：5
```

## Solution - I Recursive

c++

```c++
class Solution {
public:
    int fib(int n) {
        if (n < 2)
            return n;

        return (fib(n-1) + fib(n-2)) % (int)1e9+7;
    }
};
```

执行结果：超出时间限制

最后执行的输入：43

## Solution - II DP

c++
```c++
class Solution {
public:
    int fib(int n) {
        if (n < 2) return n;

        vector<int> fibonacci(n+1, 0);
        fibonacci[1] = 1;
        
        for (int i = 2; i <= n; i++)
            fibonacci[i] = (fibonacci[i-1] + fibonacci[i-2]) % (int)(1e9+7);

        return fibonacci[n];
    }
};
```
执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：6.1 MB, 在所有 C++ 提交中击败了15.67%的用户

## Solution - III Space Complexity Reduced DP

c++

```c++
class Solution {
public:
    int fib(int n) {
        if (n < 2)
            return n;

        int minusTwo = 0, minusOne = 1, minusZero = 1;

        for (int i = 2; i <= n; i++){
            minusZero = (minusOne + minusTwo) % (int)(1e9+7);
            minusTwo = minusOne;
            minusOne = minusZero;
        }
        
        return minusOne;
    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：6.3 MB, 在所有 C++ 提交中击败了16.92%的用户
