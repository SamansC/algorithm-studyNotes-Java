## 题目描述：

使用队列实现栈的下列操作：

- push(x) -- 元素 x 入栈
- pop() -- 移除栈顶元素
- top() -- 获取栈顶元素
- empty() -- 返回栈是否为空

注意：

- 你只能使用队列的基本操作—> 也就是 `push to back`, `peek/pop from front,` `size`, 和 `is empty` 这些操作是合法的。
- 你所使用的语言也许不支持队列。 你可以使用 list 或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。
- 你可以假设所有操作都是有效的（例如, 对一个空的栈不会调用 pop 或者 top 操作）。

----

## 解题思路：

队列是先进先出（FIFO），而栈是先进后出（LIFO）。所以用队列实现栈的第一个思路就是使用[两个队列](#2、双队列)来模拟（不推荐），但是我们也可以使用[单队列](#1、单队列)来模拟：

### 1、单队列

可以使用队列的基本操作的话，主要的问题点就是在向栈添加数据时，如何把刚添加的元素移到首位？

由于栈是先进后出，所以我们的思路就是在添加数据后，满足栈内元素大于 1 时，将添加的数据移到首位，即栈顶元素就是刚添加的元素。

```java
class MyStack {
  private Queue<Integer> queue ;
  
  //利用构造函数初始化
  public MyStack() {
    queue = new LinkedList<>();
  }

  //添加一个元素
  public void push(int x) {
    queue.add(x);
    int size = queue.size();
    while(size>1){
      queue.add(queue.remove());
      size--;
    }
  }

  //删除并返回第一个元素
  public int pop() {
    //判断栈是否为空
    if(queue.isEmpty()){
      throw new RuntimeException("stack is empty");
    }
    return queue.remove();
  }

  //得到栈顶元素
  public int top() {
    //判断栈是否为空
    if(queue.isEmpty()){
      throw new RuntimeException("stack is empty");
    }
    return queue.peek();
  }

  //判断栈是否为空
  public boolean empty() {
    return queue.isEmpty();
  }
}

/**
    * Your MyStack object will be instantiated and called as such:
    * MyStack obj = new MyStack();
    * obj.push(x);
    * int param_2 = obj.pop();
    * int param_3 = obj.top();
    * boolean param_4 = obj.empty();
    */
```

### 2、双队列

我们需要两个队列 q1 和 q2 来模拟栈的操作，并定义一个变量 topNum 来记录栈顶元素。

```java
class MyStack {
  private Queue<Integer> q1;
  private Queue<Integer> q2;
  //记录栈顶元素
  private int top;

  //利用构造函数初始化
  public MyStack() {
    q1 = new LinkedList<>();
    q2 = new LinkedList<>();
  }

  //添加一个元素
  public void push(int x) {
    q1.add(x);
    top = x;
  }

  //删除并返回第一个元素
  public int pop() {
    //判断栈是否为空
    if(q1.isEmpty()){
      throw new RuntimeException("stack is empty");
    }
    while (q1.size() > 1) {
      top = q1.remove();
      q2.add(top);
    }
    int popNum = q1.remove();
    Queue<Integer> temp = q1;
    q1 = q2;
    q2 = temp;
    return popNum;
  }

  //得到栈顶元素
  public int top() {
    //判断栈是否为空
    if(q1.isEmpty()){
      throw new RuntimeException("stack is empty");
    }
    return top;
  }

  //判断栈是否为空
  public boolean empty() {
    return q1.isEmpty();
  }
}

/**
* Your MyStack object will be instantiated and called as such:
* MyStack obj = new MyStack();
* obj.push(x);
 int param_2 = obj.pop();
* int param_3 = obj.top();
* boolean param_4 = obj.empty();
*/
```

