# array与vector

```c++
// array 定义必须指定元素个数，要么直接用字面量指定，要么给出几个值
int a[N];
```

- 数组存放在栈中，其内存的分配和释放完全由系统自动完成
- vector，存放在堆中，由STL库中程序负责内存的分配和释放

## STL与循环

```c++
    for (auto &n : nums) {
        cout << n << endl;
    }
```