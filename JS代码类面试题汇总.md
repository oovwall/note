# JS代码类面试题汇总

## 运行类型题
1. **类型判断**
    ```js
    console.log(typeof undefined);      //undefined
    console.log(typeof 'abc');  //string
    console.log(typeof 123);    //number
    console.log(typeof true);   //boolean
    console.log(typeof {});     //object
    console.log(typeof []);     //object
    console.log(typeof null);       //object
    console.log(typeof console.log);        //function
    ```
   
1. **构造函数相关**
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

1. **宏任务与微任务**
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
   
1. **写出运行结果**  
    ```js
    let a = {
      n: 10
    }
    let b = a
    b.m = b = {
      n: 20
    }
    console.log(a)
    console.log(b)
    ```
   
1. **写出运行结果**  
    ```js
    let x = [12, 23]
    
    function fn (y) {
      y[0] = 100
      y = [100]
      y[1] = 200
      console.log(y)
    }
    
    fn(x)
    console.log(x)
    ```
   
1. **写出运行结果**
    ```js
    var foo = 1
    function bar () {
      if (!foo) {
        var foo = 10
        console.log(foo)
      }
    }
    bar()
    ```
   
1. **写出运行结果**
    ```js
    var n = 0
    function a () {
      var n = 10
      function b () {
        n++
        console.log(n)
      }
      b()
      return b
    }
    var c = a()
    c()
    console.log(n)
    ```
   
1. **写出运行结果**
    ```js
    var a = 10, b = 11, c = 12
    function test (a) {
      a = 1
      var b = 2
      c = 3
    }
    test(10)
    console.log(a)
    console.log(b)
    console.log(c)
    ```
   
1. **写出运行结果**
    ```js
    if (!('a' in window)) {
      var a = 1
    }
    
    console.log(a)
    ```
   
1. **写出运行结果**
    ```js
    var a = 4
    function b (x, y , a) {
      console.log(a)
      arguments[2] = 10
      console.log(a)
    }
    a = b(1, 2, 3)
    console.log(a)
    ```
   
1. **写出运行结果**
    ```js
    function fn (x, y) {
      x = 100
      console.log(x)
      arguments[1] = 200
      console.log(y)
    }
    fn(10)
    ```
   
1. **写出运行结果**
    ```js
    'use strict'
    function fn (x) {
      x = 100
      console.log(x)
      console.log(arguments[0])
    }
    fn(10)
    ```
   
1. **写出运行结果**
    ```js
    var foo = 'hello'
    ;(function (foo) {
      console.log(foo)
      var foo = foo || 'world'
      console.log(foo)
    })(foo)
    console.log(foo)
    ```
   
1. **写出运行结果**
    ```js
    var a = {
      x: 1,
      y: 2
    }
    function fn (a) {
      a.x = 100
    }
    fn(a)
    console.log(a)
    ```
   
1. **写出运行结果**
    ```js
    var a = 9
    function fn () {
      a = 0
      return function (b) {
        return b + a++
      }
    }
    
    var f = fn()
    console.log(f(5))
    console.log(fn()(5))
    console.log(f(5))
    console.log(a)
    ```

1. **写出运行结果**
    ```js
    var ary = [1, 2, 3, 4]
    function fn (ary) {
      ary[0] = 0
      ary = [0]
      ary[0] = 100
      return ary
    }
    var res = fn(ary)
    console.log(ary)
    console.log(res)
    ```
   
1. **写出运行结果**
    ```js
    var a = 1
    var obj1 = {
      a:2,
      fn:function(){
        console.log(this.a)
      }
    }
    var fn1 = obj1.fn
    obj1.fn() // 2
    fn1() // 1
    ```
   
1. **写出运行结果**
    ```js
    f = function () {
      return true
    }
    
    g = function () {
      return false
    }
    
    ;~function () {
      if (g() && [] == ![]) {
        f = function () {
          return false
        }
        function g () {
          return true
        }
      }
    }()
    
    console.log(f())
    console.log(g())
    ```

1. **写出运行结果**
    ```js
    let obj = {
      a: 1,
      f () {
        return this.a
      }
    }
    const r = obj.f.bind({ a: 2 }).call({ a: 4 })
    console.log(r)
    ```

1. **写出运行结果**
    ```js
    const person = { name: 'Lydia' }
    
    function sayHi (age) {
      console.log(`${this.name} is ${age}`)
    }
    
    sayHi.call(person, 21)
    sayHi.bind(person, 21)
    
    console.log(sayHi.call(person, 21) === sayHi.bind(person, 21)())
    ```

1. **写出运行结果**
    ```js
    function greeting () {
      throw 'Hello World'
    }
    
    function sayHi () {
      try {
        const data = greeting()
        console.log('It worked!', data)
      } catch (e) {
        console.log('an error', e)
      }
    }
    
    sayHi()
    ```

1. **写出运行结果**
    ```js
    async function getData () {
      return await Promise.resolve('I made it !')
    }
    
    const data = getData()
    console.log(data)
    
    ```
   
1. **写出运行结果**
    ```js
    const add = x => y => z => {
      console.log(x, y, z)
      return x + y + z
    }
    
    add(4)(5)(6)
    ```

1. **写出运行结果**
    ```js
    const foo = () => console.log('First')
    const bar = () => setTimeout(() => console.log('Second'))
    const baz = () => console.log('Third')
    
    bar()
    foo()
    baz()
    ```
   
1. **写出运行结果**
1. **写出运行结果**
1. **写出运行结果**

## 开放解决方案类型题
1. **如何准确判断一个变量是数组类型**
    ```js
   var arr = [];
   
   console.log(arr instanceof Array);
   console.log(Array.isArray(arr))
   ```


1. **5点15分，时钟和分钟的夹角？**
    ```js
    function angle (hour, min) {
      let hourAngle = 360 / 12 * hour + 360 / 12 * min / 60
      let minAngle = 360 / 60 * min
      return Math.abs(hourAngle - minAngle)
    }
    
    console.log(angle(5, 15))
    ```
1. **写一个原型链继承的例子**
    ```js
    function Elem (ele) {
      this.ele = document.getElementById(ele)
    }
    
    Elem.prototype.html = function (html) {
      var ele = this.ele
    
      if (html) {
        ele.innerHTML = html
        return this
      } else {
        return ele.innerHTML
      }
    }
    
    Elem.prototype.on = function (type, fn) {
      var ele = this.ele
    
      ele.addEventListener(type, fn)
      return this
    }
    
    var v1 = new Elem('div1')
    
    v1.html('hello!!!1').on('click', function () {
      alert('你好')
    })
    ```
   
1. **用原生JS实现ES6的模板字符串**
    ```js
    
    ```

1. **大数相加方法**
