## 题目描述：

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例：**

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

----

## 解题思路：

### 1、迭代法：

先创建一个哨兵结点 `prehead`，我们对 l1 和 l2 进行迭代，直到 l1 或 l2 指向 null 时停止迭代。每迭代一次，判断 l1  和 l2 的值大小。

- 如果  l1 < l2，我们将 l1 加在 `curr` 结点后面，同时将 l1 的指针向后移一位；
- 如果  l2 < l1，我们将 l2 加在 `curr` 结点后面，同时将 l2 的指针向后移一位。

不管我们添加了 l1 还是 l2，我们都需要将 `curr` 指针后移一位。如果 l1 或 l2 指向 `null`，将跳出循环，并将不为空的链表加载到 `curr` 的后面，最后返回的是哨兵结点的下一个结点，即 `preHead.next`。

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcepz0h0j2j30ki054q3b.jpg" alt="newname2020-03-0120.52.54" style="zoom: 50%;" />

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
    ListNode preHead = new ListNode(0);
    ListNode curr =preHead;
    while(l1!=null && l2!=null){
      if(l1.val<l2.val){
        curr.next = new ListNode(l1.val);
        l1 = l1.next;
      }else {
        curr.next = new ListNode(l2.val);
        l2 = l2.next;
      }
      curr=curr.next;
    }
    if(l1!=null) curr.next=l1;
    if(l2!=null) curr.next=l2;
    return preHead.next;
  }
}
```



### 2、递归法：

第一次进入递归时，判断 l1 和 l2 的头结点谁小，

- 如果 l1 小，则 l1 为返回合并链表的头结点，然后递归调用判断 l1.next 和 l2 谁小，将小的一方加到 l1.next ；
- 如果 l2 小，则 l2 为返回合并链表的头结点，然后递归调用判断 l1 和 l2.next 谁小，将小的一方加到 l2.next 。

不断递归，直到 l1 = null 或者 l2 = null 时，结束递归，返回 l1 或 l2。

[leetCode上本题题解](https://leetcode-cn.com/problems/merge-two-sorted-lists/solution/hua-jie-suan-fa-21-he-bing-liang-ge-you-xu-lian-bi/)

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gceqferxgcj30kq05274o.jpg" alt="newname2020-03-0121.08.51" style="zoom:50%;" />

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
        if(l1==null){
            return l2;
        }
        if(l2==null){
            return l1;
        }
        if(l1.val<l2.val){
            l1.next=mergeTwoLists(l1.next,l2);
            return l1;
        }else{
            l2.next=mergeTwoLists(l1,l2.next);
            return l2;
        }
    }
}
```

