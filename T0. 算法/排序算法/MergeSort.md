# python

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