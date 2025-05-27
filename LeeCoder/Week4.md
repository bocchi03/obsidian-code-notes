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