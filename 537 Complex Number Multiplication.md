Given two strings representing two complex numbers.

You need to return a string representing their multiplication. Note i2 = -1 according to the definition.



```
Example 1:

Input: "1+1i", "1+1i"
Output: "0+2i"
Explanation: (1 + i) * (1 + i) = 1 + i2 + 2 * i = 2i, and you need convert it to the form of 0+2i.

Example 2:

Input: "1+-1i", "1+-1i"
Output: "0+-2i"
Explanation: (1 - i) * (1 - i) = 1 + i2 - 2 * i = -2i, and you need convert it to the form of 0+-2i.
```

## Solution

python

```python
class Solution:
    def complexNumberMultiply(self, a: str, b: str) -> str:
        ｍ = int(a.split('+')[0])
        n = int(b.split('+')[0])
        p = int(a.split('+')[1].split('i')[0])
        q = int(b.split('+')[1].split('i')[0])

        mn = m*n
        pq = p*q*(-1)
        mq = m*q
        np = n*p

        return str(mn + pq) + '+' + str(np + mq) + 'i'
```

执行用时：36 ms, 在所有 Python3 提交中击败了83.27%的用户

内存消耗：13.6 MB, 在所有 Python3 提交中击败了9.75%的用户