# merge_sort

分治：把一个任务分成形式和原任务相同，但规模更小的几个部分任务(通常是2个)

## c++代码

```python
void merge_sort(int q[], int l, int r)
{
    //递归的终止情况
    if(l >= r) return;

    //第一步：分成子问题
    int mid = l + r >> 1;

    //第二步：递归处理子问题
    merge_sort(q, l, mid ), merge_sort(q, mid + 1, r);

    //第三步：合并子问题
    int k = 0, i = l, j = mid + 1, tmp[r - l + 1];
    while(i <= mid && j <= r)
        if(q[i] <= q[j]) tmp[k++] = q[i++];
        else tmp[k++] = q[j++];
    while(i <= mid) tmp[k++] = q[i++];
    while(j <= r) tmp[k++] = q[j++];

    for(k = 0, i = l; i <= r; k++, i++) q[i] = tmp[k];
}
```



```c++
void Merge(int a[], int l, int m, int r) {
    // 将a的l到m 和m+1 到r 有序化合并到tmp，再拷贝回a[l,r]
    // O(n)
    int tmp[r - l + 1]; // 需要归并的数一共是r-l+1个,简单的小学问题
    int i = l, j = m + 1;
    int k = 0; // tmp指针

    // 就看谁小呗
    while (i <= m && j <= r) {
        if (a[i] <= a[j]) // 加等号才能保持稳定？
            tmp[k++] = a[i++];
        else
            tmp[k++] = a[j++];
    }
    while (i <= m)tmp[k++] = a[i++];
    while (j <= r)tmp[k++] = a[j++];

    for (int i = 0; i < k; ++i) {
        a[l + i] = tmp[i];
    }
}

void MergeSort(int a[], int l, int r) {
    // 边界条件
    if (l >= r) return;
    int mid = (l + r) >> 1;
    // 分治
    MergeSort(a, l, mid);MergeSort(a, mid + 1, r);
    // 错误写法：mid-1 mid
    // 合并
    Merge(a, l, mid, r);
}
```



## python代码

```python
# 从c++翻译而来
nums = [3, 1, 2, 4, 5]
tmp = [0] * 100010


def merge_sort(l, r):
    if l >= r: return
    # mid = int((l + r) / 2)
    mid = (l + r) // 2
    i, j, k = l, mid + 1, 0
    merge_sort(l, mid)
    merge_sort(mid + 1, r)

    while i <= mid and j <= r:
        if nums[i] <= nums[j]:
            tmp[k] = nums[i]
            k = k + 1
            i = i + 1
        else:
            tmp[k] = nums[j]
            k = k + 1
            j = j + 1
    while i <= mid:
        tmp[k] = nums[i]
        k = k + 1
        i = i + 1
    while j <= r:
        tmp[k] = nums[j]
        k = k + 1
        j = j + 1
    k = 0
    for i in range(l, r + 1):
        nums[i] = tmp[k]
        k = k + 1


# def main():
if __name__ == "__main__":
    for num in nums:
        print(num)
    merge_sort(0, len(nums) - 1)
    for num in nums:
        print(num)
```





```python
def merge(q, l, m, r):
    """合并左子数组和右子数组"""
    # 左子数组区间 [l, m], 右子数组区间 [m+1, r]
    tmp = [0] * (r - l + 1)
    i, j, k = l, m + 1, 0
    while i <= m and j <= r:
        if q[i] <= q[j]:
            tmp[k] = q[i]
            i += 1
        else:
            tmp[k] = q[j]
            j += 1
        k += 1
    while i <= m:
        tmp[k] = q[i]
        i += 1
        k += 1
    while j <= r:
        tmp[k] = q[j]
        j += 1
        k += 1
    # 拷贝回原数组
    for k in range(0, len(tmp)):
        q[l + k] = tmp[k]


def merge_sort(q, l, r):
    """归并排序"""
    # 终止条件
    if l >= r:
        return  # 当子数组长度为 1 时终止递归
    # 划分阶段
    m = (l + r) // 2  # 计算中点
    merge_sort(q, l, m)  # 递归左子数组
    merge_sort(q, m + 1, r)  # 递归右子数组
    # 合并阶段
    merge(q, l, m, r)

if __name__ == '__main__':
    q = [1, 1, 4, 5, 1, 4]
    merge_sort(q, 0, len(q) - 1)
    print(q)

```

