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

## Idea - I Mathematics + Greedy
下面有图
![](http://r.photo.store.qq.com/psc?/V50VqFfH2A6OlZ2gWBDL0uxzNK4WmFgm/TmEUgtj9EK6.7V8ajmQrEMaKYGgFOh3O8Y3xGT.bwxh9E7OOsrwEm1T1o7A3suSmpRrBXGDxfc42QH2jrVUoHmkXbFMopR*.kwSH.458Dcc!/r)

1. ① 当所有绳段长度相等时，乘积最大。② 最优的绳段长度为 3
2. 因为绳子至少剪2段或以上，且绳子长度大于1，所以当n小于等于3（也就是等于2，3）时，最大乘积分别为1，2，也就是n减去1
   定义一个变量res，用来存储乘积，初始为1
3. 当n大于4时循环遍历（因为等于4时,1x3<2x2=4,所以等于4时不能剪），每次将乘积乘以3，然后绳子长度n减去3,如果res大于1000000007，则将res对1000000007进行取模运算
4. 最后当剩余长度为4，或者3，2时剩余的最大长度都是其本身，所以再将res乘以其本身，如果res大于1000000007，则将res对1000000007进行取模运算，此即为最终结果

## Solution - I Mathematics + Greedy

java
```java
class Solution {
    public int cuttingRope(int n) {
        if (n <= 3) return n - 1;
        
        long result  = 1;

        while (n > 4){
            result *= 3;
            n -= 3;
            if (result > 1e9 + 7)
                result %= 1e9 + 7;
        }
        result *= n;
        if (result > 1e9 + 7)
            result %= 1e9 + 7;
        
        return (int)result;
    }
}
```
执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：35.2 MB, 在所有 Java 提交中击败了77.30%的用户

**复杂度分析**

- 时间复杂度 O(n) 
- 空间复杂度 O(1)

Attention

- long