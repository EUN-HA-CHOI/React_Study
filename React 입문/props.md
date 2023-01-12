## props  

* props 를 통해 컴포넌트에게 값 전달하기  
props는 properties 의 줄임말이다. 어떠한 값을 컴포넌트에게 전달해줘야 할 때, props 를 사용한다.  

* props 의 기본 사용법  
예를 들어, App 컴포넌트에서 Hello 컴포넌트를 사용할 때 name 이라는 값을 전달해주고 싶다고 가정해보자, 

```
import React from 'react';
import Hello from './Hello';

function App() {
  return (
    <Hello name="react" />
  );
}

export default App;
```

이제, Hello 컴포넌트에서 name 값을 사용 하고 싶을 땐 어떻게 하면 되는가?  

```
import React from 'react';

function Hello(props) {
  return <div>안녕하세요 {props.name}</div>
}

export default Hello;
```

컴포넌트에게 전달되는 props 는 파라미터를 통하여 조회 할 수 있다. props 는 객체 형태로 전달되며, 만약 name 값을 조회하고 싶다면 props.name 을 조회 하면 된다.   

* 여러개의 props, 비구조화 할당 
Hello 컴포넌트에 또 다른 props 를 전달 해 보자. color 라는 값을 설정해보자.  

```
import React from 'react';
import Hello from './Hello';

function App() {
  return (
    <Hello name="react" color="red"/>
  );
}

export default App;
```

그 다음에는, Hello 컴포넌트에서 color 값을 조회해서 폰트의 색상으로 설정을 해 보자.  

```
import React from 'react';

function Hello(props) {
  return <div style={{ color: props.color }}>안녕하세요 {props.name}</div>
}

export default Hello;
```

props 내부의 값을 조회 할 때마다 props. 를 입력 하고 있다. 함수의 파라미터에서 비구조화 할당 문법을 사용하면 조금 더 코드를 간결하게 작성 할 수 있다.  

```
import React from 'react';

function Hello({ color, name }) {
  return <div style={{ color }}>안녕하세요 {name}</div>
}

export default Hello;
```


* props.children    
컴포넌트 태그 사이에 넣은 값을 조회하고 싶을 땐, props.children 을 조회하면 된다.  
이번에, props.children 을 사용하는 새로운 컴포넌트를 만들어보겠다.  

```
import React from 'react';

function Wrapper() {
  const style = {
    border: '2px solid black',
    padding: '16px',
  };
  return (
    <div style={style}>

    </div>
  )
}

export default Wrapper;
```

이 컴포넌트를 App 에서 사용 해 보자   

```
import React from 'react';
import Hello from './Hello';
import Wrapper from './Wrapper';

function App() {
  return (
    <Wrapper>
      <Hello name="react" color="red"/>
      <Hello color="pink"/>
    </Wrapper>
  );
}

export default App;
```

브라우저 상에서는 Hello 컴포넌트들은 보여지지 않을거다.       
내부의 내용이 보여지게 하기 위해서는 Wrapper 에서 props.children 을 렌더링해주어야 한다.  

```
import React from 'react';

function Wrapper({ children }) {
  const style = {
    border: '2px solid black',
    padding: '16px',
  };
  return (
    <div style={style}>
      {children}
    </div>
  )
}

export default Wrapper;
```


<hr />



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

