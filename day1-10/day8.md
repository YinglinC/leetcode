# 第四章 字符串part01

● 344.反转字符串
● 541. 反转字符串II
● 卡码网：54.替换数字

## 344.反转字符串

第一想法，首位交换，奇数，中间不动

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        int n = s.size();
        int j = n - 1;
        for(int i = 0; i < n/2; i++){
            char tmp;
            tmp = s[i];
            s[i] = s[j];
            s[j] = tmp;
            j--;
        }
    }
};
```

## 541. 反转字符串II

第一想法，直接模拟，

问题，模拟的时候，处理的区间要思考，c++的库函数reverse处理方式

```cpp
class Solution {
public:
    string reverseStr(string s, int k) {
        int n = s.size();
        for(int i = 0; i < n - 1;){
            if(i + k <= n){
                reverse(s.begin()+i, s.begin()+i+k);
            }else{
                reverse(s.begin()+i, s.end());
            }
            i += 2 * k;
        }
        return s;
    }
};
```
