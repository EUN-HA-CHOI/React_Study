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

<hr>

* 함수형 업데이트      

```
import React, { useState } from 'react';

function Counter() {
  const [number, setNumber] = useState(0);
  
  const onIncrease = () => {
    setNumber(prevNumber =>prevNumber +1);
  }
  const onDecrease = () => {
    setNumber(prevNumber =>prevNumber -1);
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

onIncrease 와 onDecrease 에서 setNumber 를 사용 할 때 그 다음 상태를 파라미터로 넣어준것이 아니라, 값을 업데이트 하는 함수를 파라미터로 넣어주었습니다.   

* **prevNumber 은 선언한 적이 없는데 어디서 나타난 것일까?**    

useStare 함수를 개발한 개발자가 정한 것    
`const [number, setNumber] = useState(0);`    
이렇게 했을 때, setState란 함수에 파라미터로 함수를 넘겨주면 이전 값을 넣어주는 걸로 개발이 된 것, 콜백함수를 잘 알아놓자!  

`prevNumber => prevNumber + 1`  
이건 우리가 임의로 정의한 함수라서 파라미터명을 어떤 걸로 쓰던 상관 없음.      

setState 내부를 쉽게 생각해 보면 아래와 같다.   

```
let previosValue = 0;

function setState(callback) {
  previosValue = callback(previosValue);
}
```   

여기서 callback이 prevNumber => prevNumber + 1 이라고 생각하면 쉽다.   





