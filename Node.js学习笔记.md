# Node.js学习笔记

## Node基础
### Node三大模块
#### 全局模块
#### 系统模块
1. **path：** 用于处理文件路径和目录路径的实用工具
    ```js
    const path = require('path')
    
    console.log(path.dirname('/node/a/b/c/1.jpg')) // /node/a/b/c
    console.log(path.basename('/node/a/b/c/1.jpg')) // 1.jpg
    console.log(path.extname('/node/a/b/c/1.jpg')) // .jpg
    
    console.log(path.resolve(__dirname, 'index.js')) // 打印文件的绝对路径，resolve方法可以有多个参数，可以把多个路径处理成最终的路径
    console.log(path.resolve('/node/a/b/c', '../../', 'd')) // /node/a/d
    ```

1. **fs：** 用于文件的读写操作
    ```js
    const fs = require('fs')
    
    // 异步读取文件，同步用readFileSync，同步无回调函数，直接赋值即可
    fs.readFile('./a.txt', (err, data) => { // a.txt abc
      if (err) {
        console.log(err)
      } else {
        console.log(data) // <Buffer 61 62 63>
        console.log(data.toString()) // abc
      }
    })
    
    // 异步写入文件，同步用writeFileSync，同步无回调函数，直接赋值即可
    // 覆盖模式
    fs.writeFile('b.txt', '月薪1元', (err) => {
      if (err) {
        throw err
      }
    })
    // 追加模式
    fs.writeFile('b.txt', '月薪1元', { flag: 'a' }, (err) => {
      if (err) {
        throw err
      }
    })
    ```

1. **自定义模块：** require自己封装的模块
    - exports
        ```js
        exports.a = 1
        exports.b = 2
        ```
    
    - module
    1. example1:
        ```js
        module.exports = {
          a: 1, 
          b: 2
        }
        ```
    
    1. example2:
        ```js
        // mod.js
        module.exports = function() {
          console.log('yes')
        }
        
        // index.js
        const mod1 = require('./mod.js')
        mod1() // yes
        ```
    
    1. example3:
        ```js
        // mod.js
        module.exports = class {
          constructor (name) {
            this.name = name
          }
          show () {
            console.log(this.name)
          }
        }
        
        // index.js
        const mod1 = require('./mod.js')
        const p = new mod1('zhang')
        p.show() // zhang 
        ```
    - require

# http模块
