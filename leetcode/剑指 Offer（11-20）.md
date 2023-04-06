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

![iShot_2023-04-06_21.43.17](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304062144719.gif)

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

# 14. 剪绳子

https://leetcode.cn/problems/jian-sheng-zi-lcof/

## 题目描述

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 `k[0],k[1]...k[m-1]` 。请问 `k[0]*k[1]*...*k[m-1]` 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

```html
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
```

## 解题思路

贪心法。

尽可能得多剪长度为 3 的绳子，并且不允许有长度为 1 的绳子出现。如果出现了，就从已经切好长度为 3 的绳子中拿出一段与长度为 1 的绳子重新组合，把它们切成两段长度为 2 的绳子。以下为证明过程。

将绳子拆成 1 和 n-1，则 1(n-1)-n=-1\<0，即拆开后的乘积一定更小，所以不能出现长度为 1 的绳子。

将绳子拆成 2 和 n-2，则 2(n-2)-n = n-4，在 n\>=4 时这样拆开能得到的乘积会比不拆更大。

将绳子拆成 3 和 n-3，则 3(n-3)-n = 2n-9，在 n\>=5 时效果更好。

将绳子拆成 4 和 n-4，因为 4=2\*2，因此效果和拆成 2 一样。

将绳子拆成 5 和 n-5，因为 5=2+3，而 5\<2\*3，所以不能出现 5 的绳子，而是尽可能拆成 2 和 3。

将绳子拆成 6 和 n-6，因为 6=3+3，而 6\<3\*3，所以不能出现 6 的绳子，而是拆成 3 和 3。这里 6 同样可以拆成 6=2+2+2，但是 3(n - 3) - 2(n - 2) = n - 5 \>= 0，在 n\>=5 的情况下将绳子拆成 3 比拆成 2 效果更好。

继续拆成更大的绳子可以发现都比拆成 2 和 3 的效果更差，因此我们只考虑将绳子拆成 2 和 3，并且优先拆成 3，当拆到绳子长度 n 等于 4 时，也就是出现 3+1，此时只能拆成 2+2。

![image-20230406220939645](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304062209749.png)

```java
public int cutRope(int n) {
    if (n < 2)
        return 0;
    if (n == 2)
        return 1;
    if (n == 3)
        return 2;
    int timesOf3 = n / 3;
    if (n - timesOf3 * 3 == 1)
        timesOf3--;
    int timesOf2 = (n - timesOf3 * 3) / 2;
    return (int) (Math.pow(3, timesOf3)) * (int) (Math.pow(2, timesOf2));
}
```

# 15. 二进制中 1 的个数

https://leetcode.cn/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/

## 题目描述

输入一个整数，输出该数二进制表示中 1 的个数。

## 解题思路

n&(n-1) 位运算可以将 n 的位级表示中最低的那一位 1 设置为 0。不断将 1 设置为 0，直到 n 为 0。时间复杂度：O(M)，其中 M 表示 1 的个数。

<div align="center"> <img src="https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304062219399.gif" width="500px"> </div>


```java
public int NumberOf1(int n) {
    int cnt = 0;
    while (n != 0) {
        cnt++;
        n &= (n - 1);
    }
    return cnt;
}
```

# 16. 数值的整数次方

https://leetcode.cn/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/

## 题目描述

给定一个 double 类型的浮点数 x 和 int 类型的整数 n，求 x 的 n 次方。

```
输入：x = 2.00000, n = 10
输出：1024.00000

输入：x = 2.10000, n = 3
输出：9.26100
```

输入：x = 2.00000, n = -2
输出：0.25000
解释：2<sup>-2</sup> = 1/2<sup>2</sup> = 1/4 = 0.25

## 解题思路

快速幂解析（二分法角度）：快速幂实际上是二分思想的一种应用。

最直观的解法是将 x 重复乘 n 次，`x*x*x...*x`，那么时间复杂度为 O(N)。因为乘法是可交换的，所以可以将上述操作拆开成两半 `(x*x..*x)*(x*x..*x)`，两半的计算是一样的，因此只需要计算一次。而且对于新拆开的计算，又可以继续拆开。这就是分治思想，将原问题的规模拆成多个规模较小的子问题，最后子问题的解合并起来。

本题中子问题是 x<sup>n/2</sup>，在将子问题合并时将子问题的解乘于自身相乘即可。但如果 n 不为偶数，那么拆成两半还会剩下一个 x，在将子问题合并时还需要需要多乘于一个 x。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/image-20201105012506187.png" width="400px"> </div><br>

因为 `(x*x)`<sup>n/2</sup> 可以通过递归求解，并且每次递归 n 都减小一半，因此整个算法的时间复杂度为 O(logN)。

![image-20230406224819975](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304062248076.png)

```java
class Solution {
    public double myPow(double x, int n) {
        if(x == 0) return 0;
        long n1 = n;
        double res = 1.0;
        if (n1 < 0) {
            x = 1 / x;
            n1 = -n1;
        }
        while(n1 > 0) {
            if ((n1 & 1) == 1) res *= x;
            x *= x;
            n1 >>= 1;
        }
        return res;
    }
}
```

# 17. 打印从 1 到最大的 n 位数

## 题目描述

输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数即 999。

```
输入: n = 1
输出: [1,2,3,4,5,6,7,8,9]
```

## 解题思路

由于 n 可能会非常大，因此不能直接用 int 表示数字，而是用 char 数组进行存储。

