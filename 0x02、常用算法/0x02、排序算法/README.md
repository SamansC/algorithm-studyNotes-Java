<img src="https://tva1.sinaimg.cn/large/00831rSTly1gci8novxnij315m0u0tfr.jpg" alt="排序算法" style="zoom:80%;" />

> 本文动图来源：[十大经典排序算法算法](https://www.cnblogs.com/onepixel/p/7674659.html)

## 1、排序算法

| 常用算法                                                     | 时间复杂度 |
| ------------------------------------------------------------ | ---------- |
| [冒泡排序](#3、冒泡排序)、[插入排序](#4、插入排序)、[选择排序](#5、选择排序) | O(n^2^)    |
| [归并排序](#6、归并排序)、[快速排序](#7、快速排序)           | O(nlogn)   |
| [桶排序](#8、桶排序)、[计数排序](#9、计数排序)、[基数排序](#10、基数排序) | O(n)       |

其中<font color=orange>冒泡排序、插入排序、选择排序、归并排序、快速排序</font>都是基于比较的排序算法。

----

## 2、如何分析一个排序算法？

**1）排序算法的执行效率**

- 最好情况、最坏情况，平均时间复杂度
- 时间复杂度的常数，系数，低阶。（数据量小的情况下）
- 比较次数和交换次数。

**2）排序算法的内存消耗**

**3）排序算法的稳定性**

> 稳定性指的是：如果待排序的序列中存在值相等的元素，经过排序后相等元素之间原有的先后顺序不变。

----

## 3、冒泡排序

冒泡排序（Bubble Sort）的最好情况下数组是有序的，只需要遍历一次即可，即时间复杂度是 O(n)。最坏情况下数组正好是倒序的，即时间复杂度事 O(n^2^)，平均时间复杂度是 O(n^2^)。

**实现步骤：**

1. 比较相邻的两个元素，如果第一个值大于第二个的值，则把它俩交换。
2. 对每一对元素都进行比较，从头到尾，这样每次最后的元素都是当前循环最大的元素。
3. 重复上述步骤，并且每次比较次数 -1。
4. 设置一个标志，当没有元素交换时，说明当前数组已经排序完成，直接跳出循环。

**代码实现：**

```java
public int[] bubbleSort(int[] array){
  int len = array.length;
  if(len <= 1) return array;
  //循环次数，最多只需 len-1 次
  for(int i = 0;i<len-1;++i){
    //设置标志，当没有元素交换时，直接跳出循环
    boolean flag = false;
    //交换，交换次数最多只需 len-i-1 次
    for(int j = 0;j<len-i-1;j++){
      if(array[j] > array[j+1]){
        int temp = array[j+1];
        array[j+1]=array[j];
        array[j]=temp;
        flag=true;  //有数据交换
      }
    }
    if(!flag) break;//没有数据交换，退出
  }
  return array;
}
```

**动图演示：**

![img](https://tva1.sinaimg.cn/large/00831rSTly1gci98191yyg30my075wqv.gif)

[返回顶部](#1、排序算法)

----

## 4、插入排序

插入排序（Insertion Sort）最好是时间复杂度为 O(n)，最坏情况时间复杂度为 O(n^2^)，平均时间复杂度为 O(n^2^)。其核心思想是<font color=orange>取未排序区间中的元素，在已排序区间中找到合适的插入位置将其插入，并保证已排序区间数据一直有序。重复这个过程，直到未排序区间中元素为空，算法结束。</font>

**实现步骤：**

1. 从第一个元素开始，该元素可以认为已经被排序；
2. 取出下一个元素，在已经排序的元素序列中从后向前扫描；
3. 如果该元素（已排序）大于新元素，将该元素移到下一位置；
4. 重复步骤3，直到找到已排序的元素小于或者等于新元素的位置；
5. 将新元素插入到该位置后；
6. 重复步骤2~5。

**代码实现：**

```java
public int[] insertionSort(int[] array){
  int len = array.length;
  if(len <= 0) return array;
  int current = 0;
  int preIndex = 0;
  for(int i = 0;i<len-1;++i){
    //将当前元素的下一个元素保存到 current 中
    current = array[i+1];
    //每次从已排序区间的尾部向头部遍历
    preIndex = i;
    //寻找插入点，当 preIndex>=0 且当前元素的下一元素小于当前元素时，继续循环。
    while(preIndex >=0 && current < array[preIndex]){
      array[preIndex + 1] = array[preIndex]; //数据移动
      preIndex--;
    }
    array[preIndex + 1] = current; //插入数据
  }
  return array;
}
```

**动画演示：**

![img](https://tva1.sinaimg.cn/large/00831rSTly1gci95fmlgbg30mj0e113f.gif)

[返回顶部](#1、排序算法)

----

## 5、选择排序

选择排序（Selection Sort）首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。 **最佳情况：T(n) = O(n2)  最差情况：T(n) = O(n2)  平均情况：T(n) = O(n2)**

**实现步骤：**

1. 初始状态：无序区为R[1..n]，有序区为空；
2. 第 i 趟排序 (i=1,2,3…n-1) 开始时，当前有序区和无序区分别为 R[1..i-1] 和 R(i..n）。该趟排序从当前无序区中-选出关键字最小的记录 R[k]，将它与无序区的第1个记录R交换，使R[1..i]和R[i+1..n)分别变为记录个数增加1个的新有序区和记录个数减少1个的新无序区；
3. n-1趟结束，数组有序化了。

**代码实现：**

```java
public int[] selectionSort(int[] array){
  int len = array.length;
  if(len <= 1) return array;
  int minIndex = 0;
  for(int i = 0;i<len-1;++i){
    minIndex = i;
    for(int j = i;j<len;++j){
      //找到最小值
      if(array[i]<array[minIndex]){
        //保存最小值下标
        minIndex = j;
      }
    }
    //交换数据
    int temp = array[minIndex];
    array[minIndex] = array[i];
    array[i] = temp;
  }
  return array;
}
```

**动画实现：**

![img](https://tva1.sinaimg.cn/large/00831rSTly1gciarp64ljg30mj06w7l2.gif)

[返回顶部](#1、排序算法)

----

## 6、归并排序

归并排序（Merge Sort）用到的是**分治思想**（Divide and Conquer：分而治之，将一个大问题分解成子问题解决），**归并排序不受输入数据得影响，其时间复杂度为 O(nlogn)。**

其实现方式就是不断把序列一分为二，再把排好序子序列进行合并，得到一个完全有序的序列。

**实现步骤：**

1. 把长度为 n 的输入序列分成两个长度为 n/2 的子序列；
2. 对这两个子序列分别采用归并排序；
3. 将两个排序好的子序列合并成一个最终的排序序列。

**实现代码：**

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcmyftd3tsj30tu0qs7iu.jpg" alt="newname2020-03-0823.50.29" style="zoom:33%;" />

==版本一==：无返回值

```java
public class MergeSort {
  public static void mergeSort(int[] array, int left, int right) {
    //终止条件
    if (right <= left) return;
    //位运算，相当于除2，速度更快。
    int mid = (left + right) >> 1;
    //其实就是不断将数组一分为二，再把左右两个排好序子数组合并成一个新数组。
    mergeSort(array, left, mid);
    mergeSort(array, mid + 1, right);
    //合并
    merge(array, left, mid, right);
  }

  private static void merge(int[] array, int left, int mid, int right) {
    //中间数组长度
    int[] temp = new int[right - left + 1];
    // i 代表第一个数组的起始位置，j 代表第二个数组的起始位置。
    // k 代表 temp 数组已经填入的元素数量
    int i = left, j = mid + 1, k = 0;
    // i 且 j 两个数组都没循环完
    while (i <= mid && j <= right) {
      //把 i 和 j 中数组较小的元素添加到 temp 中，并自增。
      temp[k++] = array[i] <= array[j] ? array[i++] : array[j++];
    }
    //如果 i 或 j 没有循环完，就把 i 或 j 之后的元素添加到 temp 之后
    while (i <= mid) temp[k++] = array[i++];
    while (j <= right) temp[k++] = array[j++];
    if (temp.length >= 0) System.arraycopy(temp, 0, array, left, temp.length);
  }
  //测试数据
  public static void main(String[] args) {
    int[] array = new int[] {2, 5, 2, 6, 61, 31, 44, 23};
    //调用(数组,首下标,尾下标)
    MergeSort.mergeSort(array, 0, array.length-1);
    System.out.println(Arrays.toString(array));
  }
}
```

==版本二==：带返回值

```java
public class MergeSort {
  public static int[] mergeSort(int[] array) {
    //终止条件
    if (array.length < 2) return array;
    int mid = array.length >> 1;
    //其实就是不断将数组一分为二，再把左右两个排好序子数组合并成一个新数组。
    //注意数组下标越界
    int[] left = mergeSort(Arrays.copyOfRange(array, 0, mid));
    int[] right = mergeSort(Arrays.copyOfRange(array, mid, array.length));
    return merge(left, right); //合并
  }

  private static int[] merge(int[] left, int[] right) {
    //中间数组长度
    int[] temp = new int[left.length + right.length];
    //定义各个数组的首元素下标
    int tempIndex = 0, leftIndex = 0, rightIndex = 0;
    //在条件内，将小的添加到 temp 中
    while (leftIndex < left.length && rightIndex < right.length) {
      temp[tempIndex++] = left[leftIndex] <= right[rightIndex] ? left[leftIndex++] : right[rightIndex++];
    }
    //如果有一个数组没有循环完，就把该数组之后的元素添加到 temp 之后
    while (leftIndex < left.length) temp[tempIndex++] = left[leftIndex++];
    while (rightIndex < right.length) temp[tempIndex++] = right[rightIndex++];
    return temp;
  }
	//测试数据
  public static void main(String[] args) {
    int[] array = new int[] {2, 5, 2, 6, 61, 31, 2516, 23};
    array = MergeSort.mergeSort(array);
    System.out.println(Arrays.toString(array));
  }
}
```

**动画实现：**

![img](https://tva1.sinaimg.cn/large/00831rSTly1gcmtu785i5g30mj0e1qcv.gif)

[返回顶部](#1、排序算法)

----

## 7、快速排序

快速排序（Quick Sort）在序列中取一个基准，按照升序排序，将小于基准的元素放到基准左边（左序列）；大于基准的的元素放到基准的右边（右序列）。然后一次对左右序列进行快排，一直到序列达到有序。

**实现步骤：**

1. 从数列中挑出一个元素，称为**「基准」（pivot）**；
2. 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为**分区（partition）**操作；
3. 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。

**实现代码：**

```java
public class QuickSort {
  //无返回值
  public static void quickSort(int[] array, int begin, int end) {
    //终止条件
    if (begin >= end) return;
    //分区，返回「标杆」位置
    int pivot = partition(array, begin, end);
    //排序
    quickSort(array, begin, pivot - 1);
    quickSort(array, pivot + 1, end);
  }
  //有返回值
  public static int[] quickSort(int[] array) {
    //终止条件
    if (array.length < 2) return array;
    //分区，返回「标杆」位置，注意数组下标越界。
    int pivot = partition(array, 0, array.length-1);
    //排序，注意数组下标越界
    quickSort(array, 0, pivot - 1);
    quickSort(array, pivot + 1, array.length-1);
    return array;
  }

  private static int partition(int[] array, int begin, int end) {
    //pivot「标杆」位置，counter 小于 a[pivot] 的数量
    int pivot = end, counter = begin;
    for (int i = begin; i < end; i++) {
      if (array[i] < array[pivot]) {
        //交换元素位置，把小的放到 pivot 前
        int temp = array[counter];
        array[counter] = array[i];
        array[i] = temp;
        counter++;
      }
    }
    //交换 counter 和 pivot 的位置，使 pivot 位于左右两个子数组中间
    int temp = array[pivot];
    array[pivot] = array[counter];
    array[counter] = temp;
    return counter;
  }
  //测试数据
  public static void main(String[] args) {
    int[] array = new int[] {2, 25, 2, 8, 21, 6, 11, 7};
    QuickSort.quickSort(array, 0, array.length - 1);
    System.out.println(Arrays.toString(array));
    
    int[] array1 = QuickSort.quickSort(array);
    System.out.println(Arrays.toString(array1));
  }
}
```

**动画实现：**

![img](https://tva1.sinaimg.cn/large/00831rSTly1gcmxzn6729g30mj0707d3.gif)

<font color=orange>总结快速排序和归并排序</font>

1. 步骤顺序不同：
	- 快速排序是先调配出左右子序列，再进行排序。
	- 归并排序是先排序左右子序列，再合并两个子序列。
2. 复杂度不同：
	- 快速排序是**原地排序**算法，空间复杂度为 O(1)。快速排序的执行效率与要排序的原始数组的有序程度有关，最好、平均时间复杂度都是 O(nlogn)。极端情况下，输入数组是有序的，最差时间复杂度会到 O(n^2^)。
	- 归并排序需要额外的数组，空间复杂度为 O(n)。归并排序的执行效率与要排序的原始数组的有序程度无关，所以其时间复杂度是非常稳定的，最好、最坏、平均时间复杂度都是 O(nlogn)，
3. 稳定性不同：
	- 快速排序是不稳定的。
	- 归并排序是稳定的。

[返回顶部](#1、排序算法)

----

## 8、桶排序

桶排序（Bucket Sort）它利用了函数的映射关系，高效与否的关键就在于这个映射函数的确定。

其核心思想是将要排序的数据分到几个有序的桶里，每个桶里的数据再单独进行排序。桶内排完序之后，再把每个桶里的数据按照顺序依次取出，组成的序列就是有序的了。其时间复杂度与桶的划分有关，桶划分的越小，各个桶之间的数据越少，排序所用的时间也会越少。但相应的空间消耗就会增大。 

> 最好时间复杂度是 O(n)

**应用场景：**

桶排序比较适合用在**外部排序**中。所谓的外部排序就是数据存储在外部磁盘中，数据量比较大，内存有限，无法将数据全部加载到内存中。

> 比如说我们有 10GB 的订单数据，我们希望按订单金额（假设金额都是正整数）进行排序，但是我们的内存有限，只有几百 MB，没办法一次性把 10GB 的数据都加载到内存中。这个时候该怎么办呢？现在我来讲一下，如何借助桶排序的处理思想来解决这个问题。我们可以先扫描一遍文件，看订单金额所处的数据范围。假设经过扫描之后我们得到，订单金额最小是 1 元，最大是 10 万元。我们将所有订单根据金额划分到 100 个桶里，第一个桶我们存储金额在 1 元到 1000 元之内的订单，第二桶存储金额在 1001 元到 2000 元之内的订单，以此类推。每一个桶对应一个文件，并且按照金额范围的大小顺序编号命名（00，01，02…99）。理想的情况下，如果订单金额在 1 到 10 万之间均匀分布，那订单会被均匀划分到 100 个文件中，每个小文件中存储大约 100MB 的订单数据，我们就可以将这 100 个小文件依次放到内存中，用快排来排序。等所有文件都排好序之后，我们只需要按照文件编号，从小到大依次读取每个小文件中的订单数据，并将其写入到一个文件中，那这个文件中存储的就是按照金额从小到大排序的订单数据了。不过，你可能也发现了，订单按照金额在 1 元到 10 万元之间并不一定是均匀分布的 ，所以 10GB 订单数据是无法均匀地被划分到 100 个文件中的。有可能某个金额区间的数据特别多，划分之后对应的文件就会很大，没法一次性读入内存。这又该怎么办呢？针对这些划分之后还是比较大的文件，我们可以继续划分，比如，订单金额在 1 元到 1000 元之间的比较多，我们就将这个区间继续划分为 10 个小区间，1 元到 100 元，101 元到 200 元，201 元到 300 元…901 元到 1000 元。如果划分之后，101 元到 200 元之间的订单还是太多，无法一次性读入内存，那就继续再划分，直到所有的文件都能读入内存为止。
>
> > 《数据结构与算法之美》---王争老师

**代码实现：**

```java
public class BucketSort {
  public void sort(Double[] a) {
    int n = a.length;
    //创建链表（桶）集合并初始化，集合中的链表用于存放相应的元素
    int bucketNum = 10; // 桶数
    LinkedList<LinkedList<Double>> buckets = new LinkedList<LinkedList<Double>>();
    for(int i = 0; i < bucketNum; i++){
      LinkedList<Double> bucket = new LinkedList<Double>();
      buckets.add(bucket);
    }
    // 把元素放进相应的桶中
    for(int i = 0; i < n; i++){
      int index = (int) (a[i] * bucketNum);
      buckets.get(index).add(a[i]);
    }
    // 对每个桶中的元素排序，并放进a中
    int index = 0;
    for (LinkedList<Double> linkedList : buckets) {
      int size = linkedList.size();
      if (size == 0) {
        continue;
      }
      //把LinkedList<Double>转化为Double[]的原因是，之前已经实现了对数组进行排序的算法
      Double[] temp = new Double[size];
      for (int i = 0; i < temp.length; i++) {
        temp[i] = linkedList.get(i);
      }
      // 利用插入排序对temp排序
      new InsertSort().sort(temp);
      for (int i = 0; i < temp.length; i++) {
        a[index] = temp[i];
        index++;
      }
    }
  }

  public static void main(String[] args) {
    Double[] a = new Double[]{0.3, 0.6, 0.5};
    new BucketSort().sort(a);
    for (int i = 0; i < a.length; i++) {
      System.out.println(a[i]);
    }
  }
}
```

**图片演示：**

![newname2020-03-0921.06.16](https://tva1.sinaimg.cn/large/00831rSTly1gcnzbqemu2j311q0py761.jpg)

[返回顶部](#1、排序算法)

----

## 9、计数排序

计数排序（Counting Sort）其实是桶排序的一种特殊情况。计数排序要求输入的数据必须是有确定范围的整数。将输入的数据值转化为键存储在额外开辟的数组空间中，然后依次把计数大于 1 的填充回原数组。

**算法实现：**

1. 找出待排序的数组中最大和最小的元素；
2. 统计数组中每个值为 i 的元素出现的次数，存入数组C的第 i 项；
3. 对所有的计数累加（从C中的第一个元素开始，每一项和前一项相加）；
4. 反向填充目标数组：将每个元素i放在新数组的第C(i)项，每放一个元素就将C(i)减去1。

**代码：**

```java
// 计数排序，a是数组，n是数组大小。假设数组中存储的都是非负整数。
public void countingSort(int[] a, int n) {
  if (n <= 1) return;
  // 查找数组中数据的范围
  int max = a[0];
  for (int i = 1; i < n; ++i) {
    if (max < a[i]) {
      max = a[i];
    }
  }

  int[] c = new int[max + 1]; // 申请一个计数数组c，下标大小[0,max]
  for (int i = 0; i <= max; ++i) {
    c[i] = 0;
  }

  // 计算每个元素的个数，放入c中
  for (int i = 0; i < n; ++i) {
    c[a[i]]++;
  }

  // 依次累加
  for (int i = 1; i <= max; ++i) {
    c[i] = c[i-1] + c[i];
  }

  // 临时数组r，存储排序之后的结果
  int[] r = new int[n];
  // 计算排序的关键步骤，有点难理解
  for (int i = n - 1; i >= 0; --i) {
    int index = c[a[i]]-1;
    r[index] = a[i];
    c[a[i]]--;
  }

  // 将结果拷贝给a数组
  for (int i = 0; i < n; ++i) {
    a[i] = r[i];
  }
}
```

**动画实现：**

![img](https://tva1.sinaimg.cn/large/00831rSTly1gcnz7znjyfg30s40fhdmw.gif)

[返回顶部](#1、排序算法)

----

## 10、基数排序

基数排序（Radix sort）是按照低位先排序，然后收集；再按照高位排序，然后再收集；依次类推，直到最高位。有时候有些属性是有优先级顺序的，先按低优先级排序，再按高优先级排序。

**实现步骤：**

1. 取得数组中的最大数，并取得位数；
2. arr为原始数组，从最低位开始取每个位组成radix数组；
3. 对radix进行计数排序（利用计数排序适用于小范围数的特点）；

**代码实现：**

```java
**
  * 基数排序
  * @param array
  * @return
  */
  public static int[] RadixSort(int[] array) {
  if (array == null || array.length < 2)
    return array;
  // 1.先算出最大数的位数；
  int max = array[0];
  for (int i = 1; i < array.length; i++) {
    max = Math.max(max, array[i]);
  }
  int maxDigit = 0;
  while (max != 0) {
    max /= 10;
    maxDigit++;
  }
  int mod = 10, div = 1;
  ArrayList<ArrayList<Integer>> bucketList = new ArrayList<ArrayList<Integer>>();
  for (int i = 0; i < 10; i++)
    bucketList.add(new ArrayList<Integer>());
  for (int i = 0; i < maxDigit; i++, mod *= 10, div *= 10) {
    for (int j = 0; j < array.length; j++) {
      int num = (array[j] % mod) / div;
      bucketList.get(num).add(array[j]);
    }
    int index = 0;
    for (int j = 0; j < bucketList.size(); j++) {
      for (int k = 0; k < bucketList.get(j).size(); k++)
        array[index++] = bucketList.get(j).get(k);
      bucketList.get(j).clear();
    }
  }
  return array;
}
```

**动画实现：**

![img](https://tva1.sinaimg.cn/large/00831rSTly1gcnzwrprqeg30s40fydky.gif)

[返回顶部](#1、排序算法)

----

[动态图解十大经典排序算法]: https://mp.weixin.qq.com/s/HQg3BzzQfJXcWyltsgOfCQ
[数据结构与算法之美--极客时间专栏]: https://time.geekbang.org/column/intro/126

