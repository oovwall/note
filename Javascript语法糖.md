# Javascript语法糖

[TOC]

##### 数字
###### 金额千分符
```javascript
(123456789).toLocaleString('en-US')  // 1,234,567,89
(123456789).toLocaleString('zh-cmn', {style: 'currency', currency: 'CNY'}) // ￥123,456,789.00
(123456789).toLocaleString('en-US', {style: 'currency', currency: 'CNY'}) // CN¥123,456,789.00
(123456789).toLocaleString('en-US', {style: 'currency', currency: 'JPY'}) // ¥123,456,789
(123456789).toLocaleString('en-US', {style: 'currency', currency: 'USD'}) // $123,456,789.00
new Intl.NumberFormat().format(123456789) // 123,456,789
```

###### 判断数字是否为小数
```javascript
/**
 * 第一种方法
 */
console.log(123.87 % 1 !== 0) // true
console.log(123.00 % 1 !== 0) // false
console.log(123 % 1 !== 0) // false

/**
 * 第二种方法
 */
console.log(!!123.87.toString().split('.')[1]) // true
console.log(!!123.00.toString().split('.')[1]) // false
console.log(!!(123).toString().split('.')[1]) // false
```

###### 数字取整
```javascript
console.log(parseInt(2.3333)) // 2
console.log(Math.trunc(2.3333)) // 2， 返回整数部分
console.log(Math.floor(2.3333)) // 2， 向下取整
console.log(Math.floor(-2.3333)) // -3， 向下取整
console.log(~~2.3333) // 2
console.log(~~-2.3333) // -2
```

##### 字符串
###### 单行写一个评级组件
```javascript
const starRate = rate => '★★★★★☆☆☆☆☆'.slice(5 - rate, 10 - rate)
starRate(1) // "★☆☆☆☆"
starRate(3) // "★★★☆☆"
starRate(5) // "★★★★★"
```

##### 数组
###### 数组去重
```javascript
const arr = [1, 1, '1', '2', 1]
/* 以下重复 */
const unique = arr => Array.from(new Set(arr))
const unique = arr => [...new Set(arr)]
console.log(unique(arr))   // [1, "1", "2"]
```

##### 时间
###### 获取时间毫秒数（时间戳）
```javascript
/* 以下重复 */
const t = + new Date
const t = + new Date()
const t = new Date().getTime()
const t = new Date().valueOf() // 返回对象的原始值
const t = Date.now()
const t = Number(new Date)
```
