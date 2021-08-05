---
layout: post
title: '五种常见排序算法--Java实现'
tags:
  - 排序算法
hero: https://source.unsplash.com/collection/345758/
overlay: purple
---

本文实现了八大排序中的五种常见排序算法：选择、插入、冒泡、快速、归并，这五种排序算法在面试的时候也经常会被问到，一起来看看吧。
{: .lead}
<!–-break-–>


## 八大排序算法复杂度及稳定性分析
稳定性指的是假定在待排序的记录序列中，存在多个具有相同的关键字的记录，若经过排序，这些记录的**相对次序保持不变**，即在原序列中，A1=A2，且A1在A2之前，而在排序后的序列中，A1仍在A2之前，则称这种排序算法是**稳定**的；否则称为不稳定的。
<table border="1">
    <tr>
        <th>排序方式</th>
        <th>平均时间复杂度</th>
        <th>最好时间复杂度</th>
        <th>最坏时间复杂度</th>
        <th>空间复杂度</th>
        <th>稳定性</th>
    </tr>
    <tr>
        <td>选择排序</td>
        <td>O(n^2)</td>
        <td>O(n^2)</td>
        <td>O(n^2)</td>
        <td>O(1)</td>
        <td>不稳定</td>
    </tr>
    <tr>
        <td>插入排序</td>
        <td>O(n^2)</td>
        <td>O(n)</td>
        <td>O(n^2)</td>
        <td>O(1)</td>
        <td>稳定</td>
    </tr>
    <tr>
        <td>冒泡排序</td>
        <td>O(n^2)</td>
        <td>O(n)</td>
        <td>O(n^2)</td>
        <td>O(1)</td>
        <td>稳定</td>
    </tr>
    <tr>
        <td>快速排序</td>
        <td>O(nlog n)</td>
        <td>O(nlog n)</td>
        <td>O(n^2)</td>
        <td>O(1)</td>
        <td>不稳定</td>
    </tr>
    <tr>
        <td>归并排序</td>
        <td>O(nlog n)</td>
        <td>O(nlog n)</td>
        <td>O(nlog n)</td>
        <td>O(n)</td>
        <td>稳定</td>
    </tr>
    <tr>
        <td>希尔排序</td>
        <td>O(n^1.3)</td>
        <td></td>
        <td></td>
        <td>O(1)</td>
        <td>不稳定</td>
    </tr>
    <tr>
        <td>堆排序</td>
        <td>O(nlog n)</td>
        <td>O(nlog n)</td>
        <td>O(nlog n)</td>
        <td>O(1)</td>
        <td>不稳定</td>
    </tr>
    <tr>
        <td>基数排序</td>
        <td>O(d(n + r)))</td>
        <td>O(d(n + r)))</td>
        <td>O(d(n + r)))</td>
        <td>O(r)</td>
        <td>稳定</td>
    </tr>
</table>

## 五种常见排序算法Java实现
### 选择排序
**工作原理**：首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

```java
    public void selectiveSort(int[] nums) {
        if (nums == null || nums.length == 0) {return;}
        int len = nums.length;
        for (int i = 0; i < len; i++) {
            // 维护一个当前最小值和最小值的索引
            int min = nums[i], index = i;
            for (int j = 0; j < len; j++) {
                if (min > nums[j]) {
                    // 维护最小值和最小值的索引
                    min = nums[j];
                    index = j;
                }
            }
            // 交换最小值和排序序列起始位置
            nums[index] = nums[i];
            nums[i] = min;
        }
    }
```
### 插入排序
**工作原理**：从第二个元素开始，找到合适的位置，将元素插入到之前已**排好序**的元素中去，依次下去最终完成排序。

```java
public void insertSort(int[] nums) {
        if (nums == null || nums.length == 0) {return;}
        int len = nums.length;
        for (int i = 1; i < len; i++) {
            for (int j = i; j > 0; j--) {
                if (nums[j] < nums[j - 1]) {
                    int tmp = nums[j];
                    nums[j] = nums[j - 1];
                    nums[j - 1] = tmp;
                }
            }
        }
    }
```
### 冒泡排序
**工作原理**：比较是相邻的两个元素比较，交换也发生在这两个元素之间。
```java
public void bubbleSort(int[] nums) {
        if (nums == null || nums.length == 0) {return;}
        int len = nums.length;
        for (int i = len - 1; i >= 0; i--) {
            for (int j = 0; j < i; j++) {
                if (nums[j] > nums[j + 1]) {
                    int tmp = nums[j];
                    nums[j] = nums[j + 1];
                    nums[j + 1] = tmp;
                }
            }
        }
    }
```
### 快速排序
**思路**：
a. 找基准（一般为数组第一个元素）；<br>
b. 从右往左搜索，遇到比基准小的就停下（注意：基准如果是数组首位元素，一定先要从右往左搜索）；从左往右搜索，遇到比基准大的就停下。交换这两个元素的位置。<br>
c. 重复上述动作b，直到两个指针相遇（此时相遇处的元素一定比基准小，因为是先从右往左搜索），交换基准与相遇处元素的位置。
```java
public void quickSort(int[] s, int l, int r) {
        if (l > r) {    //如果左边大于右边，直接return
            return;
        }
        int i = l, j = r;
        int base = s[l];//将数组首位作为基准
        while (i != j) {//i和j没有相遇
            while (s[j] >= base && i < j) {//当s[j]大于等于基准并且i<j(不然i有可能到j右边了)时右指针左移
                j--;
            }
            while (s[i] <= base && i < j) {//当s[i]小于等于基准并且i<j(不然i有可能到j右边了)时左指针右移
                i++;
            }
            //当执行到这时，左右指针都停下，此时i指向的元素大于基准，j指向的元素小于基准，交换这两个元素的位置
            int temp = s[i];
            s[i] = s[j];
            s[j] = temp;
        }
        //跳出循环的时候说明左右指针相遇了，此时两个指针指向的元素小于基准，交换基准与该元素的位置
        s[l] = s[i];
        s[i] = base;
        //此时基准归位，base左边的数字都比它小，右边的数字都比它大

        //分治（递归调用）
        quickSort(s, l, i-1);
        quickSort(s, i+1, r);
    }
```
### 归并排序
**工作原理**：归并排序是建立在归并操作上的一种有效的排序算法。该算法是采用分治法的一个非常典型的应用。
```java
    public void mergeSort(int[] nums, int left, int right) {
        if (left >= right) {return;}
        int mid = (left + right) / 2;
        // 左边
        mergeSort(nums, left, mid);
        // 右边
        mergeSort(nums, mid + 1, right);
        // 左右归并
        merge(nums, left, mid, right);
    }
    public void merge(int[] nums, int left, int mid, int right) {
        // 左指针
        int i = left;
        // 右指针
        int j = mid + 1;
        // 临时数组的索引
        int k = 0;
        // 用来保存归并后的临时数组
        int[] tmp = new int[right - left + 1];
        while (i <= mid && j <= right) {
            if (nums[i] < nums[j]) { // 如果左边比右边小
                tmp[k++] = nums[i++];
            } else {
                tmp[k++] = nums[j++];
            }
        }
        // 将剩下的放入临时数组，以下两个while只有一个会执行
        while (i <= mid) {
            tmp[k++] = nums[i++];
        }
        while (j <= right) {
            tmp[k++] = nums[j++];
        }
        // 把临时数组中的数覆盖nums数组
        for (int l = 0; l < tmp.length; l++) {
            nums[l + left] = tmp[l];
        }
    }
```