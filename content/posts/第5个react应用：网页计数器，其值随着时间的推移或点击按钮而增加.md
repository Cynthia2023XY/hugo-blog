---
sr-due: 2024-08-21
sr-interval: 13
sr-ease: 290
share: true
tags:
  - review_fullstack_react
  - review_knowledge_code_practice
---

#review/fullstack/react 
#review/knowledge/code/practice 

## 创建一个页面计数器，其值随着时间的推移或点击按钮而增加

## 随时间推移变化
#### 改写`App.js` & `index.js`

文件`App.js`变成:
```js
const App = (props) => {
  const {counter} = props
  //通过counter，App组件的prop得到了计数器的值
  return (
    <div>{counter}</div>
  )
}

export default App
```

而文件`index.js`变成了:
```js
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

let counter = 1

ReactDOM.createRoot(document.getElementById('root')).render(
  <App counter={counter} />
)
```

```ad-seealso
title:`index.js`之前的内容是这样的
新增了
`let counter = 1`
和
`<App counter={counter} />`

---

```js
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

---
#### 通过`counter`，App组件的`prop`得到了计数器的值: 这个组件将该值渲染到屏幕上

#### 当 `counter` 的值改变时，会发生什么？

即使我们加入以下内容，该组件也不会重新渲染。
`counter += 1`
```ad-hint
title: 这里的`+=`是递增运算符，相当于`counter=counter+1`，原理是直接将右侧的值加到左侧变量的当前值上
递增运算符不仅可以起到简化代码的作用，还可以起到提高执行效率的作用：本来是先做加法再赋值，现在一次操作：直接将右侧的值加到左侧变量的当前值上
[i=i+1究竟有多少简写：复合赋值运算符+递增运算符]({{< relref "i=i+1%E7%A9%B6%E7%AB%9F%E6%9C%89%E5%A4%9A%E5%B0%91%E7%AE%80%E5%86%99%EF%BC%9A%E5%A4%8D%E5%90%88%E8%B5%8B%E5%80%BC%E8%BF%90%E7%AE%97%E7%AC%A6+%E9%80%92%E5%A2%9E%E8%BF%90%E7%AE%97%E7%AC%A6.md" >}})
```

---
#### 我们可以通过第二次调用`render`方法来让组件重新渲染
例如用以下方式：
```js
let counter = 1

const refresh = () => {
  ReactDOM.createRoot(document.getElementById('root')).render(
    <App counter={counter} />
  )
}

refresh()
counter += 1
refresh()
counter += 1
refresh()
```
重新渲染的命令已经被包裹在refresh函数中，以减少复制粘贴的代码量。

现在这个组件渲染了三次，首先是数值1，然后是2，最后是3。 
> 然而，数值1和2在屏幕上显示的时间非常短，以至于它们无法被注意到。
> 所以屏幕上只能看到一个3
> ![[Pasted image 20240801160633.png]]

---
### 通过设置`setInterval`反复执行render，反复渲染，来实现页面上随时间变化的计数器
每秒钟重新渲染并增加计数器
```js
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

let counter = 1

const refresh = () => {
  ReactDOM.createRoot(document.getElementById('root')).render(
    <App counter={counter} />
  )
}

setInterval(() => {
    refresh()
    counter += 1
  }, 1000)
```

效果：网页上的数字每秒增加1
![[Pasted image 20240801183023.png]]
[[setInterval和setTimeout区别与联系]]

---
### 重复调用render方法并不是重新渲染组件的推荐方式，使用hooks更好：useState()

`index.js`改为
```js
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

---
而`App.js`则改为
```js
import { useState } from 'react'
const App = () => {
  const [ counter, setCounter ] = useState(0)
  setTimeout( () => setCounter(counter + 1), 1000 )
  return (
    <div>{counter}</div>
  )
}

