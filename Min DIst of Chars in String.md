## Solution

给定一个字符串，两个字符，用O(n)的时间复杂度求出两字符之间的最短距离

```c++
int get_min_dist(string s, char x, char y){
    int min_dist = INT_MAX;
    int nearest_x = INT_MIN, nearest_y = INT_MIN;
    for (int i = 0; i < s.size(); i++){
        if (s[i] == x){
            min_dist = min(min_dist, abs(i - nearest_y));
            nearest_x = i;
        }
        if (s[i] == y){
            min_dist = min(min_dist, abs(i - nearest_x));
            nearest_y = i;
        }
		}
    return min_dist;
}    
```
