# 41. [数据流中的中位数](https://leetcode.cn/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

## 题目描述【困难】

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

- void addNum(int num) - 从数据流中添加一个整数到数据结构中。
- double findMedian() - 返回目前所有元素的中位数。

示例 1：

```
输入：
["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
[[],[1],[2],[],[3],[]]
输出：[null,null,null,1.50000,null,2.00000]
```

示例 2：

```
输入：
["MedianFinder","addNum","findMedian","addNum","findMedian"]
[[],[2],[],[3],[]]
输出：[null,null,2.00000,null,2.50000]
```

## 解题思路

建立一个 小顶堆 A 和 大顶堆 B ，各保存列表的一半元素，*A* 保存 **较大** 的一半，*B* 保存 **较小** 的一半。如果是奇数个，则 A 多保存一个。

随后，中位数可仅根据 A,B 的堆顶元素计算得到。

![image-20230428231114807](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304282311941.png)

addNum(num) 函数：

1. 当 m=n（即 N 为 偶数）：需向 A 添加一个元素。实现方法：将新元素 num 插入至 B ，再将 B 堆顶元素插入至 A ；
2. 当 m ≠ n（即 N 为 奇数）：需向 B 添加一个元素。实现方法：将新元素 num 插入至 A ，再将 A 堆顶元素插入至 B ；

findMedian() 函数：

1. 当 m=n（ N 为 偶数）：则中位数为 ( A 的堆顶元素 + B 的堆顶元素 )/2。
2. 当 m ≠ n（ N 为 奇数）：则中位数为 A 的堆顶元素。


![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304282321493.gif)

```java
class MedianFinder {
    Queue<Integer> A, B;
    public MedianFinder() {
        A = new PriorityQueue<>(); // 小顶堆，保存较大的一半
        B = new PriorityQueue<>((x, y) -> (y - x)); // 大顶堆，保存较小的一半
    }
    public void addNum(int num) {
        if(A.size() != B.size()) {
            A.add(num);
            B.add(A.poll());
        } else {
            B.add(num);
            A.add(B.poll());
        }
    }
    public double findMedian() {
        return A.size() != B.size() ? A.peek() : (A.peek() + B.peek()) / 2.0;
    }
}
```

# 42. [连续子数组的最大和](https://leetcode.cn/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

## 题目描述【简单】

输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

```
输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

## 解题思路

动态规划解析：

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230430181927593.png)

- 状态定义： 设动态规划列表 dp ，dp[i] 代表以元素 nums[i] 为结尾的连续子数组最大和。
- 转移方程： 若 dp[i−1]≤0 ，说明 dp[i−1] 对 dp[i] 产生负贡献，即 dp[i−1]+nums[i] 还不如 nums[i] 本身大。
  - 当 dp[i−1]>0 时：执行 dp[i]=dp[i−1]+nums[i] ；
  - 当 dp[i−1]≤0 时：执行 dp[i]=nums[i] ；
- 初始状态： dp[0]=nums[0]，即以 nums[0] 结尾的连续子数组最大和为 nums[0] 。
- 返回值： 返回 dp 列表中的最大值，代表全局最大值。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        int res = dp[0];
        for (int i = 1; i < nums.length; i++) {
            dp[i] = Math.max(dp[i - 1] + nums[i], nums[i]);
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}
```

# 【x】43. [1～n 整数中 1 出现的次数](https://leetcode.cn/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/)

## 题目描述【困难】

输入一个整数 n ，求1～n这n个整数的十进制表示中1出现的次数。

例如，输入12，1～12这些整数中包含1 的数字有1、10、11和12，1一共出现了5次。

**示例 1：**

```
输入：n = 12
输出：5
```

**示例 2：**

```
输入：n = 13
输出：6
```

## 解题思路

```java

```

# 44. [数字序列中某一位的数字](https://leetcode.cn/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/)

## 题目描述【中等】

数字以0123456789101112131415…的格式序列化到一个字符序列中。在这个序列中，第5位（从下标0开始计数）是5，第13位是1，第19位是4，等等。

请写一个函数，求任意第n位对应的数字。

示例 1：

```
输入：n = 3
输出：3
```


示例 2：

```
输入：n = 11
输出：0
```

## 解题思路