```java
class Solution {
    public int[] printNumbers(int n) {
        int end = (int)Math.pow(10, n) - 1;
        int[] res = new int[end];
        for(int i = 0; i < end; i++)
            res[i] = i + 1;
        return res;
    }
}
```

# 18.1 在 O(1) 时间内删除链表节点

## 解题思路

① 如果该节点不是尾节点，那么可以直接将下一个节点的值赋给该节点，然后令该节点指向下下个节点，再删除下一个节点，时间复杂度为 O(1)。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/1176f9e1-3442-4808-a47a-76fbaea1b806.png" width="600"/> </div><br>

② 否则，就需要先遍历链表，找到节点的前一个节点，然后让前一个节点指向 null，时间复杂度为 O(N)。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/4bf8d0ba-36f0-459e-83a0-f15278a5a157.png" width="600"/> </div><br>

综上，如果进行 N 次操作，那么大约需要操作节点的次数为 N-1+N=2N-1，其中 N-1 表示 N-1 个不是尾节点的每个节点以 O(1) 的时间复杂度操作节点的总次数，N 表示 1 个尾节点以 O(N) 的时间复杂度操作节点的总次数。(2N-1)/N \~ 2，因此该算法的平均时间复杂度为 O(1)。

```java
public ListNode deleteNode(ListNode head, ListNode tobeDelete) {
    if (head == null || tobeDelete == null)
        return null;
    if (tobeDelete.next != null) {
        // 要删除的节点不是尾节点
        ListNode next = tobeDelete.next;
        tobeDelete.val = next.val;
        tobeDelete.next = next.next;
    } else {
        if (head == tobeDelete)
             // 只有一个节点
            head = null;
        else {
            ListNode cur = head;
            while (cur.next != tobeDelete)
                cur = cur.next;
            cur.next = null;
        }
    }
    return head;
}
```

# 18.2 删除链表中重复的结点

[牛客网](https://www.nowcoder.com/practice/fc533c45b73a41b0b44ccba763f866ef?tpId=13&tqId=11209&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking&from=cyc_github)

## 题目描述

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/17e301df-52e8-4886-b593-841a16d13e44.png" width="450"/> </div><br>

## 解题描述

```java
public ListNode deleteDuplication(ListNode pHead) {
    if (pHead == null || pHead.next == null)
        return pHead;
    ListNode next = pHead.next;
    if (pHead.val == next.val) {
        while (next != null && pHead.val == next.val)
            next = next.next;
        return deleteDuplication(next);
    } else {
        pHead.next = deleteDuplication(pHead.next);
        return pHead;
    }
}
```

# 19. 正则表达式匹配

[牛客网](https://www.nowcoder.com/practice/28970c15befb4ff3a264189087b99ad4?tpId=13&tqId=11205&tab=answerKey&from=cyc_github)

## 题目描述

请实现一个函数用来匹配包括 '.' 和 '\*' 的正则表达式。模式中的字符 '.' 表示任意一个字符，而 '\*' 表示它前面的字符可以出现任意次（包含 0 次）。

在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串 "aaa" 与模式 "a.a" 和 "ab\*ac\*a" 匹配，但是与 "aa.a" 和 "ab\*a" 均不匹配。

## 解题思路

应该注意到，'.' 是用来当做一个任意字符，而 '\*' 是用来重复前面的字符。这两个的作用不同，不能把 '.' 的作用和 '\*' 进行类比，从而把它当成重复前面字符一次。

```java
public boolean match(String str, String pattern) {

    int m = str.length(), n = pattern.length();
    boolean[][] dp = new boolean[m + 1][n + 1];

    dp[0][0] = true;
    for (int i = 1; i <= n; i++)
        if (pattern.charAt(i - 1) == '*')
            dp[0][i] = dp[0][i - 2];

    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++)
            if (str.charAt(i - 1) == pattern.charAt(j - 1) || pattern.charAt(j - 1) == '.')
                dp[i][j] = dp[i - 1][j - 1];
            else if (pattern.charAt(j - 1) == '*')
                if (pattern.charAt(j - 2) == str.charAt(i - 1) || pattern.charAt(j - 2) == '.') {
                    dp[i][j] |= dp[i][j - 1]; // a* counts as single a
                    dp[i][j] |= dp[i - 1][j]; // a* counts as multiple a
                    dp[i][j] |= dp[i][j - 2]; // a* counts as empty
                } else
                    dp[i][j] = dp[i][j - 2];   // a* only counts as empty

    return dp[m][n];
}
```

# 20. 表示数值的字符串

[牛客网](https://www.nowcoder.com/practice/e69148f8528c4039ad89bb2546fd4ff8?tpId=13&tqId=11206&tab=answerKey&from=cyc_github)

## 题目描述

```
true

"+100"
"5e2"
"-123"
"3.1416"
"-1E-16"
```

```
false

"12e"
"1a3.14"
"1.2.3"
"+-5"
"12e+4.3"
```


## 解题思路

使用正则表达式进行匹配。

```html
[]  ： 字符集合
()  ： 分组
?   ： 重复 0 ~ 1 次
+   ： 重复 1 ~ n 次
*   ： 重复 0 ~ n 次
.   ： 任意字符
\\. ： 转义后的 .
\\d ： 数字
```

```java
public boolean isNumeric (String str) {
    if (str == null || str.length() == 0)
        return false;
    return new String(str).matches("[+-]?\\d*(\\.\\d+)?([eE][+-]?\\d+)?");
}
```



