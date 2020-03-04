## 题目描述：

给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。

你可以假设每种输入**只会对应一个答案**。但是，你不能重复利用这个数组中同样的元素。

**示例:**

```c
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

----

## 解题思路：

**1、暴力解法：**

```java
class Solution {
  public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
      int num1 = nums[i];
      for (int j = i+1; j < nums.length; j++) {
        int num2 = nums[j];
        if (num1 + num2 == target) {
          return new int[]{i,j};
        }
      }
    }
    throw new IllegalArgumentException("No two sum solution");
  }
}
```

使用两层循环，外层循环计算当前元素与 `target` 之间的差值，内层循环寻找该差值，若找到该差值，则返回两个元素的下标时间复杂度为 O(n^2^)。

<img src="https://tva1.sinaimg.cn/large/0082zybply1gc8w2k51ktj30pi02a0su.jpg" alt="newname2020-02-2519.50.42" style="zoom:50%;" />

## 2、哈希表

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> map =new HashMap<>();
        for ( int i = 0; i<nums.length ;i++){
            if (map.containsKey(target-nums[i])) {
                return new int[] {i,map.get(target-nums[i])};
            } else {
                map.put(nums[i],i);
            }
        }
        return null;
    }
}
```

遍历数组，判断 map 集合中是否包含 target - num[i] 的值，为真则返回两个元素在 `nums` 下标，反之添加到集合之中。时间复杂度为 O(n)。

<img src="https://tva1.sinaimg.cn/large/0082zybply1gc8w24tn1hj30po0503yy.jpg" alt="newname2020-02-2519.50.18" style="zoom:50%;" />