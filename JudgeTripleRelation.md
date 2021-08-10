```c++
bool JudgeTripleRelation(vector<int> nums){
    int mimNum = INT_MAX, maxNum = INT_MIN;
    vector<bool> Jforward(nums.size()-1, false), JBackward(nums.size()-1, false);

    for(int i = 0; i < nums.size(); i++){
        if (nums[i] <= minNum)
            minNum = nums[i];
        else
            JForward[i] = true;

    for(int i = nums.size() - 1; i >= 0; i--){
        if (nums[i] >= maxNum)
            maxNum = nums[i];
        else
            if (JForward[i]) return true;
            else JBackward[i] = true;

    return false;
}
```
