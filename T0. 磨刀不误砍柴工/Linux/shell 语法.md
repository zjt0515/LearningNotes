## 注释

```shell
# 单行注释

:<<EOF
多行注释1
多行注释2
EOF
```

## 变量

```shell
name1=xiaohu # =号两边不能有空格，可以加上单/双引号
echo $name1
echo ${name1}

# 只读变量
constName=shy
readonly constName
declare -r constName

# 删除变量
unset constName

```

变量类型
自定义变量（局部变量）
子进程不能访问的变量
环境变量（全局变量）
子进程可以访问的变量
自定义变量改成环境变量：

acs@9e0ebfcd82d7:~$ name=yxc  # 定义变量
acs@9e0ebfcd82d7:~$ export name  # 第一种方法
acs@9e0ebfcd82d7:~$ declare -x name  # 第二种方法
环境变量改为自定义变量：

acs@9e0ebfcd82d7:~$ export name=yxc  # 定义环境变量
acs@9e0ebfcd82d7:~$ declare +x name  # 改为自定义变量



新开bash：bash

退出子进程：exit 



## 字符串

单引号与双引号的区别：

- 单引号中的内容会原样输出，不会执行、不会取变量；
- 双引号中的内容可以执行、可以取变量；

```
name=yxc  # 不用引号
echo 'hello, $name \"hh\"'  # 单引号字符串，输出 hello, $name \"hh\"
echo "hello, $name \"hh\""  # 双引号字符串，输出 hello, yxc "hh"
```



```shell
# 获取字符串长度
name="yxc"
echo ${#name}  # 输出3

# 提取子串
name="hello, yxc"
echo ${name:0:5}  # 提取从0开始的5个字符
```

## 默认变量

在执行shell脚本时，可以向脚本传递参数。$1是第一个参数，$2是第二个参数，以此类推。特殊的，$0是文件名（包含路径）

向脚本传递参数时，参数个数超过一位需要用大括号括起来
echo 10

| 参数       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| $#         | 代表文件传入的参数个数，如上例中值为4                        |
| $*         | 由所有参数构成的用空格隔开的字符串，如上例中值为"$1 $2 $3 $4" |
| $@         | 每个参数分别用双引号括起来的字符串，如上例中值为"$1" "$2" "$3" "$4" |
| $$         | 脚本当前运行的进程ID                                         |
| $?         | 上一条命令的退出状态（注意不是stdout，而是exit code）。0表示正常退出，其他值表示错误 |
| $(command) | 返回command这条命令的stdout（可嵌套）                        |
| `command`  | 返回command这条命令的stdout（不可嵌套）                      |

## 数组

- 可以存放不同类型的值

- 只支持一维数组

- 初始化时不需要指明数组大小
- 可以跳着定义

```shell
array=(1 abc "def" yxc)
array[0]=1
array[1]=abc
array[2]="def"
array[3]=yxc

# 读取整个数组
${array[@]}
${array[*]}

# 获取数组长度
${#array[@]}
${#array[*]}
```

## expr

- 用空格隔开每一项
- 用反斜杠放在shell特定的字符前面（发现表达式运行错误时，可以试试转义）
- 对包含空格和其他特殊字符的字符串要用引号括起来
- expr会在stdout中输出结果。如果为逻辑关系表达式，则结果为真时，stdout输出1，否则输出0。
- exit code：如果为逻辑关系表达式，则结果为真时，exit code为0，否则为1。



### 字符串表达式
`length STRING`
返回STRING的长度
`index STRING CHARSET`
CHARSET中任意单个字符在STRING中最前面的字符位置，下标从1开始。如果在STRING中完全不存在CHARSET中的字符，则返回0。
`substr STRING POSITION LENGTH`
返回STRING字符串中从POSITION开始，长度最大为LENGTH的子串。如果POSITION或LENGTH为负数，0或非数值，则返回空字符串。
示例：

str="Hello World!"

echo `expr length "$str"`  # ``不是单引号，表示执行该命令，输出12
echo `expr index "$str" aWd`  # 输出7，下标从1开始
echo `expr substr "$str" 2 3`  # 输出 ell

### 整数表达式
算术表达式优先级低于字符串表达式，高于逻辑关系表达式。

+ -
加减运算。两端参数会转换为整数，如果转换失败则报错。
+ */ %
乘，除，取模运算。两端参数会转换为整数，如果转换失败则报错。

() 可以改变优先级，但需要用反斜杠转义

```shell
a=3
b=4

echo `expr $a + $b`  # 输出7
echo `expr $a - $b`  # 输出-1
echo `expr $a \* $b`  # 输出12，*需要转义
echo `expr $a / $b`  # 输出0，整除
echo `expr $a % $b` # 输出3
echo `expr \( $a + 1 \) \* \( $b + 1 \)`  # 输出20，值为(a + 1) * (b + 1)
```

