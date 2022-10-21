## props  

App 컴포넌트에서 Home 이라는 컴포넌트를 사용할 때 name 이라는 값을 전달해 주고 싶다고 가정.  

```react
import React from 'react'
import Home from './Home'

function App() {
  return (
    <Home name="react"/>
  )
}
export default App
-------------------------------------
Home 컴포넌트에서 name 값을 사용하려면 

import React from 'react'

function Home(props) {
  return 
    <div>안녕하세요 {props.name}</div>
}
```
컴포넌트에게 전달괴는 props는 파라미터를 통하여 알 수 있다.   
props는 객체형태로 전달됨. 또는 구조분해 할당으로 전달.  
name 값을 조회하고 싶다면 props.name을 입력하여 조회.  

