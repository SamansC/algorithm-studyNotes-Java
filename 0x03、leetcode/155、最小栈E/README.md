## 题目描述：

设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。

- push(x) -- 将元素 x 推入栈中。
- pop() -- 删除栈顶的元素。
- top() -- 获取栈顶元素。
- getMin() -- 检索栈中的最小元素。

**示例:**

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

----

## 解题思路：

### 1、同步栈：

**优点**：不用考虑边界情况。

**缺点**：会有垃圾数据。

**时间复杂度**：O(1)

```java
class MinStack {
  private Stack<Integer> data; //数据栈
  private Stack<Integer> helper; //辅助栈
	
  //初始化
  public MinStack() {
    data = new Stack<>();
    helper = new Stack<>();
  }

  public void push(int x) {
    data.add(x);
    //当辅助栈为空或者辅助栈的栈顶元素大于等于 x，添加 x
    if (helper.isEmpty() || helper.peek() >= x) {
      helper.add(x);
    } else {
      //反之，将辅助栈的第一个元素重新添加到辅助栈中
      //这样保证辅助栈和数据栈的长度相同，并保持辅助栈的第一个元素是最小值
      helper.add(helper.peek());
    }
  }

  public void pop() {
    //两个栈都需要 pop，这里不用担心辅助栈会把最小值 pop 出去。
    //可以看先下面图片理解，辅助栈每次都添加最小的元素，所以只有数据栈删掉最小的元素，辅助栈才会删掉最小元素。
    if (!data.isEmpty()) {
      data.pop();
      helper.pop();
    }
  }

  public int top() {
    if (!data.isEmpty()) {
      return data.peek();
    }
    throw new RuntimeException("栈为空");
  }

  public int getMin() {
    if (!helper.isEmpty()) {
      return helper.peek();
    }
    throw new RuntimeException("栈为空");
  }
}
```

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcg04hbdmuj30ps0lwtbl.jpg" alt="newname2020-03-0223.29.46" style="zoom:33%;" />

### 2、非同步栈

**优点**：辅助栈每次只会存储更小的数据。

**缺点**：需要考虑栈的边界问题。

**时间复杂度**：O(1)

```java
class MinStack {
  private Stack<Integer> data;
  private Stack<Integer> helper;
	//初始化
  public MinStack() {
    data = new Stack<>();
    helper = new Stack<>();
  }

  public void push(int x) {
    data.add(x);
    //辅助栈为空时或者辅助栈的栈顶元素大于等于 x 时，需要添加到辅助栈中。
    if (helper.empty() || helper.peek() >= x) {
      helper.add(x);
    }
  }

  public void pop() {
    if (!data.isEmpty()) {
      Integer temp = data.pop();
      //如果数据栈 pop 出来的元素和辅助栈的栈顶元素相等，把辅助栈中的栈顶元素 pop 掉。
      if (temp.equals(helper.peek())) {
        helper.pop();
      }
    }
  }

  public int top() {
    if (!data.isEmpty()) {
      return data.peek();
    }
    throw new RuntimeException("栈为空");
  }

  public int getMin() {
    if (!helper.isEmpty()) {
      return helper.peek();
    }
    throw new RuntimeException("栈为空");
  }
}
```

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcg0o230h8j30hy04w3yu.jpg" alt="newname2020-03-0223.48.42" style="zoom: 50%;" />