# 第三章 哈希表part01

242.有效的字母异位词,349. 两个数组的交集,202. 快乐数,1. 两数之和

## 242.有效的字母异位词

第一想法，s记录到map中，t遍历map看是否都存在，存在就--，最后看map中是否都为0

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        int res[26] = {0};
        for(int i = 0; i < s.size(); i++){
            res[s[i] - 'a']++;
        }
        for(int i = 0; i < t.size(); i++){
            res[t[i] - 'a']--;
        }
        for(int i = 0; i < 26; i++){
            if (res[i] != 0)return false;
        }
        return true;
        
    }
};
```

问题，cpp数据类型的初始化方法，还是不熟悉

## 349. 两个数组的交集

第一想法，放入unorderset中

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        std::unordered_set<int> result;
        std::unordered_set<int> set1(nums1.begin(), nums1.end());
        for(int i = 0; i < nums2.size(); i++){
            if(set1.find(nums2[i]) != set1.end()){
                result.insert(nums2[i]);
            }
        }
        return std::vector<int> (result.begin(), result.end());
        
    }
};

```

问题，熟悉std的用法，unordered_set的用法，vector的用法，怎么初始化，增删改查

## 202. 快乐数

第一想法，怎么求出每位数上的平方和，直接模拟暴力，如果碰到不是1，但是之前出现过的数字，则返回false

```cpp
class Solution {
public:
    int get_sum(int n){
        int sum = 0;
        while(n){
            sum += (n % 10) * (n % 10);
            n /= 10;
        }
        return sum;
    }
    bool isHappy(int n) {
        int sum = 0;
        std::unordered_set<int> res;
        while(1){
            sum = get_sum(n);
            if(res.find(sum) != res.end())return false;
            if(sum == 1)return true;
            res.insert(sum);
            n = sum;
            
        }
    }
};
```

set的判断是否存在的方法'if(res.find(sum) != res.end())'

## 1. 两数之和

第一想法，hashmap，遍历数组，看target - nums[i]是否在map中，如果在，则返回，不在，则将nums[i]放入map中

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        std::map<int, int> res;
        for(int i = 0; i < nums.size(); i++){
            auto iter = res.find(target - nums[i]);
            if(iter != res.end()){
                return {iter->second, i};
            }
            res.insert(pair<int,int>(nums[i], i));
        }
        return {};
    }
};
```

问题，map的用法，pair的用法，vector的用法
返回{}自动是vector类型

减少 pair 的使用：
可以直接使用 `map[key] = value` 的方式插入元素，而不需要显式创建 pair。
