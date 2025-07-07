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

### [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)
- 滑动窗口
```cpp
class Solution {  
public:  
    int lengthOfLongestSubstring(string s) {  
        unordered_map<char, int> seen;  
        int left = 0, right = 0;  
        int res = 0;  
        int n = s.size();  
        while (right < n) {  
            char c = s[right];  
            right++;  
            seen[c]++;  
  
            while (seen[c] > 1) {  
                char d = s[left];  
                seen[d]--;  
                left++;  
            }  
            res = max(res, right - left);  
        }  
        return res;  
    }  
};


// 滑动窗口模板
string minWindow(string s, string t) {
    unordered_map<char, int> need;   // 记录目标串t中每个字符需要的次数
    unordered_map<char, int> window; // 记录窗口中每个字符的出现次数

    // 初始化need表
    for (char c : t) need[c]++;

    int left = 0, right = 0;  // 滑动窗口左右指针
    int valid = 0;            // 表示窗口中满足要求的字符种类数
    int start = 0, len = INT_MAX;  // 记录最小覆盖子串的起始位置和长度

    while (right < s.size()) {
        char c = s[right];
        right++;  // 扩大窗口

        // 更新窗口内数据
        if (need.count(c)) {
            window[c]++;
            if (window[c] == need[c]) {
                valid++;  // 某个字符满足需求了
            }
        }

        // 判断窗口是否满足条件（即所有需要的字符都满足了）
        while (valid == need.size()) {
            // 更新最小覆盖子串的位置
            if (right - left < len) {
                start = left;
                len = right - left;
            }

            // 开始收缩窗口
            char d = s[left];
            left++;
            if (need.count(d)) {
                if (window[d] == need[d]) valid--;  // 缩小后失去一个满足条件的字符
                window[d]--;
            }
        }
    }

    // 返回结果
    return len == INT_MAX ? "" : s.substr(start, len);
}
```


### [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)
- 回溯算法
```cpp
class Solution {  
public:  
    vector<string> letterCombinations(string digits) {  
        vector<string> temp(10), res;  
        if (digits.empty())  
            return res;  
        string tem;  
        vector<int> dig;  
        for (auto c : digits)  
            dig.push_back(c - '0');  
        int n = 0;  
        temp[2] = "abc";     //构建数字到字母对应
        temp[3] = "def";  
        temp[4] = "ghi";  
        temp[5] = "jkl";  
        temp[6] = "mno";  
        temp[7] = "pqrs";  
        temp[8] = "tuv";  
        temp[9] = "wxyz";  
        backtrack(temp, dig, tem, n, res);  
        return res;  
    }  
  
    void backtrack(vector<string> &temp, vector<int> &dig, string &tem, int n, vector<string> &res) {  
        if (tem.size() == dig.size()) {   //几个数字对应几个字母
           res.push_back(tem);  
            return;  
        }  
        for (int i = 0; i < temp[dig[n]].size(); i++) {  
            tem.push_back(temp[dig[n]][i]);   
            backtrack(temp, dig, tem, n + 1, res);  
            tem.pop_back();  
        }  
    }  
};

// True
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        if (digits.empty()) return {};  // 处理空输入

        vector<string> temp(10);
        temp[2] = "abc";
        temp[3] = "def";
        temp[4] = "ghi";
        temp[5] = "jkl";
        temp[6] = "mno";
        temp[7] = "pqrs";
        temp[8] = "tuv";
        temp[9] = "wxyz";

        vector<string> res;
        string current;
        backtrack(temp, digits, current, 0, res);
        return res;
    }

    void backtrack(vector<string>& temp, string& digits, string& current, int n, vector<string>& res) {
        if (n == digits.size()) {
            res.push_back(current);
            return;
        }

        int num = digits[n] - '0';  // 将字符 '2' 转为数字 2
        for (char c : temp[num]) {
            current.push_back(c);
            backtrack(temp, digits, current, n + 1, res);
            current.pop_back();
        }
    }
};
```
>[!note]
>字符到数字变化-> char - '0'