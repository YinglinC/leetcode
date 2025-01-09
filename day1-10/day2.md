# 数组专题（二）

[209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)
[59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)

## 209. 长度最小的子数组

### 题目描述

给定一个含有 n 个正整数的数组和一个正整数 target，找出该数组中满足其和 ≥ target 的长度最小的连续子数组，并返回其长度。如果不存在符合条件的子数组，返回 0。

### 解题思路

- **滑动窗口法**：
  - 使用双指针维护一个滑动窗口
  - 右指针不断扩展窗口，左指针在满足条件时收缩窗口
  - 记录满足条件的最小窗口长度

### 复杂度分析

- 时间复杂度：O(n)，每个元素最多被访问两次
- 空间复杂度：O(1)，只使用了常数个额外空间

### 代码实现

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int n = nums.size();
        int lenwindow = n + 1;
        int sum = 0;
        int i = 0;
        for(int j = 0; j < n; j++){
            sum += nums[j];
            while (sum >= target){
                lenwindow = min(lenwindow, j - i + 1);
                sum -= nums[i];
                i++;
            }
        }
        return lenwindow == n + 1? 0: lenwindow;
    }
};
```

### 注意事项

- 注意while循环中的条件判断是`>=`还是`>`
- 初始化窗口长度时设置为n+1，便于判断是否存在符合条件的子数组

## 59. 螺旋矩阵 II

初始化的逻辑不太好想，怎么确定有多少循环

- 理解螺旋矩阵的结构：对于一个 n x n 的矩阵，螺旋矩阵是从外向内逐层填充的。每一层可以看作是一个更小的矩阵的边界。
- 计算循环次数：对于一个 n x n 的矩阵，螺旋的层数是 n / 2。这是因为每一层都会减少两个边长（每层的上下左右边界各减少一个），所以当 n 是偶数时，层数正好是 n / 2。当 n 是奇数时，最后一层会是一个单独的中心点，不需要再进行循环填充，因此循环次数仍然是 n / 2。

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        std::vector<std::vector<int>> result(n, std::vector<int>(n, 0));
        // 左闭右开
        int startx = 0;
        int starty = 0;
        int loop = n / 2;
        int mid = n / 2;
        int count = 1;
        int offset = 1;
        int i,j;
        while(loop--){
            i = startx;
            j = starty;
            for(j; j < n - offset; j++){
                result[i][j] = count++;
            }
            for(i; i < n - offset; i++){
                result[i][j] = count++;
            }
            for(j; j > starty; j--){
                result[i][j] = count++;
            }
            for(i; i > startx; i--){
                result[i][j] = count++;
            }

            startx++;
            starty++;

            offset += 1;

            }
            if (n % 2){
                result[mid][mid] = count;
        }
        return result;

    }
};
```

填充逻辑

- 初始化变量：
  - startx 和 starty 表示当前层的起始位置，初始值为0。
  - loop 表示需要填充的层数，初始值为 n / 2。
  - mid 表示矩阵的中心位置，用于处理 n 是奇数的情况。
  - count 用于记录当前填充的数字，初始值为1。
  - offset 用于控制每一层的边界，初始值为1。
- 填充每一层：
  - 向右填充：从 starty 到 n - offset。
  - 向下填充：从 startx 到 n - offset。
  - 向左填充：从 n - offset 到 starty + 1。
  - 向上填充：从 n - offset 到 startx + 1。
- 更新变量：
  - 每完成一层的填充后，startx 和 starty 都增加1，因为下一层的起始位置向内移动。
  - offset 增加1，因为每一层的边界都向内缩小。

- 处理奇数情况
  - 中心点填充：如果 n 是奇数，矩阵的中心位置（mid, mid）需要单独填充。由于 loop 循环已经完成了所有外层的填充，此时 count 的值正好是需要填充到中心点的值。


## 前缀和

```cpp
n = nums.size();
std::vector<int> p(n, 0);
int presum = 0
// p[0]是第一个数的前缀和
for(int i = 0; i < n; i++){
    presum += nums[i];
    p[i + 1] = presum;
}

//[a, b]
if (a == 0){
    sum = p[b];
}else{
    sum = p[b] - p[a - 1];
}
```

**在使用前缀和的时候，分不清前缀和的区间，建议画一画图，模拟一下 思路会更清晰。**

### 开发商购买土地

[44. 开发商购买土地](https://www.programmercarl.com/kamacoder/0044.%E5%BC%80%E5%8F%91%E5%95%86%E8%B4%AD%E4%B9%B0%E5%9C%9F%E5%9C%B0.html#%E6%80%9D%E8%B7%AF)

只想到暴力，后续没有细想
