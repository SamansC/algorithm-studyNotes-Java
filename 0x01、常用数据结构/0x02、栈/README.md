## 1、什么是栈

栈是一种受限线性表，限定仅在表尾（栈顶）进行插入和删除操作，是一种特殊的线性表，具有**后进先出（Last In First Out，LIFO）**的特性。其中，插入删除的时间复杂度是 O(1)，查询的时间复杂度是 O(n)。

我们把允许插入删除的一端叫做栈顶（top），另一端叫做栈底（bottom），不含任何元素叫做空栈。

栈的插入操作叫做入栈（压栈），栈的删除操作叫做出栈（弹栈）。

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcfmucmptdj30vi0u079g.jpg" alt="newname2020-03-0215.50.22" style="zoom:35%;" />

----

## 2、API

| Modifier and Type | Method and Description                                       |
| :---------------- | :----------------------------------------------------------- |
| `boolean`         | `empty()`：判断栈是否为空。                                  |
| `E`               | `peek()`：查看并返回栈顶元素，不删除栈顶元素。               |
| `E`               | `pop()`：删除并返回栈顶元素。                                |
| `E`               | `push(E item)`：向栈顶插入数据。                             |
| `int`             | `search(Object o)`：查找元素，返回元素在栈中第一次出现的位置，位置从1算起（非0）。-1表示栈中不存在此元素。 |

----

## 3、栈的存储结构

### 3.1、顺序栈

用数组实现的栈叫做顺序栈。栈底下标为 0，定义为变量 top 来表示栈顶元素所在位置 。

空栈的的判定条件为：top = -1。

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcfnlpa3d4j31a40k4aed.jpg" alt="newname2020-03-0216.16.30" style="zoom:33%;" />

```java
// 基于数组实现的顺序栈
public class ArrayStack {
  private String[] items;  // 数组
  private int count;       // 栈中元素个数
  private int n;           //栈的大小

  // 初始化数组，申请一个大小为n的数组空间
  public ArrayStack(int n) {
    this.items = new String[n];
    this.n = n;
    this.count = 0;
  }

  // 入栈操作
  public boolean push(String item) {
    // 数组空间不够了，直接返回false，入栈失败。
    if (count == n) return false;
    // 将item放到下标为count的位置，并且count加一
    items[count] = item;
    ++count;
    return true;
  }
  
  // 出栈操作
  public String pop() {
    // 栈为空，则直接返回null
    if (count == 0) return null;
    // 返回下标为count-1的数组元素，并且栈中元素个数count减一
    String tmp = items[count-1];
    --count;
    return tmp;
  }
}
```



### 3.2、链栈

用链表实现的栈叫做链栈。栈顶放在单链表的头部，通常对于链栈来说，是不需要头结点的，而且链栈一般不存在栈满的情况。当栈为空时，就是 top = null 时。

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcfovwu5pij30mg0kcmzh.jpg" alt="newname2020-03-0217.01.02" style="zoom:40%;" />

```java
class Node {
  private int data;
  private Node next;

  public Node(int data, Node next) {
    this.data = data;
    this.next = next;
  }

  public int getData() {
    return data;
  }
}

//基于链表实现的栈。
public class StackBasedOnLinkedList {
  private Node top = null;

  public void push(int value) {
    Node newNode = new Node(value, null);
    // 判断是否栈空
    if (top == null) {
      top = newNode;
    } else {
      newNode.next = top;
      top = newNode;
    }
  }

  /**
   * 我用-1表示栈中没有数据。
   */
  public int pop() {
    if (top == null) return -1;
    int value = top.data;
    top = top.next;
    return value;
  }

  public void printAll() {
    Node p = top;
    while (p != null) {
      System.out.print(p.data + " ");
      p = p.next;
    }
    System.out.println();
  }
}
```

**总结：**

如果栈的使用过程中元素变化不可预料，那么最好是用链栈。反之，如果它的变化在可控范围内，建议使用顺序栈会更好一些。

----

## 4、栈的应用

1. **递归：**

	> 把一个直接调用自己或通过一系列的调用语句间接地调用自己的函数，称做递归函数。经典例子就是斐波那契数列的实现。

	

2. **四则运算表达式**

	> 使用后缀（逆波兰）表示法。

	

3. **浏览器的前进后退**

	> 我们使用两个栈X和Y，我们把首次浏览的页面依次压如栈X，当点击后退按钮时，再依次从栈X中出栈，并将出栈的数据一次放入Y栈。当点击前进按钮时，我们依次从栈Y中取出数据，放入栈X中。当栈X中没有数据时，说明没有页面可以继续后退浏览了。当Y栈没有数据，那就说明没有页面可以点击前进浏览了。

	

4. **栈在括号匹配中的应用**

	> 用栈保存为匹配的左括号，从左到右一次扫描字符串，当扫描到左括号时，则将其压入栈中；当扫描到右括号时，从栈顶取出一个左括号，如果能匹配上，则继续扫描剩下的字符串。如果扫描过程中，遇到不能配对的右括号，或者栈中没有数据，则说明为非法格式。
	> 当所有的括号都扫描完成之后，如果栈为空，则说明字符串为合法格式；否则，说明未匹配的左括号为非法格式。

----

## 5、栈的作用

简化了程序设计的问题，划分了不同层次，使思考范围缩小，更加聚焦于我们需要解决的核心问题。不用考虑数组下标等的细节问题。

----

## 6、为什么函数调用要用“栈”来保存临时变量呢？用其他数据结构不行吗？

其实，我们不一定非要用栈来保存临时变量，只不过如果这个函数调用符合后进先出的特性，用栈这种数据结构来实现，是最顺理成章的选择。

从调用函数进入被调用函数，对于数据来说，变化的是什么呢？是作用域。所以根本上，只要能保证每进入一个新的函数，都是一个新的作用域就可以。而要实现这个，用栈就非常方便。在进入被调用函数的时候，分配一段栈空间给这个函数的变量，在函数结束的时候，将栈顶复位，正好回到调用函数的作用域内。