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

# 26. [树的子结构](https://leetcode.cn/problems/shu-de-zi-jie-gou-lcof/)

## 题目描述【中等】

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如:


    给定的树 A:
         3
        / \
       4   5
      / \
     1   2
    
    给定的树 B：
       4 
      /
     1
    返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。
示例1：

```
输入：A = [1,2,3], B = [3,1]
输出：false
```

示例2：

```
输入：A = [3,4,5,1,2], B = [4,1]
输出：true
```

## 解题思路

判断树 B 是否是树 A 的子结构，需完成以下两步工作：

1. 先序遍历树 A 中的每个节点 n<sub>A</sub>，对应函数 isSubStructure(A, B)；
2. 判断树 A 中 以 n<sub>A</sub> 为根节点的子树 是否包含树 B，对应函数 recur(A, B)。

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304161020465.png)

树 A 的根节点记作节点 A，树 B 的根节点记作节点 B 。

**isSubStructure(A, B) 函数：**

1. 特例处理： 当 树 A 为空 或 树 B 为空 时，直接返回 false ；
2. 返回值： 若树 B 是树 A 的子结构，则必满足以下三种情况之一，因此用或 || 连接；
   - 以 节点 A 为根节点的子树 包含树 B ，对应 recur(A, B)；
   - 树 B 是 树 A 左子树 的子结构，对应 isSubStructure(A.left, B)；
   - 树 B 是 树 A 右子树 的子结构，对应 isSubStructure(A.right, B)；

**`recur(A, B)` 函数：**

终止条件：

1. 当节点 B 为空：说明树 B 已匹配完成（越过叶子节点），因此返回 true ；
2. 当节点 A 为空：说明已经越过树 A 叶子节点，即匹配失败，返回 false ；
3. 当节点 A 和 B 的值不同：说明匹配失败，返回 false ；

返回值：

1. 判断 A 和 B 的左子节点是否相等，即 recur(A.left, B.left) ；
2. 判断 A 和 B 的右子节点是否相等，即 recur(A.right, B.right) ；

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304161028432.gif)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        return (A != null && B != null) 
            && (recur(A, B) || isSubStructure(A.left, B) || isSubStructure(A.right, B));
    }
    boolean recur(TreeNode A, TreeNode B) {
        if(B == null) return true;
        if(A == null || A.val != B.val) return false;
        return recur(A.left, B.left) && recur(A.right, B.right);
    }
}
```

# 27. [二叉树的镜像](https://leetcode.cn/problems/er-cha-shu-de-jing-xiang-lcof/)

## 题目描述【简单】

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

例如输入：

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

镜像输出：

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

## 解题思路

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304161044760.png)

递归法：根据二叉树镜像的定义，考虑递归遍历（dfs）二叉树，交换每个节点的左 / 右子节点，即可生成二叉树的镜像。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        if (root == null) return null;
        TreeNode leftRoot = mirrorTree(root.right);
        TreeNode rightRoot = mirrorTree(root.left);
        root.left = leftRoot;
        root.right = rightRoot;
        return root;
    }
}
```

# 28. [对称的二叉树](https://leetcode.cn/problems/dui-cheng-de-er-cha-shu-lcof/)

## 题目描述【简单】

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

```
    1
   / \
  2   2
   \   \
   3    3
```

**示例 1：**

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

**示例 2：**

```
输入：root = [1,2,2,null,3,null,3]
输出：false
```

## 解题思路

对称二叉树定义： 对于树中 任意两个对称节点 L 和 R ，一定有：

- L.val=R.val ：即此两对称节点值相等；
- L.left.val=R.right.val ：即 L 的 左子节点 和 R 的 右子节点 对称；
- L.right.val=R.left.val ：即 L 的 右子节点 和 R 的 左子节点 对称。

![image-20230416111124772](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304161111914.png)

根据以上规律，考虑从顶至底递归，判断每对节点是否对称，从而判断树是否为对称二叉树。

**算法流程：**

isSymmetric(root) ：

- 特例处理： 若根节点 root 为空，则直接返回 true 。
- 返回值： 即 recur(root.left, root.right) ;

recur(L, R) ：