1. 将 101112⋯ 中的每一位称为 数位 ，记为 n ；
2. 将 10,11,12,⋯ 称为 数字 ，记为 num ；
3. 数字 10 是一个两位数，称此数字的 位数 为 2 ，记为 digit ；
4. 每 digit 位数的起始数字（即：1,10,100,⋯），记为 start 。

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230501112811957.png)

根据以上分析，可将求解分为三步：

1. 确定 n 所在 数字 的 位数 ，记为 digit ；
2. 确定 n 所在的 数字 ，记为 num ；
3. 确定 n 是 num 中的哪一数位，并返回结果。

第一步：确定 n 所在 数字 的 位数

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230501121849334.png)

循环执行 n 减去 一位数、两位数、... 的数位数量 count ，直至 n≤count 时跳出。

```python
digit, start, count = 1, 1, 9
while n > count:
    n -= count
    start *= 10 # 1, 10, 100, ...
    digit += 1  # 1,  2,  3, ...
    count = 9 * start * digit # 9, 180, 2700, ...
```

第二步：确定 n 所在的 数字

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230501121903988.png)

所求数位 在从数字 start 开始的第 [(n−1)/digit] 个 数字 中（start 为第 0 个数字）。

```python
num = start + (n - 1) / digit
```

第三步：确定 n 是 num 中的哪一数位

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230501122610716.png)

所求数位为数字 num 的第 (n−1)%digit 位（数字的首个数位为第 0 位）。

```python
s = str(num) # 转化为 string
res = int(s[(n - 1) % digit]) # 获得 num 的 第 (n - 1) % digit 个数位，并转化为 int
```

算法示例：

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230501123641929.png)

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230501123656491.png)

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230501123718602.png)

```java
class Solution {
    public int findNthDigit(int n) {
        int digit = 1;
        long start = 1;
        long count = 9;
        while (n > count) { // 1.
            n -= count;
            digit += 1;
            start *= 10;
            count = digit * start * 9;
        }
        long num = start + (n - 1) / digit; // 2.
        return Long.toString(num).charAt((n - 1) % digit) - '0'; // 3.
    }
}
```

# 45. [把数组排成最小的数](https://leetcode.cn/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

## 题目描述【中等】

输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

示例 1:

```
输入: [10,2]
输出: "102"
```


示例 2:

```
输入: [3,30,34,5,9]
输出: "3033459"
```

## 解题思路

设数组 nums 中任意两数字的字符串为 x 和 y ，则规定 排序判断规则 为：

- 若拼接字符串 x+y>y+x ，则 x “大于” y ；
- 反之，若 x+y<y+x ，则 x “小于” y ；

根据以上规则，套用任何排序方法对 nums 执行排序即可。

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230501192356131.png)


```java
class Solution {
    public String minNumber(int[] nums) {
        String[] strs = new String[nums.length];
        for(int i = 0; i < nums.length; i++)
            strs[i] = String.valueOf(nums[i]);
        quickSort(strs, 0, strs.length - 1);
        StringBuilder res = new StringBuilder();
        for(String s : strs)
            res.append(s);
        return res.toString();
    }
    void quickSort(String[] strs, int l, int r) {
        if(l >= r) return;
        int i = l, j = r;
        String tmp = strs[i];
        while(i < j) {
            while((strs[j] + strs[l]).compareTo(strs[l] + strs[j]) >= 0 && i < j)
                j--;
            while((strs[i] + strs[l]).compareTo(strs[l] + strs[i]) <= 0 && i < j)
                i++;
            tmp = strs[i];
            strs[i] = strs[j];
            strs[j] = tmp;
        }
        strs[i] = strs[l];
        strs[l] = tmp;
        quickSort(strs, l, i - 1);
        quickSort(strs, i + 1, r);
    }
}
```

快速排序的思想查看这篇文章：https://blog.csdn.net/weixin_42437295/article/details/90771962

# 46. [把数字翻译成字符串](https://leetcode.cn/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

## 题目描述【中等】

给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

示例 1:

```
输入: 12258
输出: 5
解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"
```

## 解题思路

此题可用动态规划解决，以下按照流程解题。

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230501202346305.png)

