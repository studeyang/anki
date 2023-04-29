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



```java

```

# 43. 

## 题目描述【困难】

## 解题思路

```java

```

# 44. 

## 题目描述【中等】

## 解题思路



```java

```

# 45. 

## 题目描述【中等】

## 解题思路




```java

```

# 46. 

## 题目描述【】



## 解题思路



```java

```

# 47. 

## 题目描述【】

## 解题思路



```java

```

# 48. 

## 题目描述【】

## 解题思路



```java

```

# 49. 

## 题目描述【】

## 解题思路



```java

```

# 50. 

## 题目描述【】


## 解题思路



```java

```

