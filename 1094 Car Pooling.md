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
    bool carPooling(vector<vector<int>>& trips, int capacity) {
        if (trips.empty())
            return true;
        if (trips[0][0] > capacity)
            return false;

        sort(trips.begin(), trips.end(), [](const vector<int> tripA, const vector<int> tripB){
            return tripA[1] < tripB[1];
        });

        priority_queue<pair<int, int>> Q;
        pair<int, int> FirstTrip(-1*trips[0][2], trips[0][0]);
        capacity -= trips[0][0];
        Q.push(FirstTrip);

        for (int i = 1; i < trips.size(); i++){
            pair<int, int> trip(-1*trips[i][2], trips[i][0]);
            
            if (trips[i][1] >= -1*Q.top().first){
                while (!Q.empty() && trips[i][1] >= -1*Q.top().first){
                    capacity += Q.top().second;
                    Q.pop();
                }
                capacity -= trip.second;
                if (capacity < 0)
                    return false;
                Q.push(trip);
            }
            else if (capacity >= trip.second){
                capacity -= trip.second;
                Q.push(trip);
            }
            else{
                return false;
            }
        }

        return true;
    }
};
```

执行用时：76 ms, 在所有 C++ 提交中击败了9.54%的用户

内存消耗：15.5 MB, 在所有 C++ 提交中击败了5.04%的用户

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

        for (auto trip : trips){
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

执行用时：52 ms, 在所有 C++ 提交中击败了14.50%的用户

内存消耗：9.5 MB, 在所有 C++ 提交中击败了18.55%的用户

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

执行用时：16 ms, 在所有 C++ 提交中击败了73.64%的用户

内存消耗：9.5 MB, 在所有 C++ 提交中击败了17.95%的用户