# 第五章 栈与队列part01

14点09分
理论基础

- 232.用栈实现队列
- 225.用队列实现栈
- 20.有效的括号
- 1047.删除字符串中的所有相邻重复项

## 232.用栈实现队列

```cpp
class MyQueue {
public:
    stack<int> st1;
    stack<int> st2;
    int size = 0;
    MyQueue() {}

    void push(int x) { st1.push(x); }

    int pop() {
        if (st2.empty()) {
            while (!st1.empty()) {
                st2.push(st1.top());
                st1.pop();

            }
        }
        int res = st2.top();
        st2.pop();
        return res;
    }

    int peek() {
        int res = this->pop();
        st2.push(res);
        return res;
    }

    bool empty() {
        if (st1.empty() && st2.empty())
            return true;
        return false;
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */

```

问题，难度不大，c++的stack的pop方法不返回pop的值

## 225.用队列实现栈

```cpp
class MyStack {
public:
    queue<int> q1;
    queue<int> q2;
    MyStack() {
        
    }
    
    void push(int x) {
        q1.push(x);
    }
    
    int pop() {
        int size = q1.size();
        size--;
        while(size--){
            q2.push(q1.front());
            q1.pop();
        }

        int res = q1.front();
        q1.pop();
        q1 = q2;

        while(!q2.empty()){
            q2.pop();
        }

        return res;
    }
    
    int top() {
        int res = q1.back();
        return res;
    }
    
    bool empty() {
        return q1.empty();
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */

```

通过size来控制队列中的最后的一位元素，

一个队列来循环
```cpp
class MyStack {
public:
    queue<int> q;
    MyStack() {
        
    }
    
    void push(int x) {
        q.push(x);
        
    }
    
    int pop() {
        int size = q.size();
        size--;
        while(size--){
            int tmp = q.front();
            q.push(tmp);
            q.pop();
        }
        int res = q.front();
        q.pop();
        return res;
    }
    
    int top() {
        return q.back();
    }
    
    bool empty() {
        return q.empty();
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */

```

## 20.有效的括号

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        for (char i : s) {
            if (!st.empty()) {
                char top = st.top();
                // st.push(i);
                if ((top == '(' && i == ')') ||
                    (top == '[' && i == ']') ||
                    (top == '{' && i == '}')) {
                    // st.pop();
                    st.pop();
                    continue;
                }
            }
            st.push(i);
        }
        if (st.empty()) {
            return true;
        }
        return false;
    }
};
```

问题：思路不难，需要代码更细致一点

## 1047.删除字符串中的所有相邻重复项

```cpp
class Solution {
public:
    string removeDuplicates(string s) {
        stack<char> st;
        for(char i: s){
            if(!st.empty()){
                char top = st.top();
                if(i == top){
                    st.pop();
                    continue;
                }
            }
            st.push(i);
        }

        string res;
        while(!st.empty()){
            res += st.top();
            st.pop();
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

15点08分
