# 31. [ 栈的压入、弹出序列](https://leetcode.cn/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

## 题目描述【中等】

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

示例 1：

```
输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true
解释：我们可以按以下顺序执行：
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```

示例 2：

```
输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
输出：false
解释：1 不能在 2 之前弹出。
```

## 解题思路

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304182146757.gif)

```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        Stack<Integer> stack = new Stack<>();
        int i = 0;
        for(int num : pushed) {
            stack.push(num); // num 入栈
            while(!stack.isEmpty() && stack.peek() == popped[i]) { // 循环判断与出栈
                stack.pop();
                i++;
            }
        }
        return stack.isEmpty();
    }
}
```

# 32-1. [从上到下打印二叉树](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

## 题目描述【中等】

从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

例如：给定二叉树: `[3,9,20,null,null,15,7]`

```
    3
   / \
  9  20
    /  \
   15   7
```

返回：

```
[3,9,20,15,7]
```

## 解题思路

题目要求的二叉树的 **从上至下** 打印（即按层打印），又称为二叉树的 **广度优先搜索**（BFS）。

BFS 通常借助 **队列** 的先入先出特性来实现。

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304182156363.gif)

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
    public int[] levelOrder(TreeNode root) {
        if(root == null) return new int[0];
        Queue<TreeNode> queue = new LinkedList<>(){{ add(root); }};
        ArrayList<Integer> ans = new ArrayList<>();
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            ans.add(node.val);
            if(node.left != null) queue.add(node.left);
            if(node.right != null) queue.add(node.right);
        }
        int[] res = new int[ans.size()];
        for(int i = 0; i < ans.size(); i++)
            res[i] = ans.get(i);
        return res;
    }
}
```

# 32-2. [从上到下打印二叉树 II](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

## 题目描述【简单】

从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

例如：给定二叉树: `[3,9,20,null,null,15,7]`

```
    3
   / \
  9  20
    /  \
   15   7
```

返回其层次遍历结果：

```
[
  [3],
  [9,20],
  [15,7]
]
```

## 解题思路

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304182242576.gif)

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
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        if(root != null) queue.add(root);
        while(!queue.isEmpty()) {
            List<Integer> tmp = new ArrayList<>();
            for(int i = queue.size(); i > 0; i--) {
                TreeNode node = queue.poll();
                tmp.add(node.val);
                if(node.left != null) queue.add(node.left);
                if(node.right != null) queue.add(node.right);
            }
            res.add(tmp);
        }
        return res;
    }
}
```

# 32-3. [从上到下打印二叉树 III](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

## 题目描述【中等】

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

例如：给定二叉树: `[3,9,20,null,null,15,7]`

```
    3
   / \
  9  20
    /  \
   15   7
```

返回：

```
[3,9,20,15,7]
```

## 解题思路

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
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        if(root != null) queue.add(root);
        while(!queue.isEmpty()) {
            LinkedList<Integer> tmp = new LinkedList<>();
            for(int i = queue.size(); i > 0; i--) {
                TreeNode node = queue.poll();
                if(res.size() % 2 == 0) tmp.addLast(node.val); // 偶数层 -> 队列头部
                else tmp.addFirst(node.val); // 奇数层 -> 队列尾部
                if(node.left != null) queue.add(node.left);
                if(node.right != null) queue.add(node.right);
            }
            res.add(tmp);
        }
        return res;
    }
}
```

# 【33】. [二叉搜索树的后序遍历序列](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

## 题目描述【中等】

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 `true`，否则返回 `false`。假设输入的数组的任意两个数字都互不相同。

参考以下这颗二叉搜索树：

```
     5
    / \
   2   6
  / \
 1   3
```

**示例 1：**

```
输入: [1,6,3,2,5]
输出: false
```

**示例 2：**

```
输入: [1,3,2,6,5]
输出: true
```

## 解题思路

**二叉搜索树定义：** 左子树中所有节点的值 << 根节点的值；右子树中所有节点的值 >> 根节点的值；其左、右子树也分别为二叉搜索树。

![](https://technotes.oss-cn-shenzhen.aliyuncs.com/2023/202304182334083.png)



```java
class Solution {
    public boolean verifyPostorder(int[] postorder) {

    }
}
```

# 34. 

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

# 35. 

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

# 36. 

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

# 37. 

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

# 38. 

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

# 39. 

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

# 40. 

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

