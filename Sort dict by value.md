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
    vector<pair<string, int>> tmp (dict.begin(), dict.end());

    sort(tmp.begin(), tmp.end(), cmp);
    for (pair<string, int>& item : tmp)
        cout<<item.first<<item.second<<'\n';

    return 0;
}
```

