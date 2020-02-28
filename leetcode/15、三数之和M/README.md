## 题目描述：

给定一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

**示例：**

```
给定数组 nums = [-1, 0, 1, 2, -1, -4]，
满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

----

## 解题思路：

这道题首先想到的就是暴力法，循环套循环，可以看作两数之和的升级版，但是这种方法超时了 O(n^3^)……就不做考虑。

我认为这道题最重要的两个点就是`排序`和`去重`。

1. 判断数组是否符合要求，即数组长度大于3.
2. 对数组进行排序，这里的时间复杂度时 O(nlogn)。
3. 排序后，固定一个数 `nums[i]`，再用左右指针指向 `nums[i]` 后的两端，即 `nums[i+1]` 和 `nums[nums.length-1]`，分别定义为 `nums[j]` 和 `nums[k]`，计算三个数之和是否为0 ? 加入结果集 : 跳过。
4. 去重：
	- 当 `nums[i]` > 0 时，则三数之和肯定大于零，此时直接结束方法。
	- 当  `nums[i]` == `nums[i+1]` 时，说明数字重复，会导致结果重复， 跳过。
	- 当 j < k 时，循环计算 `nums[j]` + `nums[k]` + `nums[i]` 之和 sum 。
		1. 当 sum > 0时，说明值过大， `k--`，
		2. 当 sum < 0时，说明值过小，`j++`
		3. 当 sum == 0 时，将结果添加到结果集中，
			- 当  `nums[j]` == `nums[j+1]` 时，会导致结果重复，应该跳过，`j++`
			- 当  `nums[k]` == `nums[k-1]` 时， 会导致结果重复，应该跳过，`k--`

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcbw7uo268j30pe05474p.jpg" alt="newname2020-02-2810.12.26" style="zoom:50%;" />

**注解版：**

```java
class Solution {
  public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> lists = new ArrayList<>();
    //判断数组是否为空且长度是否大于三
    if (nums != null && nums.length >= 3) {
      Arrays.sort(nums);
      //当 i>nums.length-2 时，不足三位，可以结束判断。
      for (int i = 0; i < nums.length-2; i++) {
        //如果当前元素大于零，则三数之和一定大于零
        if (nums[i] > 0) {
          break;
        }
        //如果当前元素与前一个元素相同，则略过。去重
        if (i > 0 && nums[i] == nums[i - 1]) {
          continue;
        }
        //定义左右双指针
        int j = i + 1;
        int k = nums.length - 1;
        while (j < k) {
          int sum = nums[i] + nums[j] + nums[k];
          if (sum > 0) {         //如果和大于零，右指针左移。
            k--;
          } else if (sum < 0) { //如果和小于零，左指针右移
            j++;
          } else {             //如果和等于零，将三个数作为数组添加到集合中
            lists.add(Arrays.asList(nums[i], nums[j], nums[k]));
            //判断当前下标数值和下一个下标数值是否相等，去重
            while (j < k && nums[j] == nums[j + 1]) {
              j++;
            }
            //判断当前下标数值和前一个下标数值是否相等，去重
            while (j < k && nums[k] == nums[k - 1]) {
              k--;
            }
            //当前数据依然是重复的数据，所以左右指针向内收一位
            j++;
            k--;
          }
        }
      }
    }
    return lists;
  }
}
```

**简化版：**

```java
class Solution {
  public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> lists = new ArrayList<>();
    if (nums != null && nums.length >= 3) {
      Arrays.sort(nums);
      for (int i = 0; i < nums.length-2; i++) {
        if (nums[i] > 0) break;
        if (i > 0 && nums[i] == nums[i - 1]) continue;
        int j = i + 1, k = nums.length - 1;
        while (j < k) {
          int sum = nums[i] + nums[j] + nums[k];
          if (sum == 0){             
            lists.add(Arrays.asList(nums[i], nums[j], nums[k]));
            while (j < k && nums[j] == nums[j + 1]) j++;
            while (j < k && nums[k] == nums[k - 1]) k--;
            j++;
            k--;
          }
          else if (sum < 0) j++;
          else if (sum > 0) k--;
        }
      }
    }
    return lists;
  }
}
```

