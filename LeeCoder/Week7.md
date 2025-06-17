### [78. 子集](https://leetcode.cn/problems/subsets/)
- 回溯
```cpp
class Solution {  
public:  
    vector<vector<int>> subsets(vector<int>& nums) {  
        vector<vector<int>> res;  
        vector<int> temp;  
        recur(nums, res, 0, temp);  
        return res;  
    }  
  
    void recur(vector<int> &nums, vector<vector<int>> &res, int start, vector<int> temp) {  
        res.push_back(temp);  
        for (int i = start; i < nums.size(); i++) {  
            temp.push_back(nums[i]);   // 选择当前元素
            recur(nums, res, i + 1, temp);  // 递归处理后续元素
            temp.pop_back();  // 回溯撤销当前元素
        }  
    }  
};

// 回溯算法模板
void backtrack(vector<int>& path, vector<bool>& used, vector<vector<int>>& res, const vector<int>& nums) {
    if (path.size() == nums.size()) {
        res.push_back(path);
        return;
    }

    for (int i = 0; i < nums.size(); ++i) {
        if (used[i]) continue; // 剪枝：已经使用过
        used[i] = true;
        path.push_back(nums[i]);
        backtrack(path, used, res, nums);
        path.pop_back();       // 撤销选择
        used[i] = false;
    }
}
```
>[!note]
>回溯算法为’‘试探+撤销’‘