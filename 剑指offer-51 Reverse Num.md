在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。



Examples:

```
输入: [7,5,6,4]
输出: 5
```

## Solution

python

```c++
class Solution:
    def reversePairs(self, nums) -> int:
        totalReverse, _ = self.SortCount(nums)
        return totalReverse

    def SortCount(self, nums):
        if len(nums) <= 1:
            return 0, nums
		
        mid = int(len(nums) / 2)
        A = nums[:mid]
        B = nums[mid:]

        rA, A = self.SortCount(A)
        rB, B = self.SortCount(B)
        rC, nums = self.MergeCount(A, B)

        return rA + rB + rC, nums

    def MergeCount(self, listA, listB):
        i, j, rC = 0, 0, 0

        listA = sorted(listA)
        listB = sorted(listB)
        final_list = sorted(listA + listB)

        while i < len(listA) and j < len(listB):
            eleA = listA[i]
            eleB = listB[j]

            if eleA <= eleB:
                i += 1
            else:
                rC += len(listA) - i
                j += 1

        return rC, final_list
```

执行用时：1644 ms, 在所有 Python3 提交中击败了65.55%的用户

内存消耗：18.4 MB, 在所有 Python3 提交中击败了47.07%的用户

Attention:
- del 耗时
- 递归的边界返回

