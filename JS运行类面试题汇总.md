# JS运行类面试题汇总

1. 构造函数相关
    ```js
    function Person(name) {
        this.name = name
        return name
    }
    let p = new Person('Tom')
    console.log(p)
    ```
    ```js
    function Person(name) {
        this.name = name
        return {}
    }
    let p = new Person('Tom')
    console.log(p)
    ```

2. 宏任务与微任务
    ```js
    console.log('a')
    setTimeout(() => {
      console.log('b')
    }, 0)
    console.log('c')
    Promise.resolve().then(() => {
      console.log('d')
    })
      .then(() => {
        console.log('e')
      })
    
    console.log('f')
    ```
