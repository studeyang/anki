# 21. [调整数组顺序使奇数位于偶数前面](https://leetcode.cn/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

## 题目描述【简单】

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数在数组的前半部分，所有偶数在数组的后半部分。

示例：

```
输入：nums = [1,2,3,4]
输出：[1,3,2,4] 
注：[3,1,2,4] 也是正确的答案之一。
```

## 解题思路

1、初始化：i,j 双指针，分别指向数据 nums 左右两端；

2、循环交换：当 i = j 时跳出；

- 指针 i 遇到奇数则执行 i = i + 1 跳过，直到找到偶数；
- 指针 j 遇到偶数则执行 j = j - 1 跳过，直到找到奇数；
- 交换 nums[i] 和 nums[j] 值；

3、返回值：返回已修改的 nums 数组。

![iShot_2023-04-15_23.26.49](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304152327404.gif)

```java
class Solution {
    public int[] exchange(int[] nums) {
        int i = 0, j = nums.length - 1, tmp;
        while(i < j) {
            while(i < j && (nums[i] & 1) == 1) i++;
            while(i < j && (nums[j] & 1) == 0) j--;
            tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;
        }
        return nums;
    }
}
```

# 22. [链表中倒数第k个节点](https://leetcode.cn/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

## 题目描述【简单】

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```

## 解题思路

1. **初始化：** 前指针 `former` 、后指针 `latter` ，双指针都指向头节点 `head` 。
2. **构建双指针距离：** 前指针 `former` 先向前走 k 步（结束后，双指针 former 和 latter 间相距 k 步）。
3. 双指针共同移动： 循环中，双指针 former 和 latter 每轮都向前走一步，直至 former 走过链表 尾节点 时跳出（跳出后， latter 与尾节点距离为 k−1，即 latter 指向倒数第 k 个节点）。
4. 返回值： 返回 latter 即可。

![iShot_2023-04-15_23.38.36](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304152339874.gif)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        ListNode former = head, latter = head;
        for(int i = 0; i < k; i++)
            former = former.next;
        while(former != null) {
            former = former.next;
            latter = latter.next;
        }
        return latter;
    }
}
```

# 24. [反转链表](https://leetcode.cn/problems/fan-zhuan-lian-biao-lcof/)

## 题目描述【简单】

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

```html
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

## 解题思路

考虑遍历链表，并在访问各节点时修改 `next` 引用指向，算法流程见注释。

![iShot_2023-04-15_23.45.23](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304152345006.gif)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode cur = head, pre = null;
        while(cur != null) {
            ListNode tmp = cur.next; // 暂存后继节点 cur.next
            cur.next = pre;          // 修改 next 引用指向
            pre = cur;               // pre 暂存 cur
            cur = tmp;               // cur 访问下一节点
        }
        return pre;
    }
}
```

# 25. [合并两个排序的链表](https://leetcode.cn/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

## 题目描述【简单】

输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

示例1：

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

## 解题思路

1. 初始化
2. 循环合并
3. 合并剩余尾部


```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dum = new ListNode(0), cur = dum;
        while(l1 != null && l2 != null) {
            if(l1.val < l2.val) {
                cur.next = l1;
                l1 = l1.next;
            } else {
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }
        cur.next = l1 != null ? l1 : l2;
        return dum.next;
    }
}
```

# 26. 

## 题目描述【中等】

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

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/image-20201105012506187.png" width="400px"> </div>

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

# 27. 

## 题目描述【简单】

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
        int end = (int) Math.pow(10, n) - 1;
        int[] res = new int[end];
        for(int i = 0; i < end; i++)
            res[i] = i + 1;
        return res;
    }
}
```

# 28. 

## 题目描述【简单】

给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

```
输入: head = [4,5,1,9], val = 5
输出: [4,1,9]
解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
```

## 解题思路

本题删除值为 val 的节点分需为两步：定位节点、修改引用。

1. 定位节点： 遍历链表，直到 head.val == val 时跳出，即可定位目标节点。
2. 修改引用： 设节点 cur 的前驱节点为 pre ，后继节点为 cur.next ；则执行 pre.next = cur.next ，即可实现删除 cur 节点。

![image-20230407222417109](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304072224255.png)

```java
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        if(head.val == val) return head.next;
        ListNode pre = head, cur = head.next;
        while(cur != null && cur.val != val) {
            pre = cur;
            cur = cur.next;
        }
        if(cur != null) pre.next = cur.next;
        return head;
    }
}
```

# 29. 

## 题目描述【简单】

`请实现一个函数用来匹配包含'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但与"aa.a"和"ab*a"均不匹配。`

```
输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。

输入:
s = "aa"
p = "a*"
输出: true
解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
```

## 解题思路

https://leetcode.cn/problems/zheng-ze-biao-da-shi-pi-pei-lcof/solution/jian-zhi-offer-19-zheng-ze-biao-da-shi-pi-pei-dong/

设 s 的长度为 n，p 的长度为 m；

将 s 的第 i 个字符记为 s<sub>i</sub> ，p 的第 j 个字符记为 p<sub>j</sub>；（i, j 从 0 开始）

将 s 的前 i 个字符组成的子字符串记为 s[:i]。

因此，本题可转化为求 s[:n] 是否能和 p[:m] 匹配。假设 s[:n] 与 p[:m] 可以匹配，那么下一状态有两种：

1. 添加一个字符 s<sub>i+1</sub> 后是否能匹配？
2. 添加字符 p<sub>j+1</sub> 后是否能匹配？

![image-20230409231830063](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304092318189.png)

当 `p[j - 1] = '*'` 时， `dp[i][j]` 在当以下任一情况为 true 时等于 true：

