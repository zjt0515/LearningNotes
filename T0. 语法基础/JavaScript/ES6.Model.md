module 体系：例如 python 的 import

## 严格模式

- 变量必须声明后再使用
- 函数的参数不能有同名属性，否则报错
- 不能使用`with`语句
- 不能对只读属性赋值，否则报错
- 不能使用前缀 0 表示八进制数，否则报错
- 不能删除不可删除的属性，否则报错
- 不能删除变量`delete prop`，会报错，只能删除属性`delete global[prop]`
- `eval`不会在它的外层作用域引入变量
- `eval`和`arguments`不能被重新赋值
- `arguments`不会自动反映函数参数的变化
- 不能使用`arguments.callee`
- 不能使用`arguments.caller`
- **禁止`this`指向全局对象**
- 不能使用`fn.caller`和`fn.arguments`获取函数调用的堆栈
- 增加了保留字（比如`protected`、`static`和`interface`）



## export

- function
- class
- var / let / const variables

```javascript
export let myName = 'zha'

let myName = 'zha'
export { myName, } // 必须有大括号

// 重命名
export {
	myName as name,
    
}

// 实时
export var foo = 'bar';
setTimeout(() => foo = 'baz', 500);
```



## import

- import是静态执行

```javascript
import { myName } from './'

import { myName as name } from './'

// myName只读：本质上是输入接口，不允许改写
// 但是如果是对象的属性是可以改写的的
```

