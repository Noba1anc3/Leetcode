```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <unordered_map>

using namespace std;

const bool cmp(pair<string, int> a, pair<string, int> b){
    return a.second < b.second;
}

int main()
{
    unordered_map<string, int> dict = {{"a",4},{"c",2},{"b",3}};
    vector<pair<string, int>> dict_vector(dict.begin(), dict.end());

    sort(dict_vector.begin(), dict_vector.end(), cmp);
    for (const pair<string, int>& item : dict_vector)
        cout<<item.first<<item.second<<'\n';

    return 0;
}
```

