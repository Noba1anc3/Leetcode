You are driving a vehicle that has `capacity` empty seats initially available for passengers.  The vehicle **only** drives east (ie. it **cannot** turn around and drive west.)

Given a list of `trips`, `trip[i] = [num_passengers, start_location, end_location]` contains information about the `i`-th trip: the number of passengers that must be picked up, and the locations to pick them up and drop them off.  The locations are given as the number of kilometers due east from your vehicle's initial location.

Return `true` if and only if it is possible to pick up and drop off all passengers for all the given trips. 

 

**Example:**

```
Example 1:

Input: trips = [[2,1,5],[3,3,7]], capacity = 4
Output: false

Example 2:

Input: trips = [[2,1,5],[3,3,7]], capacity = 5
Output: true

Example 3:

Input: trips = [[2,1,5],[3,5,7]], capacity = 3
Output: true

Example 4:

Input: trips = [[3,2,7],[3,7,9],[8,3,9]], capacity = 11
Output: true
```

## Idea - I Priority Queue

按照开始位置排序，优先队列存放结束位置

## Solution - I Priority Queue

```c++
class Solution {
public:
    static bool cmp(vector<int> tripA, vector<int> tripB){
        return tripA[1] < tripB[1];
    }
    
    bool carPooling(vector<vector<int>>& trips, int capacity) {
        if (trips.empty())
            return true;

        sort(trips.begin(), trips.end(), cmp);

        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> Q;

        for (const vector<int>& trip : trips){
            while (!Q.empty() && trip[1] >= Q.top().first){  // 下客
                capacity += Q.top().second;
                Q.pop();
            }

            capacity -= trip[0];
            if (capacity < 0)
                return false;
            
            pair<int, int> curTrip(trip[2], trip[0]);
            Q.push(curTrip);
        }

        return true;
    }
};
```

执行用时：44 ms, 在所有 C++ 提交中击败了18.41%的用户

内存消耗：15.1 MB, 在所有 C++ 提交中击败了5.05%的用户

Attention

- pair.first
- pair.second
- pair<int, int> pairname(int a, int b)

## Solution - II Brute Force

```c++
class Solution {
public:
    bool carPooling(vector<vector<int>>& trips, int capacity) {
        vector<int> location(1001, 0);

        for (const vector<int>& trip : trips){
            for (int i = trip[1]; i < trip[2]; i++){
                location[i] += trip[0];
                if (location[i] > capacity)
                    return false;
            }
        }

        return true;
    }
};
```

执行用时：32 ms, 在所有 C++ 提交中击败了13.50%的用户

内存消耗：9.5 MB, 在所有 C++ 提交中击败了9.9%的用户

## Solution - III Time Deduced Brute Force

人员变化只发生在上下车的时候，因此不需要存中间信息

```c++
class Solution {
public:
    bool carPooling(vector<vector<int>>& trips, int capacity) {
        vector<int> location(1001, 0);

        for (auto trip : trips){
            location[trip[1]] += trip[0];
            location[trip[2]] -= trip[0];
        }

        for (auto loc : location){
            capacity -= loc;
            if (capacity < 0)
                return false;
        }

        return true;
    }
};
```

执行用时：8 ms, 在所有 C++ 提交中击败了85.64%的用户

内存消耗：9.5 MB, 在所有 C++ 提交中击败了9.9%的用户
