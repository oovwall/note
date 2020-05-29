# 函数式编程范式

## 高阶函数
- 可以把函数作为参数传递给另一个函数
- 可以把函数作为另一个函数的返回结果
   
### 函数作为参数  
- 模拟forEach()  
    ```js
    const arr = [1, 2, 3, 4, 5, 6]
    
    function forEach(arr, fn) {
      for (let i = 0; i < arr.length; i++) {
        fn(arr[i])
      }
    }
    
    forEach(arr, item => {
      console.log(item) // 1 2 3 4 5 6
    })
    ```

- 模拟filter()
    ```js
    const arr = [1, 2, 3, 4, 5, 6]
    
    function filter(arr, fn) {
      let results = []
      for (let i = 0; i < arr.length; i++) {
        if (fn(arr[i])) {
          results.push(arr[i])
        }
      }
      return results
    }
    
    console.log(filter(arr, item => item > 3))
    ```
  
- 摸拟map()
    ```js
    function map (arr, fn) {
      let results = []
      for (let i = 0; i < arr.length; i++) {
        results.push(fn(arr[i]))
      }
      return results
    }
    
    const arr = [1, 2, 3, 4, 5, 6]
    
    console.log(map(arr, item => item * 2)) // [ 2, 4, 6, 8, 10, 12 ]
    ```

### 函数作为返回值
once函数的实现（支付场景）
```js
function once (fn) {
  let done = false
  return function () {
    if (!done) {
      done = true
      return fn.apply(this, arguments)
    }
  }
}

const pay = once(money => {
  console.log(`you paied ${money} RMB`)
})

pay(5)
pay(5)
pay(5)
pay(5)
```

### 使用高阶函数的意义
- 抽象可以帮我们屏蔽细节，只需要关注与我们的目标
- 高阶函数是用来抽象通用的问题

## 闭包
`闭包的概念`：函数和其周围的状态（词法环境）的引用捆绑在一起形成闭包。闭包可以在另一个作用域中调用一个函数的内部函数并访问到该函数的作用域中的成员。  
`闭包的本质`：函数在执行时会放到一个执行栈上，当函数执行完毕之后会从执行栈上移除，**但是堆上的作用域成员因为外部引用不能释放**，因此内部函数依然可以访问外部函数的成员。

## 纯函数的好处
**纯函数**：对于相同的输入永远会得到相同的输出，而且没有任何可观察的副作用。  
- **可缓存**：可以提高程序的性能，例：模拟lodash记忆函数
    ```js
    function getArea (r) {
      console.log('run get area!')
      return Math.PI * r * r
    }
    
    function memoize (fn) {
      const cache = {}
      return function () {
        let key = JSON.stringify(arguments)
        cache[key] = cache[key] || fn.apply(fn, arguments)
        // cache[key] = cache[key] || fn(...arguments)
        return cache[key]
      }
    }
    
    const getAreaMemoize = memoize(getArea)
    console.log(getAreaMemoize(4))
    console.log(getAreaMemoize(4))
    console.log(getAreaMemoize(4))
    console.log(getAreaMemoize(4))
    
    // run get area!
    // 50.26548245743669
    // 50.26548245743669
    // 50.26548245743669
    // 50.26548245743669
    ```

- **可测试**：纯函数让测试更方便，可并行处理

## 函数柯里化
`柯里化`：当一个函数有多个参数的时侯先传递一部分参数调用它（这部分参数以后永远不变），然后返回一个新的函数接收剩余的参数，返回结果。
```js
function checkAge (min) {
  return function (age) {
    return min <= age
  }
}
// const checkAge = min => (age => min <= age)

const checkAge18 = checkAge(18)

console.log(checkAge18(20))
console.log(checkAge18(24))
```

### 模拟函数柯里化

## lodash中的fp模块
fp模块是已经柯里化的函数
```js
const fp = require('lodash/fp')

const f = fp.flowRight(fp.join('-'), fp.map(_.toLower), split(' '))
```

