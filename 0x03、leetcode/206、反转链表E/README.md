## 题目描述：

反转一个单链表。

**示例:**

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

**进阶:**

你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

----

## 解题思路：

### 1、迭代法

申请两个结点 `pre` 和 `temp` ，其中 `temp` 用来存储 `head.next`，`pre` 用来存储 `head` 的前一个结点。然后通过迭代使 `head` 指向 `pre`，`head` 和 `pre` 都向前移一位，最后当 `head = null` 时，停止迭代，此时 `pre` 就是头结点了。

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcedtj8ajgj30pc04uq3c.jpg" alt="newname2020-03-0113.52.25" style="zoom: 50%;" />

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
//答案
class Solution {
  public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode temp = null;
    while (head != null) {
      //记录当前结点的下一个下一个结点
      temp = head.next;
      //当前结点指向 pre
      head.next = prev;
      //pre 和 head 向前移一位
      prev = head;
      head = temp;
    }
    return prev;
  }
}
```



### 2、递归法

思路在注解上，看图理解，有点绕。😂😂😂

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcedsdi9oxj30pg0543yy.jpg" alt="newname2020-03-0113.51.24" style="zoom:50%;" />

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
    //如果 head == null，说明没有需要反转的链表；
    //如果 head.next == null，说明链表到达尾结点，此时尾结点就是反转后链表的头结点。
    if(head == null || head.next == null){
      return head; 
    }
    //newHead 就是反转后链表的头结点
    ListNode newHead = reverseList(head.next);
    /*
    		看下面图理解，当递归到最后一次，终止递归后返回了 head.next，即下图 5 ，
    		此时 newHead = head.next 是反转后链表的头结点，而当前 head 是 4，所以 head.next.next
    		指向了 head 替代了原本指向的 null，即 5->null 变为了 5->4，完成了链表的反转，
    		再使 head.next = null，就是让 4->5 断开，防止链表循环导致死循环。
    */
    head.next.next = head;
    head.next = null;
    return newHead;
  }
}
```

![newname2020-03-0115.05.46](https://tva1.sinaimg.cn/large/00831rSTly1gcefxnh9xnj31mn0u0472.jpg)



