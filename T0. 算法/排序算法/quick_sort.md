# quick_sort

- 分治
- 原址排序
- 123

````c++
void QuickSort(int q[], int l, int r){
    if(l >= r) return;
    int i = l , j = r ;
	// 基准数=左端点
    int x = q[l];
    while (i < j) {
        //交换方式：单个指针运动，交换
        
        while( i < j && q[j] >= x)
            j--;
        swap(a[i],a[j]);
        while( i < j && q[i] <= x)
            i--;
        swap(a[i],a[j]);
    }
    quick_sort(q, l, i - 1);quick_sort(q, i + 1, r);
}
````

## C++代码一

```c++

int Partition(vector<int>& nums, int l, int r) {
    // 基准数=nums[l]
    int i = l, j = r;
    while (i < j) {
        while (i < j && nums[j] >= nums[l]) j--;
        while (i < j && nums[i] <= nums[l]) i++;
        swap(nums[i], nums[j]);
    }
    swap(nums[i], nums[l]);
    return i;
}

void quickSort(vector<int>& nums, int l, int r) {
    if (l >= r) return;
    // 哨兵划分操作
    int i = partition(nums, l, r);
    quickSort(nums, l, i - 1);
    quickSort(nums, i + 1, r);
}
```

## C++代码二

```c++
void QuickSort(int q[], int l, int r){
    if(l >= r) return;
    int i = l - 1, j = r + 1;
	// 基准数选取中间数
    int x = q[(l + r) >> 1];
    while (i < j) {
        do
        {
            i++;
        } while (q[i] < x);
        do
        {
            j--;
        } while (q[j] > x);
        if(i < j) swap(q[i], q[j]);
    }
    quick_sort(q, l, j);quick_sort(q, j+1, r);
}
```

## C++代码三

```c++
int Partition(int q[], int l, int r) {
    int i = l, j = r;
    int pivot = q[l];
    while (i < j) {
        /* 如果没有 i < j的限制, j遇到i之后，由于i之前保存的是大于等于基准数的值，
        循环继续，而i左边的数都是小于基准数的值, 不能再去修改这些已经归好位的值，因此必须加上i < j
        也就是说：小循环体内是会出现i >= j的情况的，虽然只会出现一次
        */
        while (q[j] >= pivot && i < j) {
            j++;
        }
        q[i] = q[j];
        while (q[i] < pivot && i < j) {
            i--;
        }
        q[j] = q[i];
    }
    q[i] = pivot;
    return i;
}

void QuickSort(int q[], int l, int r) {
    if (l >= r)
        return;
    int p = Partition(q, l, r);
    QuickSort(q, l, p - 1);
    QuickSort(q, p + 1, r);
}
```

## 复杂度分析

平均时间复杂度：O(nlgn)
最差时间复杂度：O(n2)
空间复杂度：
是否稳定：不稳定

## 算法导论

循环不变量：

1. 若p <= k <= i,则A[k] <= x。
2. 若i+l <= k <= j-1,则A[k ]> x。
3. 若k=r,则A[k]=x。

> 循环不变量在第一轮迭代之前是成立的，并且在每一轮迭代后仍然都成立。在循环结束时，该循环不变量还可以为证明正确性提供有用的性质。
> 最坏情况
>
> 划分产生的两个子问题分别包含了n-l个元素和0个元素时
>

## tag1

确定基准，如 l
从l, r开始设立两个i, j指针
两个指针不同时移动，过程中保证一个指针指向空的位置，另一个指针指向待移动的位置
指针相遇时，所指向的位置为空???，将基准数放入





# todo

有大量重复元素的情况

边界问题可视化