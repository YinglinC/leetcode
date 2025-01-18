# 第五章 栈与队列part02

- 150.逆波兰表达式求值
- 239.滑动窗口最大值
- 347.前 K 个高频元素

## 150.逆波兰表达式求值

21点20分

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<long> st;
        for (string i : tokens) {
            if (i != "+" && i != "-" && i != "*" &&i != "/") {
                st.push(stol(i));
            } else {
                long y = st.top();
                st.pop();
                long x = st.top();
                st.pop();
                // i转成运算符
                long res = 0;
                if(i == "+")  res = x + y;
                if(i == "-")  res = x - y;
                if(i == "*")  res = x * y;
                if(i == "/")  res = x / y;
                st.push(res);
            }
        }
        long result = st.top();
        return result;
    }
};
```

## 239.滑动窗口最大值

```cpp
class Solution {
private:
    class MyQueue{
    public:
        deque<int> dque;
        void pop(int value){
            if(!dque.empty() && value == dque.front()){
                dque.pop_front();
            }
        }
        void push(int value){
            while(!dque.empty() && value > dque.back()){
                dque.pop_back();
            }
            dque.push_back(value);
        }
        int front(){
            return dque.front();
        }
    };
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        MyQueue result_q;
        vector<int> result;
        for(int i = 0; i <k;i++){
            result_q.push(nums[i]);
        }
        result.push_back(result_q.front());

        for(int i = k; i < nums.size();i++){
            result_q.push(nums[i]);
            result_q.pop(nums[i-k]);
            result.push_back(result_q.front());
        }
        return result;
    }
};
```

问题，使用单调队列，暴力的方法很容易想，但没有去实现

## 347.前 K 个高频元素

第一想法，暴力字典，再排序

栈和队列的做法没思路

cv
