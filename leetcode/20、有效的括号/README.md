## 题目描述：

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。

**示例 1:**

```
输入: "()"
输出: true
```

**示例 2:**

```
输入: "()[]{}"
输出: true
```

**示例 3:**

```
输入: "(]"
输出: false
```

**示例 4:**

```
输入: "([)]"
输出: false
```

**示例 5:**

```
输入: "{[]}"
输出: true
```

----

## 解题思路：

### 1、不使用 Map 

```java
class Solution {
  public boolean isValid(String s) {
    //字符串为空返回 true
    if ("".equals(s)) return true;
    //字符串长度为奇数，直接返回 false
    if ( s.length() % 2 == 1) return false;
    Stack<Character> stack =new Stack<>();
    /*
    遍历字符串，如果匹配 '{' '[' '(' 中任意一个，则向栈中添加 '}' ']' ')'，
    当遇到 '}' ']' ')' 时，如果此时栈为空或者 pop 的数据和 c 不相等，直接返回 false。
    */
    for(char c : s.toCharArray()){
      if (c == '('){
        stack.push(')');
      } else if (c == '[') {
        stack.push(']');
      }else if (c == '{'){
        stack.push('}');
      }else if (stack.empty() || c != stack.pop()){
        return false;
      }
    }
    //可能最后会剩下 '{' '[' '(' 中任意一个，所以要判断栈是否为空
    return stack.empty();
  }
}

```

![newname2020-03-0222.41.11](https://tva1.sinaimg.cn/large/00831rSTly1gcfyq42ojzj30i402kwed.jpg)

## 2、使用 Map

```java
class Solution {
  public boolean isValid(String s) {
    //字符串为空返回 true
    if ("".equals(s)) return true;
    //字符串长度为奇数，直接返回 false
    if ( s.length() % 2 == 1) return false;
    //创建 map 存储括号的匹配项
    Map<Character, Character> map = new HashMap<>();
    map.put(')', '(');
    map.put('}', '{');
    map.put(']', '[');
    Stack<Character> stack = new Stack<>();
    for (char c : s.toCharArray()) {
      //如果是 '{' '[' '(' 中任意一个，就加入到栈中
      if (c == '(' || c == '[' || c == '{') {
        stack.push(c);
        //如果遇到 '}' ']' ')' 时，如果栈为空或 pop 出来的值和 c 中 map 匹配的值不相等，返回 fasle。
      } else if (stack.empty() || stack.pop() != map.get(c)) {
        return false;
      }
    }
    //可能最后会剩下 '{' '[' '(' 中任意一个，所以要判断栈是否为空
    return stack.empty();
  }
}
```

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcfypydvb4j30ha04uaa4.jpg" alt="newname2020-03-0222.41.02" style="zoom:50%;" />