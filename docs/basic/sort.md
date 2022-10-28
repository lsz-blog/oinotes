---
tags:
  - 基础算法
  - 排序
---

## 快速排序

快速排序是一种分治的排序算法，其基本思想是：通过一趟排序将待排记录分割成独立的两部分，其中一部分记录的关键字均比另一部分的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序。

### 算法描述

1. 从数列中挑出一个元素，称为 “基准”（pivot）；

2. 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；

3. 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。

### 代码

```c++
void quick_sort (int *arr, int l, int r) {
    if (l >= r) swap(l, r);
    int i = l-1, j = r+1, x = arr[l+r>>1];
    while (i < j) {
        do i++; while (arr[i] < x);
        do j--; while (arr[j] > x);
        if (i < j) swap(arr[i], arr[j]);
    }
    quick_sort(arr, l, j);
    quick_sort(arr, j+1, r);
}
```

## 归并排序

归并排序是建立在归并操作上的一种有效的排序算法，该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为2-路归并。

### 算法描述

1. 把长度为 $n$ 的输入序列分成两个长度为 $\dfrac{n}{2}$ 的子序列；

2. 对这两个子序列分别采用归并排序；

3. 将两个排序好的子序列合并成一个最终的排序序列。

### 代码

```c++
void merge_sort (int *arr, int l, int r) {
    if (l >= r) return;
    int mid = l+r >> 1;
    merge_sort(arr, l, mid);
    merge_sort(arr, mid+1, r);
    int k = 0, i = l, j = mid+1;
    while (i <= mid && j <= r) {
        if (arr[i] <= arr[j]) tmp[k++] = arr[i++];
        else tmp[k++] = arr[j++];
    }
    while (i <= mid) tmp[k++] = arr[i++];
    while (j <= r) tmp[k++] = arr[j++];
    for (int i = l, j = 0; i <= r; i++, j++) arr[i] = tmp[j];
}
```

## 堆排序

堆排序是指利用堆这种数据结构所设计的一种排序算法。堆是一个近似完全二叉树的结构，并同时满足堆的性质：即子节点的键值或索引总是小于（或者大于）它的父节点。

我们可以使用 `std::priority_queue` 来实现堆排序。

### 代码

```cpp
void heap_sort (int *arr, int l, int r) {
    std::priority_queue<int, std::vector<int>, std::greater<int>> q;
    for (int i = l; i <= r; i++) q.push(arr[i]);
    for (int i = l; i <= r; i++) arr[i] = q.top(), q.pop();
}
```

## std::sort

`std::sort` 是 C++ STL 中的排序函数，其底层实现是：

+ 当元素个数很小时，使用插入排序；

+ 当元素个数很大时，使用快速排序；

+ 当递归层数超过某个值时，使用堆排序。

`std::sort` 源代码：

```cpp
template<typename _RandomAccessIterator, typename _Compare>
void sort(_RandomAccessIterator __first, _RandomAccessIterator __last, _Compare __comp) {
    if (__last - __first < 16) {
        __insertion_sort(__first, __last, __comp);
        return;
    }
    _RandomAccessIterator __cut = __unguarded_partition(__first, __last, __comp);
    __unguarded_linear_insert(__cut, __value_type(__first), __comp);
    __final_insertion_sort(__first, __cut, __comp);
}
```