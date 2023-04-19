* styled-componented

1.定义
```js
import styled from 'styled-components'

const BasePlayerWrapper = styled.div`
    border: 1px solid #ccc;
    height: 80px;
    background-color: ${props => props.color};   // 动态更改颜色
    .inner{
        height: 10px;
        background-color: #ccc;
    }
`

export default BasePlayerWrapper
```

2.使用
```js
import React from 'react'
import BasePlayerWrapper from './base-play-style'

export default function BasePlayer({ color}) {
  return (
      <BasePlayerWrapper className='BasePlayer' color={color}>
          <div className="inner"></div>
      </BasePlayerWrapper>
  )
}
```

> 优点：
>  * 内联样式方便而且易于维护
>  * 支持JavaScript直接控制样式属性
>  * 高度可定制化，可以灵活应对样式变化
>
> 缺点：
>  * 学习成本和上手难度较高
>  * 可能会影响性能和代码大小
>  * 与CSS文件不兼容，可能需要进行转换


* 内联样式

```js
const styles = {
  backgroundColor: 'blue',
  fontSize: '20px',
  color: 'white'
}

function App() {
  return (
    <div style={styles}>
      Hello World
    </div>
  )
}
```

> 优点：
>  * 方便快捷，不需要引入CSS文件
>  * 可以直接使用JavaScript来控制样式的一些属性
>  * 方便利用props、state等数据动态改变样式
>
> 缺点：
>  * 不易维护和修改，样式与组件代码耦合在一起，不易复用
>  * 没有类名的概念，无法方便地复用样式


* 直接使用css单文件

```js
/* App.css */
.container {
  background-color: blue;
  font-size: 20px;
  color: white;
}

/* App.js */
import './App.css';

function App() {
  return (
    <div className="container">
      Hello World
    </div>
  )
}
```

> 优点：
>  * 方便维护和修改，样式与组件代码分离，易于多处复用
>  * 支持CSS的各种功能，如伪类、媒体查询等
>  * 可以集成less和sass
>
> 缺点：
>  * 引入样式的方式在组件数量增多时会变得不够优雅
>  * 无法直接使用JavaScript来控制样式的一些属性