- 状态定义： 设动态规划列表 dp ，dp[i] 代表以 x<sub>i</sub> 为结尾的数字的翻译方案数量。
- 转移方程： 若 x<sub>i</sub> 和 x<sub>i-1</sub> 组成的两位数字可以被翻译，则 dp[i]=dp[i−1]+dp[i−2] ；否则 dp[i]=dp[i−1] 。
  - 可被翻译的两位数区间：当 x<sub>i−1</sub>=0 时，组成的两位数是无法被翻译的（例如 00, 01, 02, ⋯），因此区间为 [10,25] 。

![image-20230501202837892](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230501202837892.png)

- 初始状态： dp[0]=dp[1]=1 ，即 “无数字” 和 “第 1 位数字” 的翻译方法数量均为 1 ；
- 返回值： dp[n] ，即此数字的翻译方案数量。

> Q： 无数字情况 dp[0]=1 从何而来？
> A： 当 num 第 1,2 位的组成的数字 ∈ [10, 25] 时，显然应有 2 种翻译方法，即 dp[2]=dp[1]+dp[0]=2 ，而显然 dp[1]=1 ，因此推出 dp[0]=1 。

```java
class Solution {
    public int translateNum(int num) {
        String s = String.valueOf(num);
        int a = 1, b = 1;
        for(int i = 2; i <= s.length(); i++) {
            String tmp = s.substring(i - 2, i);
            int c = tmp.compareTo("10") >= 0 && tmp.compareTo("25") <= 0 ? 
                a + b : a;
            b = a;
            a = c;
        }
        return a;
    }
}
```

# 47. [礼物的最大价值](https://leetcode.cn/problems/li-wu-de-zui-da-jie-zhi-lcof/)

## 题目描述【中等】

在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

示例 1:

```
输入: 
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 12
解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物
```

## 解题思路

设 f(i,j) 为从棋盘左上角走至单元格 (i,j) 的礼物最大累计价值，易得到以下递推关系：f(i,j) 等于 f(i,j−1) 和 f(i−1,j) 中的较大值加上当前单元格礼物价值
grid(i,j) 。

```
f(i,j) = max[f(i,j−1), f(i−1,j)] + grid(i,j)
```

动态规划解析：

- 状态定义： 设动态规划矩阵 dp ，dp(i,j) 代表从棋盘的左上角开始，到达单元格 (i,j) 时能拿到礼物的最大累计价值。
- 转移方程：
  1. 当 i=0 且 j=0 时，为起始元素；
  2. 当 i=0 且 j≠0 时，为矩阵第一行元素，只可从左边到达；
  3. 当 i≠0 且 j=0 时，为矩阵第一列元素，只可从上边到达；
  4. 当 i≠0 且 j≠0 时，可从左边或上边到达；

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230502150428256.png)

- 初始状态： dp\[0\]\[0\]=grid\[0\]\[0\] ，即到达单元格 (0,0) 时能拿到礼物的最大累计价值为 grid\[0\]\[0\]；
- 返回值： dp\[m−1\]\[n−1\] ，m,n 分别为矩阵的行高和列宽，即返回 dp 矩阵右下角元素。

> 时间复杂度 O(MN) ： M,N 分别为矩阵行高、列宽；动态规划需遍历整个 grid 矩阵，使用 O(MN) 时间。
>
> 空间复杂度 O(1) ： 原地修改使用常数大小的额外空间。

```java
class Solution {
    public int maxValue(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(i == 0 && j == 0) continue;
                if(i == 0) grid[i][j] += grid[i][j - 1] ;
                else if(j == 0) 
                    grid[i][j] += grid[i - 1][j];
                else 
                    grid[i][j] += Math.max(grid[i][j - 1], grid[i - 1][j]);
            }
        }
        return grid[m - 1][n - 1];
    }
}
```

# 48. [最长不含重复字符的子字符串](https://leetcode.cn/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

## 题目描述【中等】

请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

示例 1:

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

示例 2:

```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

示例 3:

```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

## 解题思路

动态规划解析：

- 状态定义： 设动态规划列表 dp ，dp[j] 代表以字符 s[j] 为结尾的 “最长不重复子字符串” 的长度。
- 转移方程： 固定右边界 j ，设字符 s[j] 左边距离最近的相同字符为 s[i] ，即 s[i]=s[j] 。
  1. 当 i<0 ，即 s[j] 左边无相同字符，则 dp[j]=dp[j−1]+1 ；
  2. 当 dp[j−1]<j−i ，说明字符 s[i] 在子字符串 dp[j−1] 区间之外 ，则 dp[j]=dp[j−1]+1 ；
  3. 当 dp[j−1]≥j−i ，说明字符 s[i] 在子字符串 dp[j−1] 区间之中 ，则 dp[j] 的左边界由 s[i] 决定，即 dp[j]=j−i ；

