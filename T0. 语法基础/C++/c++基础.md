# C++ 基础



## 进制转换

16->2：1位变成四位
8->2：1位变成3位

## 左移右移运算符

- 算数移位
- 逻辑移位
- 旋转循环移位
- 带进位旋转循环移位



符号位：0正1负

```c++
// test

	int i = 0x40000000; // 二进制：i = 01000000 00000000 00000000 00000000
    cout << i << endl;
    i = i << 1;         // 二进制：i = 10000000 00000000 00000000 00000000 == -0 == -2147483648
    cout << i << endl;
    i = i << 1;
    cout << i << endl;
```



右移：所有位右移后，补上原本的符号位数字
算数右移(正负不变)

```c++
// test
    int j = 0b10000000000000000000000000000000;
    cout << j << endl;
    j = j >> 1;
    cout << j << endl;
```



