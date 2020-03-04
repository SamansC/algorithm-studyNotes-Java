<img src="https://tva1.sinaimg.cn/large/00831rSTly1gci8novxnij315m0u0tfr.jpg" alt="排序算法" style="zoom:80%;" />

> 本文动图来源：[十大经典排序算法算法](https://www.cnblogs.com/onepixel/p/7674659.html)

## 1、排序算法

| 常用算法                                                     | 时间复杂度 |
| ------------------------------------------------------------ | ---------- |
| [冒泡排序](#3、冒泡排序)、[插入排序](#4、插入排序)、[选择排序](#5、选择排序) | O(n^2^)    |
| [归并排序](#6、归并排序)、[快速排序](#7、快速排序)           | O(nlogn)   |
| [计数排序](#8、计数排序)、[基数排序](#9、基数排序)、[桶排序](#10、桶排序) | O(n)       |

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

插入排序（Insertion Sort）最好是时间复杂度为 O(n)，最坏情况时间复杂度为 O(n2)，平均时间复杂度为 O(n2)。其核心思想是<font color=orange>取未排序区间中的元素，在已排序区间中找到合适的插入位置将其插入，并保证已排序区间数据一直有序。重复这个过程，直到未排序区间中元素为空，算法结束。</font>

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
    //寻找插入点，当 preIndex >=0 且当前元素的下一元素小于当前元素时，继续循环。
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

选择排序（Selection Sort）首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。 

**实现步骤：**

1. 初始状态：无序区为R[1..n]，有序区为空；
2. 第i趟排序(i=1,2,3…n-1)开始时，当前有序区和无序区分别为R[1..i-1]和R(i..n）。该趟排序从当前无序区中-选出关键字最小的记录 R[k]，将它与无序区的第1个记录R交换，使R[1..i]和R[i+1..n)分别变为记录个数增加1个的新有序区和记录个数减少1个的新无序区；
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

动画实现：**

![img](https://tva1.sinaimg.cn/large/00831rSTly1gciarp64ljg30mj06w7l2.gif)

返回顶部](#1、排序算法)

----

## 6、归并排序

[返回顶部](#1、排序算法)

----

## 7、快速排序

[返回顶部](#1、排序算法)

----

## 8、计数排序

[返回顶部](#1、排序算法)

----

## 9、基数排序

[返回顶部](#1、排序算法)

----

## 10、桶排序

[返回顶部](#1、排序算法)