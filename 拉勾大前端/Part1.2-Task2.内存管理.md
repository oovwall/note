# 内存管理

## 内存管理
- `内存`：由可读写单元组成，表示一片可操作空间
- `管理`：人为的去操作一片空间的申请、使用和释放
- `内存管理`：开发者主动申请空间、使用空间、释放空间
- `管理流程`：申请-使用-释放

```js
// 申请
let obj = {}
// 使用
obj.name = 'lg'
// 释放
obj = null
```

## Javascript 中的垃圾
- Javascript中内存管理是自动的
- 对象不再被引用时是垃圾
- 对象不能从根上访问到时是垃圾

```js
let obj = { name: 'zhang' }
let ali = obj
obj = null
console.log(obj) // null
console.log(ali) // { name: 'zhang' }

```

## Javascript中的可达对象
- 可以访问到的对象就是可达对象（引用、作用域链）
- 可达的标准就是从根出发是否能够被找到
- Javascript中的根就可以理解为全局变量对象

```js
function objGroup (obj1, obj2) {
  obj1.next = obj2
  obj2.prev = obj1

  return {
    o1: obj1,
    o2: obj2
  }
}


let obj = objGroup({ name: 'obj1' }, { name: 'obj2' })
console.log(obj)

// { o1: { name: 'obj1', next: { name: 'obj2', prev: [Circular] } },
//  o2: { name: 'obj2', prev: { name: 'obj1', next: [Circular] } } }
```

## GC定义与作用
- GC就是垃圾回收机制的简写 *Garbage Collection*
- GC可以找到内存中的垃圾、并释放和回收空间

### GC算法是什么
- GC是一种机制， 垃圾回收器完成具体的工作
- 工作的内容就是查找垃圾释放空间、回收空间
- 算法就是工作时查找和回收所遵循的规则

### 常见GC算法
- 引用计数
- 标记清除
- 标记整理
- 分代回收

#### 引用计数算法
##### 引用计数算法优点
- 发现垃圾时立即回收
- 最大限度减少程序暂停

##### 引用计数算法缺点
- 无法回收循环引用的对象
- 时间开销大

#### 标记清除算法
- 核心思想：分标记和清除二个阶段完成
- 遍历所有对象找标记活动对象
- 遍历所有对象清除没有标记对象
- 回收相应的空间

