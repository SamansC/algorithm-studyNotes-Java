## 题目描述：

给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2。

<img src="/Users/moby/Library/Application%20Support/typora-user-images/newname2020-02-2521.46.31.png" alt="newname2020-02-2521.46.31" style="zoom:40%;" />

**示例:**

```
输入: [1,8,6,2,5,4,8,3,7]
输出: 49
```

----

## 解题思路：

**1、穷举法**

```java
class Solution {
  public int maxArea(int[] height) {
    int max = 0;
    for(int i = 0;i<height.length-1;++i){
      for(int j = i+1;j<height.length;++j){
        int area = Math.min(height[i],height[j])*(j-i);
        max = Math.max(max,area);
      }
    }
    return max;
  }
}
```

设置一个面积最大值，两层循环把所有情况都遍历出来，通过 Math.max() 来求出最大值面积。时间复杂度 O(n^2^)

<img src="/Users/moby/Library/Application%20Support/typora-user-images/newname2020-02-2521.50.39.png" alt="newname2020-02-2521.50.39" style="zoom: 50%;" />

**2、左右夹逼**

```java
class Solution {
  public int maxArea(int[] height) {
    int max = 0;
    for(int i=0,j=height.length-1;i<j;){
      int minHeight = height[i] < height[j] ? height[i++] : height[j--];
      max=Math.max(max,(j-i+1)*minHeight); //先进行了 i++ 或者 j-- 操作，所以要 +1 补 x 轴长度
    }
    return max;
  }    
}
```

设置左右边界  i、j ，放到数组两端，短的一方向内移动，计算出面积保存到 max 中。时间复杂度为 O (n)

<img src="/Users/moby/Library/Application%20Support/typora-user-images/newname2020-02-2522.05.12.png" alt="newname2020-02-2522.05.12" style="zoom:50%;" />