#### css in js方案之styled-componented

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