- 终止条件：
  - 当 L 和 R 同时越过叶节点： 此树从顶至底的节点都对称，因此返回 true ；
  - 当 L 或 R 中只有一个越过叶节点： 此树不对称，因此返回 false ；
  - 当节点 L 值 ≠ 节点 R 值： 此树不对称，因此返回 false ；
- 递推工作：
  - 判断两节点 L.left 和 R.right 是否对称，即 recur(L.left, R.right) ；
  - 判断两节点 L.right 和 R.left 是否对称，即 recur(L.right, R.left) ；

返回值： 两对节点都对称时，才是对称树，因此用与逻辑符 && 连接。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return root == null ? true : recur(root.left, root.right);
    }
    boolean recur(TreeNode L, TreeNode R) {
        if(L == null && R == null) return true;
        if(L == null || R == null || L.val != R.val) return false;
        return recur(L.left, R.right) && recur(L.right, R.left);
    }
}
```

# 29. [顺时针打印矩阵](https://leetcode.cn/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

## 题目描述【简单】

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

示例1：

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

示例2：

```
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```

## 解题思路

考虑设定矩阵的“左、上、右、下”四个边界，模拟以上矩阵遍历顺序。

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304161125075.png)

算法流程：

1. 空值处理： 当 matrix 为空时，直接返回空列表 [] 即可。
2. 初始化： 矩阵 左、右、上、下 四个边界 l , r , t , b ，用于打印的结果列表 res 。
3. 循环打印：“从左向右、从上向下、从右向左、从下向上” 四个方向循环，每个方向打印中做以下三件事 （各方向的具体信息见下表）；
   - 根据边界打印，即将元素按顺序添加至列表 res 尾部；
   - 边界向内收缩 1 （代表已被打印）；
   - 判断是否打印完毕（边界是否相遇），若打印完毕则跳出。
4. 返回值： 返回 res 即可。

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304161130241.gif)

```java
class Solution {
    public int[] spiralOrder(int[][] matrix) {
        if(matrix.length == 0) return new int[0];
        int l = 0, r = matrix[0].length - 1, t = 0, b = matrix.length - 1, x = 0;
        int[] res = new int[(r + 1) * (b + 1)];
        while(true) {
            //从左往右，上边界内缩：++t
            for(int i = l; i <= r; i++) res[x++] = matrix[t][i];
            if(++t > b) break;
            //从上往下，右边界收缩：--r
            for(int i = t; i <= b; i++) res[x++] = matrix[i][r];
            if(l > --r) break;
            //从右往左，下边界收缩：--b
            for(int i = r; i >= l; i--) res[x++] = matrix[b][i];
            if(t > --b) break;
            //从下到上，左边界收缩：++l
            for(int i = b; i >= t; i--) res[x++] = matrix[i][l];
            if(++l > r) break;
        }
        return res;
    }
}
```

# 30. [包含min函数的栈](https://leetcode.cn/problems/bao-han-minhan-shu-de-zhan-lcof/)

## 题目描述【简单】

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

示例：

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.min();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.min();   --> 返回 -2.
```


## 解题思路

本题难点： 将 min() 函数复杂度降为 O(1) ，可通过建立辅助栈实现；

- 数据栈 A ： 栈 A 用于存储所有元素，保证入栈 push() 函数、出栈 pop() 函数、获取栈顶 top() 函数的正常逻辑。
- 辅助栈 B ： 栈 B 中存储栈 A 中所有 非严格降序 的元素，则栈 A 中的最小元素始终对应栈 B 的栈顶元素，即 min() 函数只需返回栈 B 的栈顶元素即可。

因此，只需设法维护好 栈 B 的元素，使其保持非严格降序，即可实现 `min()` 函数的 O(1) 复杂度。

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304161150406.png)



```java
class MinStack {
    Stack<Integer> A, B;
    public MinStack() {
        A = new Stack<>();
        B = new Stack<>();
    }
    public void push(int x) {
        A.add(x);
        if(B.empty() || B.peek() >= x)
            B.add(x);
    }
    public void pop() {
        if(A.pop().equals(B.peek()))
            B.pop();
    }
    public int top() {
        return A.peek();
    }
    public int min() {
        return B.peek();
    }
}
```

