Given an array `nums` of size n, return the majority element.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.



 **Example:**

```
Example 1:

Input: nums = [3,2,3]
Output: 3
Example 2:

Input: nums = [2,2,1,1,1,2,2]
Output: 2
```

## Solution - I Hash
### c++

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int target = nums.size() / 2;
        if (nums.size() % 2 == 1)
            target++;

        unordered_map<int, int> hashmap;
        for (int num : nums){
            if (hashmap[num] + 1 == target)
                return num;
            hashmap[num]++;
        }
        return 0;
    }
};
```

执行用时：24 ms, 在所有 C++ 提交中击败了86.54%的用户

内存消耗：18.4 MB, 在所有 C++ 提交中击败了81.78%的用户

## Solution - II Vote Idea

### c++

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int x = 0, votes = 0;
        for(int num : nums){
            if(votes == 0) x = num;
            votes += num == x ? 1 : -1;
        }
        return x;
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了97.13%的用户

内存消耗：19.2 MB, 在所有 C++ 提交中击败了18.20%的用户