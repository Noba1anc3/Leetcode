```c++
int get_minIdx(string s){
    unordered_map<char, pair<int, bool>> M;
    for (int i = 0; i < s.size(); i++)
        if (M.count(s[i]))
            M[s[i]].second = false;
        else
            M[s[i]] = make_pair(i, true);

    int minIdx = INT_MAX;
    unordered_map<char, pair<int, bool>>::iterator it;
    
    for (it = M.begin(); it != M.end(); it++)
        if ((*it).second.second)
            minIdx = min(minIdx, (*it).second.first);
    
    return minIdx;
}
```
