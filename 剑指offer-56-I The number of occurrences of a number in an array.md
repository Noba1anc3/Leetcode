 一个整型数组 `nums` 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。 



 **Example:**

```
示例 1：

输入：nums = [4,1,4,6]
输出：[1,6] 或 [6,1]
示例 2：

输入：nums = [1,2,10,4,1,4,3,3]
输出：[2,10] 或 [10,2]
```

## Solution - I Hash
### c++

```c++
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums) {
        vector<int> ans;
        unordered_set<int> hash;

        for (int num : nums){
            if (hash.count(num))
                hash.erase(num);
            else
                hash.insert(num);
        }

        for(int num : hash)
            ans.push_back(num);
        
        return ans;
    }
};
```

执行用时：44 ms, 在所有 C++ 提交中击败了38.43%的用户

内存消耗：18.8 MB, 在所有 C++ 提交中击败了14.05%的用户

Attention

- ```
  unordered_set.erase()
  ```

## Idea - II - 按位异或

让我们先来考虑一个比较简单的问题：

如果除了一个数字以外，其他数字都出现了两次，那么如何找到出现一次的数字？

答案很简单：全员进行**异或**操作即可。



考虑异或操作的性质：对于两个操作数的每一位，相同结果为 0，不同结果为 1。

那么在计算过程中，成对出现的数字的所有位会两两抵消为 0，最终得到的结果就是那个出现了一次的数字。

那么这一方法如何扩展到找出两个出现一次的数字呢？



如果我们可以把所有数字分成两组，使得：

- 两个只出现一次的数字在不同的组中；

- 相同的数字会被分到相同的组中。

 那么对两个组分别进行异或操作，即可得到答案的两个数字。**这是解决这个问题的关键。** 



那么如何实现这样的分组呢？

记这两个只出现了一次的数字为 a 和 b，那么所有数字异或的结果就等于 a 和 b 异或的结果，我们记为 x。

如果我们把 x 写成二进制的形式
$$
x_k x_{k - 1} \cdots x_2 x_1 x_0, 其中x_i \in \{ 0, 1 \}
$$
我们考虑一下 x_i = 0和 x_i = 1的含义是什么？它意味着如果我们把 a 和 b 写成二进制的形式，a_i 和 b_i 的关系—— x_i = 1 表示 a_i 和 b_i 不等，x_i = 0 表示 a_i 和 b_i 相等。

假如我们任选一个不为 0 的 x_i，按照第 i 位给原来的序列分组，如果该位为 0 就分到第一组，否则就分到第二组，这样就能满足以上两个条件，为什么呢？

- 首先，两个相同的数字的对应位都是相同的，所以一个被分到了某一组，另一个必然被分到这一组，所以满足了条件 2。

- 这个方法在 x_i = 1 的时候 a 和 b 不被分在同一组，因为 x_i = 1 表示 a_i 和 b_i 不等，根据这个方法的定义「如果该位为 0 就分到第一组，否则就分到第二组」可以知道它们被分进了两组，所以满足了条件 1。

**算法**

先对所有数字进行一次异或，得到两个出现一次的数字的异或值。

在异或结果中找到任意为 1 的位。

根据这一位对所有的数字进行分组。

在每个组内进行异或操作，得到两个数字。

## Solution - II

### c++

```c++
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums) {
        int ret = 0;
        for (int n : nums)
            ret ^= n;

        int div = 1;
        while ((div & ret) == 0)
            div <<= 1;

        int a = 0, b = 0;
        for (int n : nums)
            if (div & n)
                a ^= n;
            else
                b ^= n;
                
        return vector<int>{a, b};
    }
};
```

执行用时：20 ms, 在所有 C++ 提交中击败了93.28%的用户

内存消耗：15.7 MB, 在所有 C++ 提交中击败了91.98%的用户

Attention:

- ```c++
  int ret = 0;
  for (int n : nums)
  	ret ^= n;
  ```

- ```c++
  int div = 1;
  while ((div & ret) == 0)
  	div <<= 1;
  ```