## 通过`bind()`函数绑定this值

```js
const talk = person.talk.bind(person); // 为talk绑定this
talk(); //如果没有bind， 输出window 没有东西调用talk，默认是window调用
```





## 箭头函数

```js
const f = (x) => f{
	return x*x
}
```

> 简写方式： 当参数唯一，可以删括号
>
> 当函数体只有一句时， 可以删return

- 箭头函数不会重新绑定this,  this就是外部的this



## 对象的解构

```js
const {name, age} = person
//const {name: new_name, age} = person
console.log(name,age)
```



## 数组和对象展开——...运算符



## `export default`

import的时候不能加大括号，且`export default`唯一



## 分号争论

可以不写分号，但是遇到换行必须要加小括号



## key和value相同

可以只写key