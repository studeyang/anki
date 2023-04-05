# 11. 旋转数组的最小数字

[牛客网](https://www.nowcoder.com/practice/9f3231a991af4f55b95579b44b7a01ba?tpId=13&tqId=11159&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking&from=cyc_github)

## 题目描述

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/0038204c-4b8a-42a5-921d-080f6674f989.png" width="210px"> </div><br>

## 解题思路

排序数组的查找问题首先考虑使用 二分法 解决。

1、初始化 i、j、m

![image-20230405204830938](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230405204830938.png)

2、因为 array[m] > array[j]，执行 i = m + 1，同时更新 m 的位置

![image-20230405205034652](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230405205034652.png)

3、因为 array[m] < array[j]，执行 j = m，同时更新 m 的位置

![image-20230405205042435](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230405205042435.png)

4、因为 array[m] < array[j]，执行 j = m

![image-20230405205049978](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230405205049978.png)

5、因为 i == j；跳出循环返回 array[i] = 1

![image-20230405205057883](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230405205057883.png)

```java
import java.util.ArrayList;
public class Solution {
    public int minNumberInRotateArray(int [] array) {
        // 特殊情况判断
        if (array.length == 0) {
            return 0;
        }
        // 左右指针i j
        int i = 0, j = array.length - 1;
        // 循环
        while (i < j) {
            // 找到数组的中点 m
            int m = (i + j) / 2;
            // m在左排序数组中，旋转点在 [m+1, j] 中
            if (array[m] > array[j]) i = m + 1;
            // m 在右排序数组中，旋转点在 [i, m]中
            else if (array[m] < array[j]) j = m;
            // 缩小范围继续判断
            else j--;
        }
        // 返回旋转点
        return array[i];
    }
}
```

# 12. 矩阵中的路径

https://leetcode.cn/problems/ju-zhen-zhong-de-lu-jing-lcof/

## 题目描述

给定一个 `m x n` 二维字符网格 `board` 和一个字符串单词 `word` 。如果 `word` 存在于网格中，返回 `true` ；否则，返回 `false` 。

例如，在下面的 3×4 的矩阵中包含单词 "ABCCED"（单词中的字母已标出）。

![image-20230405211952559](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230405211952559.png)

示例：

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```

## 解题思路

使用 深度优先搜索（DFS）+ 剪枝 解决。

深度优先搜索： 可以理解为暴力法遍历矩阵中所有字符串可能性。DFS 通过递归，先朝一个方向搜到底，再回溯至上个节点，沿另一个方向搜索，以此类推。

剪枝： 在搜索中，遇到 这条路不可能和目标字符串匹配成功 的情况，则应立即返回，称之为 可行性剪枝 。

<div align="center"> <img src="https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/image-20230405213151700.png" width="400px"> </div>

本题的输入是数组而不是矩阵（二维数组），因此需要先将数组转换成矩阵。

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        char[] words = word.toCharArray();
        for(int h = 0; h < board.length; h++) {
            for(int l = 0; l < board[0].length; l++) {
                if (dfs(board, words, h, l, 0)) return true;
            }
        }
        return false;
    }
    boolean dfs(char[][] board, char[] words, int h, int l, int wordIndex) {
        if (h >= board.length || h < 0 
            || l >= board[0].length || l < 0 
            || board[h][l] != words[wordIndex]) {
            return false;
        }
        if (wordIndex == words.length - 1) return true;
        board[h][l] = '\0';
        boolean res = dfs(board, words, h + 1, l, wordIndex + 1) 
            || dfs(board, words, h - 1, l, wordIndex + 1) 
            || dfs(board, words, h, l + 1, wordIndex + 1) 
            || dfs(board, words, h , l - 1, wordIndex + 1);
        board[h][l] = words[wordIndex];
        return res;
    }
}
```

