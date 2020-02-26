## 题目描述：

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

**示例:**

```c
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```


**说明:**

1. 必须在原数组上操作，不能拷贝额外的数组。
2. 尽量减少操作次数。

----

## 解题思路：

**1、快慢下标指针**

```java
class Solution {
  public void moveZeroes(int[] nums) {
    int i = 0;
    for (int j = 0; j < nums.length; ++j) {
      if (nums[j] != 0) {
        nums[i] = nums[j];
        if (j != i) {
          nums[j] = 0;
        }
        i++;
      }
    }
  }
```

先设置一个计数器。遍历数组，如数组中的值不为 0，则把当前值赋值给下标为 i 的元素，判断 i 和 j 是否相等。如果不相等，下标为 j 的数组元素赋值为 0，计数器加 1；反之，跳过此 if 判断，计数器加 1。时间复杂度为 O(n)。

<img src="https://tva1.sinaimg.cn/large/0082zybply1gc8yic70owj30qe02s0sv.jpg" alt="newname2020-02-2521.15.04" style="zoom:50%;" />

**2、先前移非零数，后补零**

```java
class Solution {
  public void moveZeroes(int[] nums) {
    int count = 0;
    for (int i = 0;i<nums.length;++i) {
      if (nums[i]!=0) {
        nums[count] = nums[i];
        count++;
      }
    }
    while(count<nums.length) {
      nums[count++] = 0;
    }
  }
}
```

设置一个计数器。遍历数组，判断元素是否为 0 ，不为零则交换，计数器加1。遍历结束后，判断计数器是否小于数组长度，小于则补零。时间复杂度为 O(n)

<img src="https://tva1.sinaimg.cn/large/0082zybply1gc8ykcds3wj30q8056jrt.jpg" alt="newname2020-02-2521.17.00" style="zoom:50%;" />