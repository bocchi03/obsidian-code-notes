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