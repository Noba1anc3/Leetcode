在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。



Examples:

```
输入: [7,5,6,4]
输出: 5
```

## Solution

## python

```python
class Solution:
    def reversePairs(self, nums) -> int:
        return self.SortCount(nums)

    def SortCount(self, nums):
        if len(nums) <= 1:
            return 0
		
        mid = int(len(nums) / 2)
        A = nums[:mid]
        B = nums[mid:]

        rA = self.SortCount(A)
        rB = self.SortCount(B)
        rC = self.MergeCount(A, B)

        return rA + rB + rC

    def MergeCount(self, listA, listB):
        i, j, rC = 0, 0, 0

        listA = sorted(listA)
        listB = sorted(listB)

        while i < len(listA) and j < len(listB):
            if listA[i] <= listB[j]:
                i += 1
            else:
                rC += len(listA) - i
                j += 1

        return rC
```

执行用时：1192 ms, 在所有 Python3 提交中击败了93.63%的用户

内存消耗：20 MB, 在所有 Python3 提交中击败了63.16%的用户

Attention:
- del 耗时
- 递归的边界返回

### c++

```c++
class Solution {
public:
    int reversePairs(vector<int>& nums) {
        return count(nums);
    }
    
    int count(vector<int>& nums){
        if (nums.size() <= 1)
            return 0;
        
        int mid = nums.size() / 2;
        vector<int> numsA(nums.begin(), nums.begin() + mid);
        vector<int> numsB(nums.begin() + mid, nums.end());

        int num1 = count(numsA);
        int num2 = count(numsB);
        int num3 = mergecount(numsA, numsB);

        return num1 + num2 + num3;
    }

    int mergecount(vector<int>& numsA, vector<int>& numsB){
        sort(numsA.begin(), numsA.end());
        sort(numsB.begin(), numsB.end());

        int i = 0, j = 0, num = 0;

        while (i < numsA.size() && j < numsB.size()){
            if (numsA[i] <= numsB[j])
                i++;
            else{
                num += numsA.size() - i;
                j++;
            }
        }

        return num;
    }
};
```

执行用时：644 ms, 在所有 C++ 提交中击败了11.25%的用户

内存消耗：136.9 MB, 在所有 C++ 提交中击败了11.41%的用户
