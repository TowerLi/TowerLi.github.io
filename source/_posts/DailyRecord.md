---
title: LeetCode每日一题吐槽
date: 2020-07-22 16:04:57
tags:	
---

今天工作日，按照日常，就打开了每日一题做做，没想到来了一道easy，还和昨天做的一模一样（我以为的），于是我直接用昨天的思路，二分法去找最小值。

```
剑指 Offer 11. 旋转数组的最小数字
把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组
的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 
的一个旋转，该数组的最小值为1。  

示例 1：

输入：[3,4,5,1,2]
输出：1
示例 2：

输入：[2,2,2,0,1]
输出：0
```



我的错误代码： 

```
class Solution {
    public int minArray(int[] numbers) {
        if (numbers == null || numbers.length == 0) {
            return 0;
        }
        int len = numbers.length;
        int left = 0;
        int right = len - 1;
        while (left < right) {
            int mid = ( (right - left) >> 1) + left;
            if (numbers[mid] < numbers[right]) {
                right = mid;
            }else {
                left = mid + 1;
            }
        }
        return numbers[left];
    }
}
```

![avatar](./DailyRecord/error.png)

我总是倒在了     [1,3,3], [3,3,1,3] 这样的测试案例上，这时候我才发现这题和昨天做的【[153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)】不一样，最大的难点和区别在于这个题的有序数组是可以包含重复元素的，也就是说当你发现mid的值和right值相等的时候，无法确定最小值在mid的左边还是右边！

我一直苦苦的调试，找解决方案，终究不得解。

最后打开了题解，发现了大神们的做法是每当发现mid的值和right的值相等的时候，right就往前退一位，这样就很好地排除了重复元素，实在是一个高明的做法，**太秀了**！



附AC代码：

```
class Solution {
    public int minArray(int[] numbers) {
        if (numbers == null || numbers.length == 0) {
            return 0;
        }
        int len = numbers.length;
        int left = 0;
        int right = len - 1;
        /**
        [1,3,3]
        [3,3,1,3]
        [1,2,3,4,5]
        [4,5,1,2,3]
        [1,2,3,4,5,6,7]
        [6,7,1,2,3,4,5]
        [4,5,6,7,1,2,3]
        */
        while (left < right) {
            int mid = ( (right - left) >> 1) + left;
            if (numbers[mid] < numbers[right]) {
                right = mid;
            }else if (numbers[mid] > numbers[right]){
                left = mid + 1;
            }else {
                //去重
                right--;
            }
        }
        return numbers[left];
    }
}
```





> You know that why your girls never send message to you and your funds always at a loss.
>
> 你知道为什么女孩子永远不给你主动发消息，也知道为什么基金总是亏钱。

> If you're not satisfied with the life you're living, don't just complain. Do something about it.
> 对于现况的不满，你不能只是抱怨，而是要有勇气作出改变。







