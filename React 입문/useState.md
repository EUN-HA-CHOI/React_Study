## useState  

버튼을 누르면 숫자가 바뀌는 Counter 컴포넌트를 만들어 본다.  
```
import React from 'react';

function Counter() {
  return (
    <div>
      <h1>0</h1>
      <button>+1</button>
      <button>-1</button>
    </div>
  );
}

export default Counter;
```

```
import React from 'react';
import Counter from './Counter';

function App() {
  return (
    <Counter />
  );
}

export default App;
```

![image](https://user-images.githubusercontent.com/97012561/197100155-77f35fa5-9ce6-4cf6-adb4-ff4d35db34d1.png)

Counter 에서 버튼이 클릭되는 이벤트가 발생 했을 때,특정 함수가 호출되도록 설정.  

```
import React from 'react';

function Counter() {
  const onIncrease = () => {
    console.log('+1')
  }
  const onDecrease = () => {
    console.log('-1');
  }
  return (
    <div>
      <h1>0</h1>
      <button onClick={onIncrease}>+1</button>
      <button onClick={onDecrease}>-1</button>
    </div>
  );
}

export default Counter;
```
onIncrease 와 onDecreases 화살표 함수를 이용하여 구현  
함수 만들고, button의 onClick으로 함수를 연결 해 준다.  

**리액트에서 엘리먼트에 이벤트를 설정해줄때에는 on이벤트이름={실행하고싶은함수} 형태로 설정해주어야 함.**

<hr>
동적인 값인 useState 함수를 사용하여 컴포넌트 상태를 관리한다.  
```
import React, { useState } from 'react';

function Counter() {
  const [number, setNumber] = useState(0);

  const onIncrease = () => {
    setNumber(number + 1);
  }

  const onDecrease = () => {
    setNumber(number - 1);
  }

  return (
    <div>
      <h1>{number}</h1>
      <button onClick={onIncrease}>+1</button>
      <button onClick={onDecrease}>-1</button>
    </div>
  );
}

export default Counter;
```

useState 를 사용 할 때에는 상태의 기본값을 파라미터로 넣어서 호출 해 준다.  
이 함수를 호출해주면 배열이 반환되는데, 첫 번째 원소는 현재상태, 두번째는 Setter 함수이다.  

**배열 비구조화 할당**을 사용한 것.  
```
  const onIncrease = () => {
    setNumber(number + 1);
  }

  const onDecrease = () => {
    setNumber(number - 1);
  }
```
Setter 함수는 파라미터로 전달 받은 값을 최신 상태로 설정  
`  <h1>{number}</h1>`





