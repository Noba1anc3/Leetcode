```c++
class HeapSort {
private:
    vector<int> Heap;

public:
    int _get_parent(int i){
        if (i == 0) return 0;
        if (i % 2 == 0) return i / 2 - 1;
        else return i / 2;
    }

    void _build_heap(vector<int> nums){
        for (int i = 0; i < nums.size(); i++){
            Heap.push_back(nums[i]);
            int curIndex = i;
            while (Heap[_get_parent(curIndex)] > Heap[curIndex]){
                swap(Heap[_get_parent(curIndex)], Heap[curIndex]);
                curIndex = _get_parent(curIndex);
            }
        }
    }

    void _insert(int num){
        int curIndex = Heap.size();
        Heap.push_back(num);
        while (Heap[_get_parent(curIndex)] > Heap[curIndex]){
            swap(Heap[_get_parent(curIndex)], Heap[curIndex]);
                curIndex = _get_parent(curIndex);
        }
    }

    int _extract_min(){
        int root = 0;
        int minEle = Heap[root];
        Heap[root] = Heap[Heap.size()-1];
        Heap.pop_back();

        while (true){
            int lchild = (2*root+1 < Heap.size()) ? Heap[2*root+1] : INT_MAX;
            int rchild = (2*root+2 < Heap.size()) ? Heap[2*root+2] : INT_MAX;
            if (lchild == INT_MAX || Heap[root] <= min(lchild, rchild)) break;
            if (lchild <= min(Heap[root], rchild)){
                swap(Heap[root], Heap[2*root+1]);
                root = 2*root+1;
            }
            else if (rchild < min(Heap[root], lchild)){
                swap(Heap[root], Heap[2*root+2]);
                root = 2*root+2;
            }
        }

        return minEle;
    }

    vector<int> sortArray(vector<int>& nums) {
        vector<int> rsp;
        _build_heap(nums);
        for (int i = 0; i < nums.size(); i++)
            rsp.push_back(_extract_min());
        return rsp;
    }
};
```
