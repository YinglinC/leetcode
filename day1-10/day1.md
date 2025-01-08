# 数组理论基础

[数组理论基础文章](https://programmercarl.com/%E6%95%B0%E7%BB%84%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)

## 704. 二分查找

二分查找是一种高效的搜索算法，适用于有序数组。实现时需要注意区间的定义：

- **关键点**：确定搜索区间
- **两种常见实现方式**：
  - 左闭右闭 [left, right]
  - 左闭右开 [left, right)

左闭右闭
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0;
        int right = n - 1;
        while (left <= right){
            int mid = left + (right - left) / 2;
            if (nums[mid] > target){
                right = mid - 1;
            }else if(nums[mid] < target){
                left = mid + 1;
            }else{
                return mid;
            }
        }
        return -1;
    }
};
```

左闭右开
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0;
        int right = n;
        while (left < right){
            int mid = left + (right - left) / 2;
            if (nums[mid] > target){
                right = mid;
            }else if (nums[mid] < target){
                left = mid + 1;
            }else{
                return mid;
            }
        }
        return -1;
}};
```

## 27. 移除元素

### 暴力解法
- **时间复杂度**：O(n^2)
- **空间复杂度**：O(1)
- **实现思路**：
  - 使用两层循环遍历数组
  - 当遇到目标值时，将后续元素整体前移

### 双指针解法（推荐）
- **时间复杂度**：O(n)
- **空间复杂度**：O(1)
- **实现思路**：
  - 使用快慢指针，通过一次遍历完成操作
  - **指针定义**：
    - 快指针：扫描数组，寻找非目标值元素
    - 慢指针：维护新数组的当前位置

暴力
```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int n = nums.size(); // 获取数组长度
        for (int i = 0; i < n; ) { // 遍历数组
            if (nums[i] == val) { // 如果当前元素等于 val
                // 将后续元素向前移动
                for (int j = i + 1; j < n; j++) {
                    nums[j - 1] = nums[j];
                }
                n--; // 数组大小减一
                // 此时不需要 i++，因为需要重新检查当前位置
            } else {
                i++; // 只有不等于 val 时才向后移动
            }
        }
        return n; // 返回新的数组大小
    }
};
```

双指针
```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int n = nums.size();
        int slowpoint = 0;
        int fastpoint = 0;
        for (fastpoint; fastpoint < n; fastpoint++){
            if (nums[fastpoint] != val){
                nums[slowpoint] = nums[fastpoint];
                slowpoint++;
            }
        }
        return slowpoint;
    }
};

```

## 977. 有序数组的平方

**解题思路**：
- 利用数组已排序的特性
- 使用双指针法，分别指向数组的首尾
- 比较指针所指元素的平方值
- 将较大的平方值放入结果数组的末尾
- 时间复杂度：O(n)
- 空间复杂度：O(n)

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int left = 0;  // 左指针
        int right = nums.size() - 1;  // 右指针
        int n = nums.size();
        vector<int> result(n, 0);  // 用于存储结果数组
        int pos = n - 1;  // 从结果数组的末尾开始填充

        // 双指针从两端向中间移动
        while (left <= right) {
            int leftSquare = nums[left] * nums[left];  // 左侧平方
            int rightSquare = nums[right] * nums[right];  // 右侧平方

            // 较大的平方数放到结果数组的当前位置
            if (leftSquare > rightSquare) {
                result[pos] = leftSquare;
                left++;  // 移动左指针
            } else {
                result[pos] = rightSquare;
                right--;  // 移动右指针
            }
            pos--;  // 填充位置向前移动
        }

        return result;  // 返回排序后的平方数组
    }
};
```
