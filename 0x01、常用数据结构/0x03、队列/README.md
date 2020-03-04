## 1、什么是队列

队列是一种受限线性表，它只允许在一端进行插入操作，在另一端进行删除操作，具有**先进先出（First In First Out，FIFO）**的特性。其中，插入删除的时间复杂度是 O(1)，查询的时间复杂度是 O(n)。

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcfspzb44uj31640cqdj8.jpg" alt="newname2020-03-0219.13.26" style="zoom: 40%;" />

----

## 2、API

> 队列有两套 API，主要区别是对错误处理方式不同

|          | Throws exception | Returns special value |
| -------- | ---------------- | --------------------- |
| **添加** | `add(e)`         | `offer(e)`            |
| **删除** | `remove()`       | `poll()`              |
| **查看** | `element()`      | `peek()`              |

----

## 3、队列的存储形式

### 3.1、顺序队列

假设队列长度为 n，队头为 head，队尾为 tail，此时判断队列空的条件是：head == tail，而判断队列满的条件为：tail == n。在不断出队入队的过程中，head 和 tail 不断后移，由于队列先进先出，当 tail == n 时，将无法再添加元素，但是此时队头之前仍有空位，此时就发生了「假溢出」。

为了解决「假溢出」的现象，我们将队列的头尾相连，当尾部满了之后，再从头开始。我们把队列头尾相连的存储结构称为**循环队列**。此时判断队列空的条件依然是：head == tail，而判断队列满的条件就变为：( tail+1 )%n=head​，数组长度通用计算公式为：( tail-head+n )%n​。<font color=orange>但是这种情况有一个数组单元就被闲置，也会面临数组溢出的情况。</font>

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcftvpsydbj30km0nggod.jpg" alt="newname2020-03-0219.53.39" style="zoom:29%;" />

```java
//循环队列的实现
public class CircularQueue {
  // 数组：items，数组大小：n
  private String[] items;
  private int n = 0;
  // head表示队头下标，tail表示队尾下标
  private int head = 0;
  private int tail = 0;

  // 申请一个大小为capacity的数组
  public CircularQueue(int capacity) {
    items = new String[capacity];
    n = capacity;
  }

  // 入队
  public boolean offer(String item) {
    // 队列满了
    if ((tail + 1) % n == head) return false;
    items[tail] = item;
    tail = (tail + 1) % n;
    return true;
  }

  // 出队
  public String poll() {
    // 如果head == tail 表示队列为空
    if (head == tail) return null;
    String ret = items[head];
    head = (head + 1) % n;
    return ret;
  }
	//打印
  public void printAll() {
    if (0 == n) return;
    for (int i = head; i % n != tail; i = (i + 1) % n) {
      System.out.print(items[i] + " ");
    }
    System.out.println();
  }
}
```



### 3.2、链式队列

队列的链式存储结构，其实就是线性表的单链表，只不过它只能尾进头出而已， 我们把它简称为链队列。

```java
class Node {
  private String data;
  private Node next;

  public Node(String data, Node next) {
    this.data = data;
    this.next = next;
  }

  public String getData() {
    return data;
  }
}

//基于链表实现的队列
public class QueueBasedOnLinkedList {
  // 队列的队首和队尾
  private Node head = null;
  private Node tail = null;

  // 入队
  public void offer(String value) {
    if (tail == null) {
      Node newNode = new Node(value, null);
      head = newNode;
      tail = newNode;
    } else {
      tail.next = new Node(value, null);
      tail = tail.next;
    }
  }

  // 出队
  public String poll() {
    if (head == null) return null;
    String value = head.data;
    head = head.next;
    if (head == null) {
      tail = null;
    }
    return value;
  }
	//打印
  public void printAll() {
    Node p = head;
    while (p != null) {
      System.out.print(p.data + " ");
      p = p.next;
    }
    System.out.println();
  }
}
```



----

## 4、阻塞队列和并发队列

**阻塞队列**其实就是在队列基础上增加了阻塞操作。简单来说，就是在队列为空的时候，从队头取数据会被阻塞。因为此时还没有数据可取，直到队列中有了数据才能返回；如果队列已经满了，那么插入数据的操作就会被阻塞，直到队列中有空闲位置后再插入数据，然后再返回。

线程安全的队列我们叫作**并发队列**。最简单直接的实现方式是直接在 offer()、dequeue() 方法上加锁，但是锁粒度大并发度会比较低，同一时刻仅允许一个存或者取操作。实际上，基于数组的循环队列，利用 CAS 原子操作，可以实现非常高效的并发队列。这也是循环队列比链式队列应用更加广泛的原因。