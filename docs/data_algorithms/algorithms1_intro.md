排序算法介绍
==============================

## 算法分析

https://zh.wikipedia.org/wiki/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95#.E7.A9.A9.E5.AE.9A.E6.80.A7

http://algorithm.yuanbin.me/zh-hans/basics_sorting/

### 复杂度

1. `时间复杂度`:执行时间(比较和交换次数)
2. `空间复杂度`:所消耗的额外内存空间
    + 使用小堆栈或表
    + 使用链表或指针、数组索引来代表数据
    + 排序数据的副本

### 稳定性
相同值的项排序时位置不会变化就是稳定的

+ 稳定的排序
    + 冒泡排序（bubble sort）— O(n2)
    + 鸡尾酒排序（cocktail sort）—O(n2)
    + 桶排序（bucket sort）—O(n)；需要O(k)额外空间
    + 计数排序（counting sort）—O(n+k)；需要O(n+k)额外空间
    + 归并排序（merge sort）—O(n log n)；需要O(n)额外空间
    + 原地归并排序— O(n log2 n)如果使用最佳的现在版本
    + 二叉排序树排序（binary tree sort）— O(n log n)期望时间；O(n2)最坏时间；需要O(n)额外空间
    + 鸽巢排序（pigeonhole sort）—O(n+k)；需要O(k)额外空间
    + 基数排序（radix sort）—O(n·k)；需要O(n)额外空间
    + 侏儒排序（gnome sort）— O(n2)
    + 图书馆排序（library sort）— O(n log n)期望时间；O(n2)最坏时间；需要(1+ε)n额外空间
    + 块排序（block sort）— O(n log n)

+ 不稳定的排序
    + 选择排序（selection sort）—O(n2)
    + 插入排序（insertion sort）—O(n2)
    + 希尔排序（shell sort）—O(n log2 n)如果使用最佳的现在版本
    + Clover排序算法（Clover sort）—O(n)期望时间，O(n2)最坏情况
    + 梳排序— O(n log n)
    + 堆排序（heap sort）—O(n log n)
    + 平滑排序（smooth sort）— O(n log n)
    + 快速排序（quick sort）—O(n log n)期望时间，O(n2)最坏情况；对于大的、随机数列表一般相信是最快的已知排序
    + 内省排序（introsort）—O(n log n)
    + 耐心排序（patience sort）—O(n log n + k)最坏情况时间，需要额外的O(n + k)空间，也需要找到最长的递增子序列（longest increasing subsequence）

## 分类

