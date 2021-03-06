Given two arrays, write a function to compute their intersection.

Examples:

```
Example 1:

Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]

Example 2:

Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
```

## Idea

### Baseline

计算两个数组的交集，直观的方法是遍历数组 `nums1`，对于其中的每个元素，遍历数组 `nums2` 判断该元素是否在数组 `nums2` 中，如果存在，则将该元素添加到返回值。假设数组 `nums1` 和 `nums2` 的长度分别是 `m` 和 `n`，则遍历数组 `nums1` 需要 `O(m)` 的时间，判断 `nums1` 中的每个元素是否在数组 `nums2` 中需要 `O(n)` 的时间，因此总时间复杂度是 `O(mn)`。

### Two Sets (Hashmap)

如果使用哈希集合存储元素，则可以在 `O(1)` 的时间内判断一个元素是否在集合中，从而降低时间复杂度。

首先使用两个集合分别存储两个数组中的元素，然后遍历较小的集合，判断其中的每个元素是否在另一个集合中，如果元素也在另一个集合中，则将该元素添加到返回值。该方法的时间复杂度可以降低到 `O(m+n)`。

#### 复杂度

时间复杂度：`O(m+n)`，其中 `m` 和 `n` 分别是两个数组的长度。使用两个集合分别存储两个数组中的元素需要 `O(m+n)` 的时间，遍历较小的集合并判断元素是否在另一个集合中需要 `O(min(m,n))` 的时间，因此总时间复杂度是 `O(m+n)`。

空间复杂度：`O(m+n)`，其中 `m` 和 `n` 分别是两个数组的长度。空间复杂度主要取决于两个集合。

### 排序 + 双指针

如果两个数组是有序的，则可以使用双指针的方法得到两个数组的交集。

首先对两个数组进行排序，然后使用两个指针遍历两个数组。可以预见的是加入答案的数组的元素一定是递增的。

初始时，两个指针分别指向两个数组的头部。每次比较两个指针指向的两个数组中的数字，如果两个数字不相等，则将指向较小数字的指针右移一位，如果两个数字相等，将该数字添加到答案，同时将两个指针都右移一位。当至少有一个指针超出数组范围时，遍历结束。

#### 复杂度

时间复杂度：`O(m log m + n log n)`，其中 `m` 和 `n` 分别是两个数组的长度。对两个数组排序的时间复杂度分别是 `O(m log m)` 和 `O(n log n)`，双指针寻找交集元素的时间复杂度是 `O(m+n)`，因此总时间复杂度是 `O( m log m + n log n)`。

空间复杂度：`O(log m + log n)`，其中 `m` 和 `n` 分别是两个数组的长度。空间复杂度主要取决于排序使用的额外空间。

## Solution

### Baseline

#### c++

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> rsp_set;
        vector<int> rsp;
        
        for (const int& num1 : nums1)
            for (const int& num2 : nums2)
                if (num1 == num2)
                    rsp_set.insert(num1);

        for (const int& ele : rsp_set)
            rsp.push_back(ele);
            
        return rsp;
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了24.70%的用户

内存消耗：9.9 MB, 在所有 C++ 提交中击败了76.27%的用户

### Hashmap

#### c++

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> set1, set2;
        
        for (const int& num : nums1)
            set1.insert(num);
        for (const int& num : nums2)
            set2.insert(num);
        
        return getIntersection(set1, set2);
    }

    vector<int> getIntersection(unordered_set<int>& set1, unordered_set<int>& set2) {
        if (set1.size() > set2.size())
            return getIntersection(set2, set1);

        vector<int> intersection;
        
        for (const int& num : set1)
            if (set2.count(num))
                intersection.push_back(num);

        return intersection;
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了24.70%的用户

内存消耗：10.4 MB, 在所有 C++ 提交中击败了25.07%的用户

#### python

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return self.set_intersection(set(nums1), set(nums2))

    def set_intersection(self, set1, set2):
        if len(set1) > len(set2):
            return self.set_intersection(set2, set1)
        return [x for x in set1 if x in set2]
```

执行用时：32 ms, 在所有 Python3 提交中击败了97.37%的用户

内存消耗：15.1 MB, 在所有 Python3 提交中击败了17.65%的用户

### 排序 + 双指针

#### 双指针

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
            if (index1 + 2 <= len1 && nums1[index1] == nums1[index1+1]){
                index1 += 1;
                continue;
            }
            if (index2 + 2 <= len2 && nums2[index2] == nums2[index2+1]){
                index2 += 1;
                continue;
            }

            if (nums1[index1] < nums2[index2])
                index1 += 1;
            else if (nums1[index1] > nums2[index2])
                index2 += 1;
            else{
                interSection.push_back(nums1[index1]);
                index1 += 1; index2 += 1;
            }
        }

        return interSection;
    }
};
```

执行用时：8 ms, 在所有 C++ 提交中击败了63.97%的用户

内存消耗：9.8 MB, 在所有 C++ 提交中击败了80.93%的用户


#### 双迭代器

set自带排序，因此无需再次排序

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        vector<int> interSection;
        set<int> nums1_set, nums2_set;

        for (const int& num : nums1)
            nums1_set.insert(num);
        for (const int& num : nums2)
            nums2_set.insert(num);

        set<int>::iterator iter1 = nums1_set.begin(), iter2 = nums2_set.begin();

        while (iter1 != nums1_set.end() && iter2 != nums2_set.end())
            if (*iter1 < *iter2) iter1++;
            else if (*iter1 > *iter2) iter2++;
            else{
                interSection.push_back(*iter1);
                iter1++; iter2++;
            }

        return interSection;
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了94.06%的用户

内存消耗：10.6 MB, 在所有 C++ 提交中击败了5.02%的用户
