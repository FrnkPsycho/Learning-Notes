# 类型

> 基本类型不是对象。
>
> 基本类型不能存储数据。
>
> 所有的属性/方法操作都是在临时对象的帮助下执行的。

## 数字

默认double类型

科学计数法 `1e3 2.7e-3`

BigInt `1729371927123123123123123123123123n`

十六进制 `0xffffff`

二进制 `0b00110011`

八进制 `0o377`  



`number.toFixed(n)` 保留n位小数

`number.toString(base)` base为进制数 36最大

`Math.floor`向下舍入 `Math.ceil`向上舍入 `Math.round`四舍五入 `Math.Trunc`移除小数点

`isFinite()` 将参数转换为数字，如果是常规数字返回true

`isNaN()`参数是否为NaN 值"NaN" 是独一无二的，它不等于任何东西，包括它自身  



`parseInt('', base)` `parseFloat('')`

从字符串中读取数字直到无法读取（从头开始遇到不能读的就结束）

`Math.random()` 返回一个从0到1的随机数（不包括1）

`Math.max()` `Math.pow()`

## 字符串

反引号更通用，可以嵌入变量/换行

`\xXX` `\uXXXX` `\u{XXXXX}` 

`str.length` 不用括号，因为length是字符串的属性

`str.charAt(index)` or `str[index]`√

`for ( let char of "hello") {}` 遍历字符串

`str.toUpperCase()`

`str.toLowerCase()`

`str.indexOf(substr, pos)`

-1 为其理想的退出循环条件

`str.slice(start, end)`



## 数组

`let arr = []`

`arr[]` or `arr.at()`后者可以倒数

`arr[i]()` 调用arr[i]函数  



js中的数组相当于双向队列(deque)

`pop`/`push` 懂得都懂

`shift` 取出第一个元素并返回

`unshift` 在首端插入元素

`push`/`unshift` 都能一次插入多个元素

### 数组循环

```js
for ( let k of array ) {
    // code here...
}
```

### 清空数组

`arr.length = 0`

length是数组对象的一个可修改属性 增加无事 减小会截断数组

### 数组转字符串

```
[] --> ''
[1] --> '1'
[1,2,3] --> '1,2,3'
```

不要用 == 来比较