> 由于 dp[j−1]≤j 恒成立，因而 dp[j−1]<j−i 恒成立，因此分支 1. 和 2. 可被合并。
>

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230502212602892.png)

- 返回值： max(dp) ，即全局的 “最长不重复子字符串” 的长度。

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230502213036638.png)

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> dic = new HashMap<>();
        int res = 0, tmp = 0;
        for(int j = 0; j < s.length(); j++) {
            // 获取索引 i
            int i = dic.getOrDefault(s.charAt(j), -1);
            dic.put(s.charAt(j), j); // 更新哈希表
            // dp[j - 1] -> dp[j]
            tmp = tmp < j - i ? tmp + 1 : j - i;
            // max(dp[j - 1], dp[j])
            res = Math.max(res, tmp);
        }
        return res;
    }
}
```

# 49. [丑数](https://leetcode.cn/problems/chou-shu-lcof/)

## 题目描述【中等】

我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

示例:

```
输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
```

说明:  

1. 1 是丑数。
2. n 不超过1690。

## 解题思路

动态规划解析：

- 状态定义： 设动态规划列表 dp[i-1] 代表第 i 个丑数；
- 转移方程：
  1. 当索引 a,b,c 满足以下条件时， dp[i] 为三种情况的最小值；
  2. 每轮计算 dp[i] 后，需要更新索引 a,b,c 的值，使其始终满足方程条件。实现方法：分别独立判断 dp[i] 和 dp[a]×2 , dp[b]×3 , dp[c]×5 的大小关系，若相等则将对应索引 a , b , c 加 1 ；
     - dp[a] * 2 > dp[i-1] ≥ dp[a-1] * 2：即 dp[a] 为首个乘以2后大于dp[i-1]的丑数
     - dp[b] * 3 > dp[i-1] ≥ dp[b-1] * 3：即 dp[b] 为首个乘以3后大于dp[i-1]的丑数
     - dp[c] * 5 > dp[i-1] ≥ dp[c-1] * 5：即 dp[c] 为首个乘以5后大于dp[i-1]的丑数

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230503115207818.png)

- 初始状态： dp[0]=1 ，即第一个丑数为 1 ；
- 返回值： dp[n−1] ，即返回第 n 个丑数；

> 时间复杂度 O(N) ： 其中 N=n ，动态规划需遍历计算 dp 列表。
>
> 空间复杂度 O(N) ： 长度为 N 的 dp 列表使用 O(N) 的额外空间。

```java
class Solution {
    public int nthUglyNumber(int n) {
        int a = 0, b = 0, c = 0;
        int[] dp = new int[n];
        dp[0] = 1;
        for(int i = 1; i < n; i++) {
            int n2 = dp[a] * 2, n3 = dp[b] * 3, n5 = dp[c] * 5;
            dp[i] = Math.min(Math.min(n2, n3), n5);
            if(dp[i] == n2) a++;
            if(dp[i] == n3) b++;
            if(dp[i] == n5) c++;
        }
        return dp[n - 1];
    }
}
```

# 50. [第一个只出现一次的字符](https://leetcode.cn/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

## 题目描述【简单】

在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

示例 1:

```
输入：s = "abaccdeff"
输出：'b'
```

示例 2:

```
输入：s = "" 
输出：' '
```


## 解题思路

有序哈希表：

1. 遍历字符串 `s` ，使用哈希表统计 “各字符数量是否 >1 ”。
2. 再遍历字符串 `s` ，在哈希表中找到首个 “数量为 1 的字符”，并返回。

```java
class Solution {
    public char firstUniqChar(String s) {
        Map<Character, Boolean> dic = new LinkedHashMap<>();
        char[] sc = s.toCharArray();
        for(char c : sc)
            dic.put(c, !dic.containsKey(c));
        for(Map.Entry<Character, Boolean> d : dic.entrySet()){
           if(d.getValue()) return d.getKey();
        }
        return ' ';
    }
}
```

