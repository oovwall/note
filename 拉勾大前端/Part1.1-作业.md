# Javascript深度剖析 模块一作业

1. 执行结果为10，因为var声明的i为全局变量，i最后满足条件循环完+1，所以全局变量i=10，而数组a的每个元素都是打印全局变量i的值，所以最后输出为`10`

1. **报错。**
   因为在ES2015中，每个花括号中就是一个块级作用域。如果在这个作用域使用相同名称的变量必须先声明后使用。

1. 
    ```js
    Math.min(...[12, 34, 32, 89, 4]) // 4
    ```

1. `var`有变量提升的作用，只要全局作用域或函数作用域中有该变量不管在什么位置都会提前声明。`let`和`const`是ES2015中才有的语法，相较于以前的js，增加了块级作用域。`const`只能声明常量，不能更改，且声明时必须初始化值。`let`用于声明变量，可以先声明后初始化。

1. `20`。这题考的this指向，obj.fn()是obj调用该函数，fn函数内的this指向obj本身。setTimeout里的function用的箭头函数，而箭头函数不影响this的指向，所以this.a还是指向obj自身的属性a为20。

1. Symbol最主要的作用就是为对象添加一个独一无二的属性名。Symbol还可以实现对象的私有成员。

1. 浅拷贝是指只复制了被拷贝对象的内存地址，如果该内存地址的内容发生变化，那指向该内存地址的对象内容也会发生变化。深拷贝则拷贝了被拷贝对象的内容，会生成一个新的内存地址，拷贝对象与被拷贝对象之间任何一个的内容发生变化，另一个均不受影响。

1. Event Loop是事件循环，它一直在任务队列中查找新的事件并执行。宏任务是在任务队列中排列的任务，setTimeout是宏任务；微任务是执行完当前任务就立即执行的任务，promise是微任务。

1. 
    ```js
    const greeting = function (init, str) {
      return new Promise(((resolve, reject) => {
        setTimeout(() => {
          resolve( init + str)
        }, 10)
      }))
    }
    
    greeting('', 'Hello ')
      .then((res) => {
        return greeting(res, 'Lagou ')
      })
      .then((res) => {
        return greeting(res, 'I love you ')
      })
      .then(res => {
        console.log(res) // Hello Lagou I love you 
      })
    ```

1. Typescript是javascript的超集，一些新的ECMAScript未被标准化的新特征在Typescript中先投入使用，Typescript需要编译成javascript才能变被正常运行。

1. 
- 优点：Typescript是强类型的语言，更加规范和严谨。
- 缺点：要写的代码比直接写js要多，且需要编译配置，相对于小项目来说比较麻烦点。
