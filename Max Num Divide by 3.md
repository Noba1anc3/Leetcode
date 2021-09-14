
int cal_num(vector<int> nums){
    int num = 0;
    for (int i = 0; i < nums.size(); i++)
        num += nums[i] * pow(10, nums.size() - i - 1);
    return num;
}

void erase_num(vector<int>& nums, int num){
    for (int i = nums.size() - 1; i >= 0; i--){
        if (nums[i] == num){
            nums.erase(nums.begin() + i);
            break;
        }
    }
}

int main(){
    vector<int> nums = {6, 3, 4, 1, 2, 2, 3, 5};
    sort(nums.begin(), nums.end());
    reverse(nums.begin(), nums.end());

    int one_0 = INT_MAX, one_1 = INT_MAX;
    int two_0 = INT_MAX, two_1 = INT_MAX;

    for (const int& num : nums){
        if (num % 3 == 1){
            if (num <= one_0){
                one_1 = one_0;
                one_0 = num;
            }
            else if (num < one_1){
                one_1 = num;
            }
        }
        else if (num % 3 == 2){
            if (num <= two_0){
                two_1 = two_0;
                two_0 = num;
            }
            else if (num < two_1){
                two_1 = num;
            }
        }
    }

    int num = cal_num(nums);

    if (num % 3 == 1){
        if (one_0 != INT_MAX){
            erase_num(nums, one_0);
        }
        else{
            erase_num(nums, two_0);
            erase_num(nums, two_1);
        }
        num = cal_num(nums);
    }
    else if (num % 3 == 1){
        if (two_0 != INT_MAX){
            erase_num(nums, two_0);
        }
        else{
            erase_num(nums, one_0);
            erase_num(nums, one_1);
        }
        num = cal_num(nums);
    }
  
    cout << num;
}