export default App
```
^e5hs9s

```ad-faq
title:import { useState } from 'react' 为什么useState要加入{}括号?为什么不能去掉大括号，直接import useState from 'react'
![默认导入和命名导入]({{< relref "%E9%BB%98%E8%AE%A4%E5%AF%BC%E5%85%A5%E5%92%8C%E5%91%BD%E5%90%8D%E5%AF%BC%E5%85%A5.md" >}}#^bbxx9r)
[默认导入和命名导入]({{< relref "%E9%BB%98%E8%AE%A4%E5%AF%BC%E5%85%A5%E5%92%8C%E5%91%BD%E5%90%8D%E5%AF%BC%E5%85%A5.md" >}})
```

---
在第一行，文件导入了`useState`函数——这就是hooks。
```js
import { useState } from 'react'
```
在函数的第一行，将`state`添加到组件中，并将其初始化为0值。
```js
const [ counter, setCounter ] = useState(0)
```

该函数`useState(0)`返回一个包含两个项目的数组。
我们通过使用前面显示的[[js解构赋值：将数组中的值或对象的属性取出，赋值给其他变量|解构赋值语法]]将这些项目赋值给变量`counter`和`setCounter`。

```ad-faq
title:这个初始状态值0代表什么呢?
初始状态值 `0` 代表组件挂载时状态 `counter` 的起始值。

**将初始状态值改为99的话，计时器就会从99开始计时**
```

---

```ad-faq
title:`setTimeout`不是只能运行一次吗？为什么可以实现持续渲染？
因为useState()`返回的第二个参数为更新状态的参数(即`setCounter`)，调用`setTimeout`即可更新App组件
（哪个组件用hooks，则哪个组件更新）

每当用于更新状态的函数setCounter被调用时，React都会重新渲染组件，这意味着组件函数的函数体被重新执行，所以`setTimeout`又运行了一次，引发一秒后新的重新渲染，又重新执行`setTimeout`——子子孙孙无穷匮也。
```

---
```ad-faq
title:这段计时器代码，如果我减少setTimeout的时间间隔，比如将1000改为10甚至更小，会导致函数不能正常运行吗？

理论上不会，但是由于 React 状态更新和 DOM 重新渲染是异步的，如果 `setTimeout` 回调执行得太快，用户可能会观察到组件渲染的**闪烁或抖动**现象。

如果时间间隔设置得非常短，比如 10 毫秒，这将导致 `setTimeout` 的回调函数非常频繁地被调用。这可能会造成大量的渲染更新，从而对性能产生负面影响
```

---
```ad-faq
title:这段计时器代码，如果我使用setInterval取代setTimeout, 可能会发生什么问题？是不是会以两倍速计时？

不会两倍速计时

实际结果：使用 `setInterval` 替换 `setTimeout` 后，计时器正常一倍速运行一段时间后开始频繁闪烁且显示的数字开始不对，变成不停闪烁的数字乱码

import { useState } from 'react'
const App = () => {
  const [ counter, setCounter ] = useState(0)
  setInterval( () => setCounter(counter + 1), 1000 )
  return (
    <div>{counter}</div>
  )
}

export default App

推测应该是setInterval的循环触发和setCounter的渲染间隔刚好撞一起了，导致了一些函数更新的混乱
```

---

## 随点击按钮增加

### 增加事件处理函数Event handling`handleClick`

### 1每点击一下就在控制台打印clicked
```js
const App = () => {
  const [ counter, setCounter ] = useState(0)

  const handleClick = () => {
    console.log('clicked')
  }

  return (
    <div>
      <div>{counter}</div>

      <button onClick={handleClick}>
        plus
      </button>
    </div>
  )
}
```

---
### 2直接把事件处理函数写到`<button>`的`onClick`中
```js
const App = () => {
  const [ counter, setCounter ] = useState(0)

  return (
    <div>
      <div>{counter}</div>

      <button onClick={() => console.log('clicked')}>
        plus
      </button>
    </div>
  )
}
```

---
### 3添加useState()的更新
```js
import { useState } from "react";

const App = () => {
  const [ counter, setCounter ] = useState(0)

  return (
    <div>
      <div>{counter}</div>

      <button onClick={() => setCounter(counter + 1)}>
        plus
      </button>
    </div>
  )
}

export default App;
```

{{< figure src="/images/Pasted image 20240808193320.png"  width="100" height="">}}

---

### 4增加一个按钮: 归零计数器
```js {hl_lines=["11-13"]}
const App = () => {
  const [ counter, setCounter ] = useState(0)

  return (
    <div>
      <div>{counter}</div>

      <button onClick={() => setCounter(counter + 1)}>
        plus
      </button>
      <button onClick={() => setCounter(0)}>
        zero
      </button>
    </div>
  )
}
```

{{< figure src="/images/Pasted image 20240808194219.png"  width="100" height="">}}

---

#### 为什么不能写成`<button onClick={setCounter(counter + 1)}>plus</button`？非得用个箭头函数嘛？

`<button onClick={() => setCounter(counter + 1)}>plus</button`
为什么不能写成
`<button onClick={setCounter(counter + 1)}>plus</button`？

因为`() => setCounter(counter + 1)`是一个**函数引用**，在按钮点击的时候才执行

而`setCounter(counter + 1)`是一个**函数调用**，在当React第一次渲染组件时，它执行了函数调用setCounter(0+1)，并将组件的状态值改为1。

这将导致该组件被重新渲染，React将再次执行setCounter函数调用，并且状态将改变，导致再次重新渲染......
```ad-faq
title:为什么箭头函数可以将函数的执行延迟到实际点击事件发生时？
因为箭头函数在 JavaScript 中是**定义函数**的一种方式，而不是立即执行函数。
`<button onClick={() => setCounter(counter + 1)}>plus</button`

实际上相当于引用了预先定义好的一个函数
```js
const increaseByOne = () => setCounter(counter + 1)
<button onClick={increaseByOne}>plus</button>
```

---

### 5给`增加按钮`和`归零按钮`都分配对应的函数
`increaseByOne`
`setToZero`
```js
import { useState } from "react";

const App = () => {
  const [ counter, setCounter ] = useState(0)


  const increaseByOne = () => setCounter(counter + 1)
  const setToZero = () => setCounter(0)

  return (
    <div>
      <div>{counter}</div>

      <button onClick={increaseByOne}>
        plus
      </button>

      <button onClick={setToZero}>
        zero
      </button>
    </div>
  )
}

export default App;
```

---

### 6重构应用，使其由三个小的组件组成，一个组件用于显示计数器，两个组件用于按钮: React的状态提升-单向数据流思想

1个父组件：`App`
3个子组件：`Display`, `increaseByOne button`, `setToZero button`
![[Drawing 2024-08-14 10.29.27.excalidraw]]
这样单向的数据流动借助了[[React的状态提升]]，体现了[[React单向数据流]]的设计思想

---

```js
import { useState } from "react";

const Display = (props) => {
  return (
    <div>{props.counter}</div>
  )
}

const App = () => {
  const [ counter, setCounter ] = useState(0)


  const increaseByOne = () => setCounter(counter + 1)
  const setToZero = () => setCounter(0)

  return (
    <div>
      <Display counter={counter}/>

      <button onClick={increaseByOne}>
        plus
      </button>

      <button onClick={setToZero}>
        zero
      </button>
    </div>
  )
}

export default App;
```

---

### 7把button组件也改成自定义的react组件

```js
import { useState } from "react";

const Display = (props) => {
  return (
    <div>{props.counter}</div>
  )
}
const Button = (props) => {
  return (
    <button onClick={props.onClick}>
      {props.text}
    </button>
  )
}

const App = () => {
  const [ counter, setCounter ] = useState(0)


  const increaseByOne = () => setCounter(counter + 1)
  const decreaseByOne = () => setCounter(counter - 1)
  const setToZero = () => setCounter(0)

  return (
    <div>
      <Display counter={counter}/>
      <Button onClick={increaseByOne} text='plus'/>
      <Button onClick={setToZero} text='zero'/>
      <Button onClick={decreaseByOne} text='minus'/>
    </div>
  )
}

export default App;
```


![[Pasted image 20240814104838.png|144]]

---

### 8还能简化：使用解构代替props来简化组件
[[js解构赋值：将数组中的值或对象的属性取出，赋值给其他变量]]
改成这样即可
```js
const Display = ({counter}) => {
  return (
    <div>{counter}</div>
  )
}
const Button = ({onClick,text}) => {
  return (
    <button onClick={onClick}>
      {text}
    </button>
  )
}
```

```ad-attention
title:小心不要过于简化组件，因为这会增加复杂性，增大日后维护难度
```

---
