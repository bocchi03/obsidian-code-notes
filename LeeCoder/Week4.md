### [367. 有效的完全平方数](https://leetcode.cn/problems/valid-perfect-square/)
- 二分查找
```cpp
class Solution {  
public:  
    bool isPerfectSquare(int num) {  
        long left = 1, right = num;  
        while(left <= right){  
            long mid = left + (right - left) / 2;  
            long sqt = mid * mid;  
            if(sqt < num)  
                left = mid + 1;  
            else if(sqt > num)  
                right = mid - 1;  
            else{  
                return true;  
            }  
        }  
        return false;  
    }  
};

// Other
class Solution {  
public:  
    bool isPerfectSquare(int num) {  
        long left = 1, right = num /  left;  
        while(left < right){  
            left++;  
            right = num / left;  
        }  
        return left * left == num;  
    }  
};
```
>[!note]
>二分查找变形
>- 查找第一个出现位置(命中后继续左移right)
>- 查找最后一个出现位置(命中后继续右移left)
>- 查找第一个大于target值

### [441. 排列硬币](https://leetcode.cn/problems/arranging-coins/)
- 二分查找
```cpp
class Solution {  
public:  
    int arrangeCoins(int n) {  
        long left = 1, right = n;  
        long res = 0;  
  
        while(left <= right){  
            long mid = left + (right - left) / 2;  
            long coin = mid * (mid + 1) / 2;    // 二分查找前mid个是否符合
  
            if(coin == n)  
                return mid;  
            if(coin < n){  
                res = mid;     // 前mid个太少
                left = mid + 1;  
            }else  
                right = mid - 1;  
        }  
        return res;  
    }  
};
```
>[!tip]
>查找顺序排序后某数使用二分查找



### [118. 杨辉三角](https://leetcode.cn/problems/pascals-triangle/)
- 数组
```cpp
// True
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> res;
        if (numRows == 0) return res;
        
        res.push_back({1}); // 第一行
        
        for (int i = 1; i < numRows; ++i) {
            vector<int> newRow(i + 1, 1); // 创建新行并初始化为1
            
            for (int j = 1; j < i; ++j) { // 跳过首尾的1
                newRow[j] = res[i-1][j-1] + res[i-1][j];
            }
            
            res.push_back(newRow);
        }
        
        return res;
    }
};

// False
class Solution {  
public:  
    vector<vector<int>> generate(int numRows) {  
        vector<vector<int>> res;  
        res.push_back({1});   // 创建第一行
        for(int i = 1; i < numRows; i++){  
            res[i].push_back(1);     // 当前只有一行->Error
            for(int j = 1; j <= i; j++){  
                if(j == i)  
                    res[i].push_back(1);  
                else{  
                    int tem = res[i - 1][j - 1] + res[i - 1][j];  
                    res[i].push_back(tem);  
                }  
            }  
        }  
        return res;  
    }  
};
```
>[!warning]
>使用数组时注意大小-> 是否使用空行？