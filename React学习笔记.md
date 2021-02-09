# React学习笔记 

## JSX语法
## 组件
### 双向数据绑定
```jsx
import React, { Component } from 'react'

const Person = props => {
  return <input type="text" value={props.name} onChange={props.changed} />
}

class App extends Component {
  constructor () {
    super()
    this.state = {
      name: '张三'
    }
    this.nameChanged = this.nameChanged.bind(this)
  }

  nameChanged (event) {
    this.setState({
      name: event.target.value
    })
  }

  render () {
    return (
      <div>
        <div>{this.state.name}</div>
        <Person name={this.state.name} changed={this.nameChanged}></Person>
      </div>
    )
  }
}

export default App
```
