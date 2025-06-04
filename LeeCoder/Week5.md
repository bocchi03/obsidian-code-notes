### [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)
- 动态规划
```cpp
class Solution {  
public:  
    int climbStairs(int n) {  
        if(n == 1 || n == 2)  // 边界条件
            return n;  
        vector<int> dp(n + 1);  
        dp[1] = 1;     // 初始化dp表
        dp[2] = 2;  
        for(int i = 3; i <= n; i++)  
            dp[i] = dp[i - 1] + dp[i  - 2];    // 状态转移方程
        return dp[n];  
    }  
};
```
>[!note]
>dp主要写出状态转移方程

### [746. 使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/)
- 动态规划
```cpp
class Solution {  
public:  
    int climbStairs(int n) {  
        if(n == 1 || n == 2)    
            return n;  
        vector<int> dp(n + 1);  
        dp[1] = 1;  
        dp[2] = 2;  
        for(int i = 3; i <= n; i++)  
            dp[i] = dp[i - 1] + dp[i  - 2];    // 状态转移方程
        return dp[n];  
    }  
};
```

### [198. 打家劫舍](https://leetcode.cn/problems/house-robber/)
- 动态规划
```cpp
// T
class Solution {  
public:  
    int rob(vector<int>& nums) {  
        int n = nums.size();  
        if(n == 1)  
            return nums[0];  
        if(n == 2)  
            return max(nums[0], nums[1]);  
        vector<int> dp(n + 1);  
        dp[0] = nums[0];  
        dp[1] = max(nums[0], nums[1]);  
        for(int i = 2; i < n; i++)     //上一次不偷或上上次偷
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[i]);  
        return dp[n - 1];  
    }  
};

// Inferior
class Solution {  
public:  
    int rob(vector<int>& nums) {  
        int n = nums.size() - 1;  
        if(n == 0)  
            return nums[0];  
        if(n == 1)  
            return max(nums[0], nums[1]);  
        if(n == 2)  
            return max(nums[0] + nums[2], nums[1]);  
        vector<int> dp(n + 1);  
        dp[0] = nums[0];  
        dp[1] = nums[1];  
        dp[2] = dp[0] + nums[2];  
        for(int i = 3; i <= n; i++)  
            dp[i] = max(dp[i - 2], dp[i - 3]) + nums[i];  
        return max(dp[n], dp[n - 1]);  
    }  
};
```
 
### [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)
- 动态规划
```cpp
// False
class Solution {  
public:  
    int maxSubArray(vector<int>& nums) {  
        int n = nums.size();  
        if(n == 1)  
            return nums[0];  
        vector<int> dp(n);  
        dp[0] = nums[0] < 0 ? 0 : nums[0];  
        int Miu = 0;  
        for(int i = 1; i < n; i++){  
            if(nums[i] < 0){    
                Miu += nums[i];  
                dp[i] = dp[i - 1];  
            }else{  
                dp[i] = max(dp[i - 1] + nums[i] + Miu, dp[i - 1]);  
                dp[i] = max(dp[i], nums[i]);  // 加上前数组或重新开始
                Miu = 0;  
            }  
        }  
        return dp[n - 1];    // 无法处理小于0情况 
    }  
};
//True
class Solution {  
public:  
    int maxSubArray(vector<int>& nums) {  
        int n = nums.size();  
        if(n == 1)  
            return nums[0];  
        vector<int> dp(n);    // 以当前元素为结尾的最大子数组和
        dp[0] = nums[0];  
        int Ma = nums[0];  
        for(int i = 1; i < n; i++){  
            dp[i] = max(dp[i - 1] + nums[i], nums[i]);  
            Ma = max(dp[i], Ma);  
        }  
        return Ma;  
    }  
};
```