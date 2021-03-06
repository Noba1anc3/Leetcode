Given two arrays, write a function to compute their intersection.

Examples:

```
Example 1:

Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2, 2]

Example 2:

Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
```

Each element in the result should appear as many times as it shows in both arrays.
The result can be in any order.

## Idea

### 排序 + 双指针

如果两个数组是有序的，则可以使用双指针的方法得到两个数组的交集。

首先对两个数组进行排序，然后使用两个指针遍历两个数组。可以预见的是加入答案的数组的元素一定是递增的。

初始时，两个指针分别指向两个数组的头部。每次比较两个指针指向的两个数组中的数字，如果两个数字不相等，则将指向较小数字的指针右移一位，如果两个数字相等，将该数字添加到答案。当至少有一个指针超出数组范围时，遍历结束。

#### 复杂度

时间复杂度：`O(m log m + n log n)`，其中 `m` 和 `n` 分别是两个数组的长度。对两个数组排序的时间复杂度分别是 `O(m log m)` 和 `O(n log n)`，双指针寻找交集元素的时间复杂度是 `O(m+n)`，因此总时间复杂度是 `O( m log m + n log n)`。

空间复杂度：`O(log m + log n)`，其中 `m` 和 `n` 分别是两个数组的长度。空间复杂度主要取决于排序使用的额外空间。

## Solution

### 排序 + 双指针

c++

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        sort(nums1.begin(), nums1.end());
        sort(nums2.begin(), nums2.end());

        int len1 = nums1.size(), len2 = nums2.size();
        int index1 = 0, index2 = 0;

        vector<int> interSection;
        while (index1 < len1 && index2 < len2){
            if (nums1[index1] < nums2[index2])
                index1 += 1;
            else if (nums1[index1] > nums2[index2])
                index2 += 1;
            else{
                interSection.push_back(nums1[index1]);
                index1 += 1;
                index2 += 1;
            }
        }

        return interSection;
    }
};
```

执行用时：8 ms, 在所有 C++ 提交中击败了56.39%的用户

内存消耗：9.8 MB, 在所有 C++ 提交中击败了66.38%的用户

### 排序 + 真双指针（不是

```c++
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        sort(nums1.begin(), nums1.end());
        sort(nums2.begin(), nums2.end());

        vector<int>::iterator it1 = nums1.begin(), it2 = nums2.begin();
        vector<int> interSection;
        
        while (it1 < nums1.end() && it2 < nums2.end()){
            if (*it1 < *it2)
                *it1++;
            else if (*it1 > *it2)
                *it2++;
            else{
                interSection.push_back(*it1);
                *it1++;
                *it2++;
            }
        }

        return interSection;
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了91.53%的用户

内存消耗：9.6 MB, 在所有 C++ 提交中击败了96.67%的用户

### 突发奇想 利用双Map计算交集

```c++
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        vector<int> interSection;
        unordered_map<int, int> M1, M2;
        for (const int& num : nums1) M1[num]++;
        for (const int& num : nums2) M2[num]++;

        unordered_map<int, int>::iterator it1 = M1.begin();

        while (it1 != M1.end()){
            if (M2.find(it1->first) != M2.end())
                for (int i = 0; i < min(it1->second, M2[it1->first]); i++)
                    interSection.push_back(it1->first);
            it1++;
        }

        return interSection;
    }
};
```

执行用时：8 ms, 在所有 C++ 提交中击败了56.39%的用户

内存消耗：10.5 MB, 在所有 C++ 提交中击败了6.58%的用户
