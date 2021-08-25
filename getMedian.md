```c++
class Median{
private:
    vector<int> nums;
    const int size;
    float median = 0;
    
	int partition(int p, int q){
        int i = p - 1;
        int pivot = nums[q];

        for (int j = p; j < q; j++)
            if (nums[j] < pivot)
                swap(nums[++i], nums[j]);

        swap(nums[++i], nums[q]);
        return i;
    }

    void Quicksort(int p, int q, int k){
        if (p < q){
            int r = partition(p, q);
            int num = q - r + 1;
            if (num < k) Quicksort(p, r - 1, k - num);
            else if (num > k) Quicksort(r + 1, q, k);
            else return;
        }
    }

    int getKth(int k){
        Quicksort(0, size - 1, k);
        return nums[size - k];
    }
    
public:
    Median(vector<int> nums_vec) : nums(nums_vec), size(nums_vec.size()){};

    float getMedian(){
        if (size % 2 == 1){
       	    int k = size / 2 + 1;
            median = getKth(k);
        }
        else{
            int k = size / 2;
            median = getKth(k);
            nums_vec = nums_vec(nums_vec.begin(), nums_vec.begin() + k);
            size /= 2;
            median += getKth(1);
            median /= 2.;
        }
        return median;
    }
};
```
