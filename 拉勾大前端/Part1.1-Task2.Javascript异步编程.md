# Javascript异步编程

## Promise 使用案例：编写一个返回promise的ajax方法
```js
const ajax = function (url) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest()
    xhr.open('GET', url)
    xhr.responseType = 'json'
    xhr.onload = function () {
      if (this.status === 200) {
        resolve(this.response)
      } else {
        reject(new Error(this.statusText))
      }
    }
    xhr.send()
  })
}

ajax('/promise/api/user.json').then(res => {
  console.log(res)
}, err => {
  console.log(err)
})
```

## Promise 链式调用
每个then方法它实际上都是在为上一个then返回的Promise对象添加状态明确后的回调  
**模拟数据**
```json
[
  {
    "name": "zhang",
    "age": 17
  },
  {
    "name": "li",
    "age": 18
  },
  {
    "name": "wang",
    "age": 19
  },
  {
    "name": "zhao",
    "age": 20
  }
]
```
**代码**
```js
ajax('/promise/api/user.json').then(res => {
  console.log('then1', res[0]) // then1 {name: "zhang", age: 17}
  return ajax('/promise/api/user.json') // 这里的返回值作为下一个then方法回调的参数，如果返回的是Promise，那后面then方法的回调会等待它的结束
}) // 这里Promise对象的then方法会返回一个全新的Promise对象
  .then(res => { // 这里的then方法是在为上一个then返回的Promise注册回调
    console.log('then2', res[1]) // then2 {name: "li", age: 18}
    return ajax('/promise/api/user.json')
  })
  .then(res => {
    console.log('then3', res[2]) // then3 {name: "wang", age: 19}
    return ajax('/promise/api/user.json')
  })
```

## Promise异常
1. 全局对象上注册unhandledrejection事件，去处理代码中没有手动捕获到的异常
    ```js
    window.addEventListener('unhandledrejection', event => {
    
    const { reason, promise } = event
    console.log(reason, promise)
    // reason => Promise失败原因，一般是一个错误对象
    // promise => 出现异常的Promise对象
    
    event.preventDefault()
    })
    ```
2. Node中的Promise全局异常处理
    ```js
    process.on('unhandledrejection', (reason, promise) => {
      console.log(reason, promise)
      // reason => Promise失败原因，一般是一个错误对象
      // promise => 出现异常的Promise对象
    })
    ```

## Promise静态方法
```js
const p1 = Promise.resolve('foo') // 静态方法
const p2 = new Promise((resolve, rejected) => { 
  resolve('foo')
})

p1.then(res => {
  console.log(res) // foo
})

p2.then(res => {
  console.log(res) // foo
})
```
```js
const p1 = ajax('/promise/api/user.json')
const p2 = Promise.resolve(p1)

console.log(p1 === p2) // true
```

可以把第三方的Promise对象转成原生的Promise对象
```js
Promise.resolve({
  then (resolve, rejected) {
    resolve('foo')
  }
}).then(res => {
  console.log(res)
})
```

## Promise 并行执行
Promise.all([promise1, promise2])  
例：请求列表中的接口
```js
ajax('/promise/api/url.json')
.then(value => {
  const urls = Object.values(value)
  const tasks = urls.map(url => ajax(url))
  return Promise.all(tasks)
}).then(values => {
  console.log(values)
})
```

用race实现请求自定义超时效果
```js
const request = ajax('/promise/api/user.json')
const timeout = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject(new Error('time out'))
  }, 100)
})

Promise.race([request, timeout]).then(res => {
  console.log(res)
}).catch(err => {
  console.log(err)
})
```

## Promise 执行时序
`setTimeout` 宏任务  
`Promise` 微任务

## Generator
