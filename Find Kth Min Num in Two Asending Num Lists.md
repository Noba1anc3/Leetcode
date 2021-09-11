## Solution

```c++
class Solution {
public:
    int find_kth(vector<int>& arr1, vector<int>& arr2, int k) {
        int m = arr1.size(), n = arr2.size();
        int index1 = 0, index2 = 0;
        
        while(true){
            if (index1 == m)
                return arr2[index2 + k - 1];
            if (index2 == n)
                return arr1[index1 + k - 1];
            if (k == 1)
                return min(arr1[index1], arr2[index2]);

            int newIndex1 = min(index1 + k / 2 - 1, m - 1);
            int newIndex2 = min(index2 + k / 2 - 1, n - 1);

            if (arr1[newIndex1] <= arr2[newIndex2]){
                k -= newIndex1 - index1 + 1;
                index1 = newIndex1 + 1;
            }
            else{
                k -= newIndex2 - index2 + 1;
                index2 = newIndex2 + 1;
            }
        }
    }
};
```
