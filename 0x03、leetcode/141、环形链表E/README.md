## 题目描述：

给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

**示例 1：**

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

![img](https://tva1.sinaimg.cn/large/00831rSTly1gcejo1m17wj30er04r0sr.jpg)

**示例 2：**

```
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```

![img](https://tva1.sinaimg.cn/large/00831rSTly1gcejoni9ivj305l02xt8j.jpg)

**示例 3：**

```
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

![img](https://tva1.sinaimg.cn/large/00831rSTly1gcejp1pygej301t01twe9.jpg)

----

## 解题思路：

### 1、双指针法：

设置两个指针，慢指针（slow）每次走一步，快指针（fast）每次走两步，进行迭代，

- 当快指针走到链表尾部时，则说明没有环。
- 如果有环的话，在第 n 圈这两个指针肯定会相遇。

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
  public boolean hasCycle(ListNode head) {
    ListNode fast = head;
    ListNode slow = head;
    while(fast != null && fast.next != null){
      //快慢指针
      fast = fast.next.next;
      slow = slow.next;
      //判断是否相遇
      if(fast == slow) {
        return true;
      }
    }
    return false;
  }
}
```

<img src="/Users/moby/Library/Application%20Support/typora-user-images/newname2020-03-0117.17.23.png" alt="newname2020-03-0117.17.23" style="zoom:50%;" />

### 2、Set 集合

set 集合的特点是无序，不重复。每遍历一个结点，将结点放到 set 中，如果访问到第二次时，则能判断链中有环。

```java
/**
   * Definition for singly-linked list.
   * class ListNode {
   *     int val;
   *     ListNode next;
   *     ListNode(int x) {
   *         val = x;
   *         next = null;
   *     }
   * }
   */
public class Solution {
  public boolean hasCycle(ListNode head) {
    HashSet<ListNode> set =new HashSet<>();
    while(head!=null){
      if(set.contains(head)){
        return true;
      }
      set.add(head);
      head=head.next;
    }
    return false;
  }
}
```

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcematvkm5j30l00543ym.jpg" alt="newname2020-03-0118.45.22" style="zoom:50%;" />