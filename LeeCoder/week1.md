## [1. 两数之和](https://leetcode.cn/problems/two-sum/)
- 排序+哈希表+结构体（键值对）
```c++
vector<int> res;  
vector<pair<int, int>> dire;  
// 结构体(原数+下标)
for(int i = 0; i < nums.size(); i++)  
    dire.push_back({nums[i], i});  
  
sort(dire.begin(), dire.end());  
int left = 0, right = nums.size() - 1;  
while(left != right){  
    int tem = dire[left].first + dire[right].first;  
    if(tem < target)  
        left++;  
    else if(tem > target)  
        right--;  
    else{  
        res.push_back(dire[left].second);  
        res.push_back(dire[right].second);  
        return res;  
    }
}
```
>[!warning]
>排序后直接使用哈希表，重复元素无法分开，使用同一个index
>使用键值对结构体 


#### #[88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)
- 双指针+原地合并
```cpp
int n1 = m - 1, n2 = n - 1;  
    int end = m + n - 1;  
    while(n1 >= 0 && n2 >= 0){  
        if(nums1[n1] >= nums2[n2])  
            nums1[end--] = nums1[n1--];  
        else{  
            nums1[end--] = nums2[n2--];  
        }  
    }  
    while(n2 >= 0)  
        nums1[end--] = nums2[n2--];  
}
```
>[!warning] 
>没有联想从前往后合并

#### #[26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)
- 双指针+原地删除
```cpp
int slow = 0, fast = 0;  
while(fast != nums.size()){  
    if(nums[fast] == nums[slow])  
        fast++;  
    else{  
       nums[++slow] = nums[fast];  
    };  
}  
return slow + 1;
```

>[!note] 
>原地合并/删除——额外添加一个指针（前后）
#### #[136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)
- 位运算
```cpp
int res = 0;  
for(int i : nums)  
    res ^= i;  
return res;
```
 - 异或运算满足交换律和结合律  a⊕0 = a
 - 一个数和 0 做 XOR 运算等于本身：a⊕0 = a
 - 一个数和其本身做 XOR 运算等于 0：a⊕a = 0


#### #[242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)
- 哈希表（数组模拟）
```cpp
int count[26] = {0}; 

for (char c : s) count[c - 'a']++;
for (char c : t) count[c - 'a']--;

for (int num : count) {
    if (num != 0) return false; 
    }
return true;
```
>[!tip]
>利用出现次数相加减是否为0，判断次数是否一致


#### #[560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)
```cpp
int n = nums.size();  
int cnt = 0;  
vector<int> temp(n + 1);  
unordered_map<int, int> dire;  
// 构造前缀和数组
for(int i = 0; i < nums.size(); i++)  
    temp[i + 1] = nums[i] + temp[i];  
  
dire[0]++;  // 第一项和为0
// 数组基准从[1]开始
for(int i = 1; i < temp.size(); i++){  
    int diff = temp[i] - k;  
    if(dire.count(diff)){  
        cnt += dire[diff]; // 加上所有diff出现次数  
    }  
    dire[temp[i]]++;  
}  
return cnt;
```
>[!note]
>- 前缀和+哈希表
>- tag: 和为k的子数组

#### #[202. 快乐数](https://leetcode.cn/problems/happy-number/)
```cpp
class Solution {  
public:  
    bool isHappy(int n) {  
        int slow = n;  
        int fast = getNext(n);  
        // 抽象成链表 ，用快慢指针判断是否循环
        while(fast != 1 && slow != fast){  
            slow = getNext(slow);  
            fast = getNext(getNext(fast));  
        }  
        return fast == 1;  
    }  
  
    // 求平方和
    int getNext(int n){  
        int sum = 0;  
        while(n > 0){  
            int d = n % 10;  
            n = n / 10;  
            sum += d * d;  
        }  
        return sum;  
    }  


	// 哈希表判断是否循环
	bool isHappy(int n) {  
    unordered_map<int, int> seen;  
    while(n != 1 && !seen.count(n)){  
        seen[n]++;  
        n = getNext(n);  
    }  
    return n == 1;  
}
};
```

>[!tip]
>- key:关键为判断是否循环


- Reasult
```cpp
// 滑动窗口
// 变长窗口
int left = 0, right = 0;
while(right < nums.size()){
	//增加窗口
	sum += nums[right];
	// 满足条件
	while(sum > target){
		// 减小窗口
		// process
		sum -= nums[left];
		left++;
	}
}
```

