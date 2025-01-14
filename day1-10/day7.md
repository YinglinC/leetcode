# 第三章 哈希表part02

● 454.四数相加II
● 383. 赎金信
● 15. 三数之和
● 18. 四数之和

## 454.四数相加II

第一想法，有点费劲，如何使用hash

```cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        int count = 0;
        std::unordered_map<int,int> abmap;
        for(int a: nums1){
            for(int b: nums2){
                if(abmap.find(a+b) == abmap.end()){
                    abmap[a+b] = 1;
                }else{
                    abmap[a+b] += 1;
                }
            }
        }
        for(int c: nums3){
            for(int d: nums4){
                if(abmap.find(0-(c+d)) != abmap.end()){
                    count += abmap[(0-(c+d))];
                }
            }
        }
        return count;
    }
};
```

没有什么思路，看了解释能写出来


## 383. 赎金信

第一想法，记录ransom每个字符出现的次数，再遍历magizine


```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        std::unordered_map<char, int> count_map;
        for(char s: ransomNote){
            count_map[s]++;
        }
        for(char s: magazine){
            if(count_map.find(s) != count_map.end()){
                count_map[s] -= 1;
            }
        }
        for(auto it = count_map.begin(); it != count_map.end(); it++){
            if(it->second >= 1)return false;
        }
        return true;
    }
};

```

`if(it->second >= 1)return false;`最有的判断条件需要思考一下

## 15. 三数之和

第一想法，排序
python3有好几种优化的方法，排序后判断

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        int n = nums.size();
        sort(nums.begin(), nums.end());

        for(int i = 0; i < n - 2; i++){
            if(i > 0 && nums[i] == nums[i-1]){
                continue;
            }
            int j = i + 1;
            int k = n - 1;
            // 优化1
            if(nums[i] + nums[i+1] + nums[i+2] > 0){
                break;
            }
            // 优化2
            if(nums[i] + nums[k-1] + nums[k] < 0){
                continue;
            }
            
            
            while(j < k){
                int sum = nums[i] + nums[j] + nums[k];
                if(sum == 0){
                    result.push_back({nums[i], nums[j], nums[k]});
                    j += 1;
                    while((j < k) && nums[j] == nums[j - 1]){
                        j += 1;
                    }

                    k -= 1;
                    while((j < k) && nums[k] == nums[k + 1]){
                        k -= 1;
                    }
                }else if(sum < 0){
                    j += 1;
                }else{
                    k -= 1;
                }
            }
        }
        return result;
    }
};
```

思路不难，能够写出来，但是代码实现水平还需要练习

## 18. 四数之和

第一想法，排序，a+b，c，d和三数之和一样求解

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        int n = nums.size();
        for (int i = 0; i < n - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            for (int b = i + 1; b < n - 2; b++) {
                if (b > i + 1 && nums[b] == nums[b - 1]) {
                    continue;
                }
                int j = b + 1;
                int k = n - 1;

                while (j < k) {
                    long sum = (long)nums[i] + nums[b] + nums[j] + nums[k];
                    if (sum == target) {
                        result.push_back(
                            {nums[i], nums[b], nums[j], nums[k]});
                        j += 1;
                        while ((j < k) && nums[j] == nums[j - 1]) {
                            j += 1;
                        }

                        k -= 1;
                        while ((j < k) && nums[k] == nums[k + 1]) {
                            k -= 1;
                        }
                    } else if (sum < target) {
                        j += 1;
                    } else {
                        k -= 1;
                    }
                }
            }
        }
        return result;
    }
}; 
```

问题，还有a中2个优化，b中2个优化没写，还需要注意数据范围，使用long，还需要强制数据类型转换
