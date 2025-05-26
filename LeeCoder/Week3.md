### [682. 棒球比赛](https://leetcode.cn/problems/baseball-game/)
- 栈
```cpp
// False
class Solution {  
public:  
    int calPoints(vector<string>& operations) {  
        stack<int> stk;  
        int res = 0;  
        for(auto c : operations){  
            if(c == "C"){  
                stk.pop();  
            }else if(c == "+"){  
                int n1 = stk.top();  
                stk.pop();  
                int n2  = stk.top();  
                stk.pop();  
                int sum = n2 + n1;  
                stk.push(2 * sum);   // 破坏了栈的结构  
            }else if(c == "D"){     //  + + 会导致错误结构
                int n = stk.top();  
                stk.push(2 * n);  
            }else{  
                int n = stoi(c);  
                stk.push(n);  
            }  
        }  
        while(!stk.empty()){  
            res += stk.top();  
            stk.pop();  
        }  
        return res;  
    }  
};

// True
class Solution {  
public:  
    int calPoints(vector<string>& operations) {  
        stack<int> stk;  
        int res = 0;  
        for(auto c : operations){  
            if(c == "C"){  
                stk.pop();  
            }else if(c == "+"){  
                int n1 = stk.top();  
                stk.pop();  
                int n2  = stk.top();  
                stk.pop();  
                int sum = n2 + n1;  
                stk.push(n2);    // 保存和恢复
                stk.push(n1);  
                stk.push(sum);  
            }else if(c == "D"){  
                int n = stk.top();  
                stk.push(2 * n);  
            }else{  
                int n = stoi(c);  
                stk.push(n);  
            }  
        }  
        while(!stk.empty()){  
            res += stk.top();  
            stk.pop();  
        }  
        return res;  
    }  
};

```
>[!note]
>当需要撤销操作时想到用栈

### [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)
```cpp
class MyQueue {  
public:  
    MyQueue() {  
    }  
  
    void push(int x) {  
        input.push(x);  
    }  
    int pop() {  
        int n = peek();  
        output.pop();  
        return n;  
    }  
  
    int peek() {  
        if(output.empty()){  
            while(!input.empty()){    // 用output接收倒过来的栈
                int n = input.top();  
                input.pop();  
                output.push(n);  
            }  
        }  
        return output.top();  
    }  
  
    bool empty() {  
        return input.empty() && output.empty();  
    }  
  
private:  
    stack<int> input, output;  
};
```

### [496. 下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/)
- 单调栈
```cpp
vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {  
    stack<int> stk;  
    unordered_map<int, int> next_greater;  // 存每个元素的下一个更大元素  
          // Step 1: 用单调栈处理nums2  
    for (int num : nums2) {  
        while (!stk.empty() && stk.top() < num) {  
            next_greater[stk.top()] = num;  // num是栈顶元素的下一个更大值  
            stk.pop();  
        }  
        stk.push(num);   
    }  
    // 剩余元素没有下一个更大值，默认为-1（已在map初始化时处理）  
  
    // Step 2: 直接查询结果  
    vector<int> res;  
    for (int num : nums1) {  
        res.push_back(next_greater.count(num) ? next_greater[num] : -1);  
    }  
    return res;  
}
```
>[!note]
>用nums2进行单调栈模拟，若当前元素>栈顶元素则有对应下一个更大元素

### [844. 比较含退格的字符串](https://leetcode.cn/problems/backspace-string-compare/)
```cpp
// False
class Solution {  
public:  
    bool backspaceCompare(string s, string t) {  
        stack<char> stk1, stk2;  
        for(char c : s){  
            if(c == '#')  
                if(!stk1.empty())   // if作用范围错误
                    stk1.pop();  
                else  
                    stk1.push(c);  
        }  
  
        for(char c : t){  
            if(c == '#')  
                if(!stk2.empty())    // 同
                    stk2.pop();  
                else  
                    stk2.push(c);  
        }  
        return stk2 == stk1;  
    }  
};
```


### [155. 最小栈](https://leetcode.cn/problems/min-stack/)
- 双栈
```cpp
class MinStack {  
public:  
    MinStack() {  
  
    }  
  
    void push(int val) {  
        if(Min.empty() || val <= Min.top())  //Min维持最小元素
            Min.push(val);  
        stk.push(val);  
    }  
  
    void pop() {  
        if(stk.top() == Min.top())  
            Min.pop();  
        if(!stk.empty())  
            stk.pop();  
    }  
  
    int top() {  
        return stk.top();  
    }  
  
    int getMin() {  
        return Min.top();  
    }  
  
private:  
    stack<int> stk;  
    stack<int> Min;  
};
```
>[!note]
>联想Stack   需要撤回(pop())最小值后还是最小值

### [394. 字符串解码](https://leetcode.cn/problems/decode-string/)
- 栈+括号匹配
```cpp
class Solution {  
public:  
    string decodeString(string s) {  
        stack<char> stk;  
        string res;  
        for(auto c : s){  
            if(c == ']'){  
                string tem;  
                while(stk.top() != '['){    //提取括号内的内容
                    tem.push_back(stk.top());  
                    stk.pop();  
                }  
                stk.pop();  
                std::reverse(tem.begin(), tem.end());  
                
                string nu;  
                while(!stk.empty() && isdigit(stk.top())){  //提取次数k
                    nu += stk.top();  
                    stk.pop();  
                }  
                std::reverse(nu.begin(), nu.end());  
                int n = stoi(nu);    
                
                string t = tem;  
                for(int i = n - 1; i >0; i--)    //重复内容
                    tem += t;  
                
                for(char q : tem)     //压回栈
                    stk.push(q);  
            }else  
                stk.push(c);  
        }  
        
        while(!stk.empty()){   //从栈中提取res
            res += stk.top();  
            stk.pop();  
        }  
        std::reverse(res.begin(), res.end());  
        return res;  
    }  
};
```
>[!note]
>提取数字可能不止一位数 ， stoi()-> string to int


>[!tip]
>要用到之前元素， 返回之前元素， 撤销操作， 符号匹配 -> stack