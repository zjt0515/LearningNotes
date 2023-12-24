# bubble_sort

## c++代码

```c++
// 外层循环：排序的轮数，一共 n - 1 轮 
    for (int i = 0; i < nums.size() - 1; i++)
    {
        bool flag = false;
        // 内层循环：每轮循环要比较的次数，如第一轮从0到n，以后每轮n都要--
        // bug1: 注意数组越界，看用到的最大下标就行，如 j + 1 = size -0 -1 ，一开始没有-1导致数组越界
        for (int  j = 0; j < nums.size() - 1 - i; j++)
        {
            cout<< "j:" << j << " ";
            if (nums[j] > nums[j+1])
            {
                int temp;
                temp = nums[j+1];
                nums[j+1] = nums[j];
                nums[j] = temp;
                //swap(nums[j], nums[j+1]);
                flag = true;
            }
        }
        // 标志位优化
        if (flag == false)
        {
            break;
        }
    }
```

- 外层循环是 0-n-1
- 内层循环是 0-n-1-i

## 复杂度分析

平均时间复杂度：O(N2)
最差时间复杂度：O(N2)
空间复杂度：O(1)
是否稳定：稳定
