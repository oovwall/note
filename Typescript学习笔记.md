# Typescript学习笔记

## 快速上手
### 编译代码
```powershell
tsc greeter.ts
```

### 类型注解
TypeScript里的类型注解是一种轻量级的为函数或变量添加约束的方式。
```typescript
// 约束参数类型为string类型
function greeter(person: string) {
    return "Hello, " + person;
}

let user = 'zhangsan';

document.body.innerHTML = greeter(user);
```

### 接口
我们在实现接口时候保证包含了接口要求的结构。
```typescript
interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = { firstName: "Jane", lastName: "User" };

document.body.innerHTML = greeter(user);
```

### 类

## 知识点
### 变量声明
```typescript
const myname: string = 'zhangs'
```