1. **`dp[i][j - 2]`:** 即将字符组合 `p[j - 2] *` 看作出现 0 次时，能否匹配；
2. **`dp[i - 1][j]` 且 `s[i - 1] = p[j - 2]`:** 即让字符 `p[j - 2]` 多出现 1 次时，能否匹配；
3. **`dp[i - 1][j]` 且 `p[j - 2] = '.'`:** 即让字符 `'.'` 多出现 1 次时，能否匹配；

当 `p[j - 1] != '*'` 时， `dp[i][j]` 在当以下任一情况为 true 时等于 true：

1. **`dp[i - 1][j - 1]` 且 `s[i - 1] = p[j - 1]`：** 即让字符 `p[j - 1]` 多出现一次时，能否匹配；
2. **`dp[i - 1][j - 1]` 且 `p[j - 1] = '.'`：** 即将字符 `.` 看作字符 `s[i - 1]` 时，能否匹配；

初始化： 需要先初始化 dp 矩阵首行，以避免状态转移时索引越界。

- `dp[0][0] = true`： 代表两个空字符串能够匹配。
- `dp[0][j] = dp[0][j - 2] 且 p[j - 1] = '*'`： 首行 s 为空字符串，因此当 p 的偶数位为 * 时才能够匹配（即让 p 的奇数位出现 0 次，保持 p 是空字符串）。因此，循环遍历字符串 p ，步长为 2（即只看偶数位）。

![image-20230409223134258](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304092231402.png)

![iShot_2023-04-09_23.37.41](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304092338363.gif)

```java
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length() + 1, n = p.length() + 1;
        boolean[][] dp = new boolean[m][n];
        dp[0][0] = true;
        // 初始化首行
        for(int j = 2; j < n; j += 2)
            dp[0][j] = dp[0][j - 2] && p.charAt(j - 1) == '*';
        // 状态转移
        for(int i = 1; i < m; i++) {
            for(int j = 1; j < n; j++) {
                if(p.charAt(j - 1) == '*') {
                    if(dp[i][j - 2]) dp[i][j] = true;                                            // 1.
                    else if(dp[i - 1][j] && s.charAt(i - 1) == p.charAt(j - 2)) dp[i][j] = true; // 2.
                    else if(dp[i - 1][j] && p.charAt(j - 2) == '.') dp[i][j] = true;             // 3.
                } else {
                    if(dp[i - 1][j - 1] && s.charAt(i - 1) == p.charAt(j - 1)) dp[i][j] = true;  // 1.
                    else if(dp[i - 1][j - 1] && p.charAt(j - 1) == '.') dp[i][j] = true;         // 2.
                }
            }
        }
        return dp[m - 1][n - 1];
    }
}

```

# 30. 

## 题目描述【简单】

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。

数值（按顺序）可以分成以下几个部分：

- 若干空格
- 一个 小数 或者 整数
- （可选）一个 'e' 或 'E' ，后面跟着一个 整数
- 若干空格

小数（按顺序）可以分成以下几个部分：

- （可选）一个符号字符（'+' 或 '-'）
- 下述格式之一：
  - 至少一位数字，后面跟着一个点 '.'
  - 至少一位数字，后面跟着一个点 '.' ，后面再跟着至少一位数字
  - 一个点 '.' ，后面跟着至少一位数字

整数（按顺序）可以分成以下几个部分：

- （可选）一个符号字符（'+' 或 '-'）
- 至少一位数字

部分数值列举：["+100", "5e2", "-123", "3.1416", "-1E-16", "0123"]

部分非数值列举：["12e", "1a3.14", "1.2.3", "+-5", "12e+5.4"]


## 解题思路

https://leetcode.cn/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/solution/mian-shi-ti-20-biao-shi-shu-zhi-de-zi-fu-chuan-y-2/

本题使用有限状态自动机。根据字符类型和合法数值的特点，先定义状态，再画出状态转移图，最后编写代码即可。

**状态定义：**

按照字符串从左到右的顺序，定义以下 9 种状态。

0. 开始的空格
1. 幂符号前的正负号
2. 小数点前的数字
3. 小数点、小数点后的数字
4. 当小数点前为空格时，小数点、小数点后的数字
5. 幂符号
6. 幂符号后的正负号
7. 幂符号后的数字
8. 结尾的空格

**结束状态：**

合法的结束状态有 2, 3, 7, 8 。

![image-20230409234426231](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304092344340.png)

```java
class Solution {
    public boolean isNumber(String s) {
        Map[] states = {
            new HashMap<>() {{ put(' ', 0); put('s', 1); put('d', 2); put('.', 4); }}, // 0.
            new HashMap<>() {{ put('d', 2); put('.', 4); }},                           // 1.
            new HashMap<>() {{ put('d', 2); put('.', 3); put('e', 5); put(' ', 8); }}, // 2.
            new HashMap<>() {{ put('d', 3); put('e', 5); put(' ', 8); }},              // 3.
            new HashMap<>() {{ put('d', 3); }},                                        // 4.
            new HashMap<>() {{ put('s', 6); put('d', 7); }},                           // 5.
            new HashMap<>() {{ put('d', 7); }},                                        // 6.
            new HashMap<>() {{ put('d', 7); put(' ', 8); }},                           // 7.
            new HashMap<>() {{ put(' ', 8); }}                                         // 8.
        };
        int p = 0;
        char t;
        for(char c : s.toCharArray()) {
            if(c >= '0' && c <= '9') t = 'd';
            else if(c == '+' || c == '-') t = 's';
            else if(c == 'e' || c == 'E') t = 'e';
            else if(c == '.' || c == ' ') t = c;
            else t = '?';
            if(!states[p].containsKey(t)) return false;
            p = (int)states[p].get(t);
        }
        return p == 2 || p == 3 || p == 7 || p == 8;
    }
}
```

