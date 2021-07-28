Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.

Implement the LRUCache class:

- LRUCache(int capacity) Initialize the LRU cache with positive size capacity.
- int get(int key) Return the value of the key if the key exists, otherwise return -1.
- void put(int key, int value) Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.
The functions get and put must each run in O(1) average time complexity.

The functions get and put must each run in O(1) average time complexity.

**Example:**

```
Example 1:

Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]

Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

## Solution

```c++
class LRUCache{
private:
    int capacity;           // cache capacity
    struct KeyVal{
        int key; int value;
    };
    list<KeyVal> cache;     // cache, implement in Double-Linked-List
    unordered_map<int, list<KeyVal>::iterator> cacheMap;    // Hash Map, quickly access to value

public:
    LRUCache(int capacity):
        capacity(capacity)
    { }

    int get(int key) {
        auto it = cacheMap.find(key);
        // Find by key
        if (cacheMap.find(key) != cacheMap.end()) {
            //Find it! Get the value.
            auto kv = it->second;
            // Move to front
            if (kv != cache.begin())
                cache.splice(cache.begin(), cache, kv, next(kv));
            return kv->value;
        }
        return -1;
    }

    void put(int key, int value) {
        auto it = cacheMap.find(key);
        // Find by key
        if (it != cacheMap.end()){
            // find and set new value
            auto kv = it->second;
            kv->value = value;
            // move front
            if (kv != cache.begin())
                cache.splice(cache.begin(), cache, kv, next(kv));
        }
        else {
            // Not found
            if (cacheMap.size() < capacity) {
                KeyVal newKv = {key, value};
                // set in cache
                cache.push_front(newKv);
                // add to map
                cacheMap.insert(make_pair(key, cache.begin()));
            }
            else {
                // Capacity is not enough
                // Delete the least used value
                int oldKey = cache.back().key;
                cache.pop_back();
                cacheMap.erase(oldKey);

                // Add new value
                KeyVal newKv = {key, value};
                cache.push_front(newKv);
                cacheMap.insert(make_pair(key, cache.begin()));
            }
        }
    }
};
```

执行用时：360 ms, 在所有 C++ 提交中击败了55.55%的用户

内存消耗：161.2 MB, 在所有 C++ 提交中击败了26.26%的用户
