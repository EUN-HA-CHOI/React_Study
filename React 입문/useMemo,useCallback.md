## useMemo 와 useCallback 의 차이  

* 메모이제이션(memoization)  
useMemo 함수에 대해서 알아보기 전에 메모이제이션(memoization) 개념에 대해 알아 보겠다.  
**메모이제이션(memoization)** 이란 기존에 수행한 연산의 결괏값을 어딘가에 저장 해 두고 동일한 입력이 들어오면 재활용 하는 프로그래밍 기법을 말한다. 메모이제이션(memoization)을 잘 적용하면 중복 연상을 피할 수 있기 때문에 메모리를 조금 더 쓰더라도 애플리케이션의 성능을 최적화 할 수 있다.  

* useMemo  
메모이제이션된 '값'을 반환한다.  
`useMeno(() => fn, deps)`   
useMemo 는 deps 가 변한다면, () => fn 이라는 함수를 실행하고, 그 함수의 반환 값을 반환 한다.   
deps는 dependency 이며, useMemo 가 이 deps 라는 것에 '의존' 한다는 뜻이다.  

ex)
```
import React, { useState, useCallback, useMemo } from "react";

export default function App() {
  const [ex, setEx] = useState(0);
  const [why, setWhy] = useState(0);

  // useMemo 사용하기
  useMemo(() => {console.log(ex)}, [ex]);

  // 두 개의 버튼을 설정했다. X버튼만이 ex를 변화시킨다.
  return (
    <>
      <button onClick={() => setEx((curr) => (curr + 1))}>X</button>
      <button onClick={() => setWhy((curr2) => (curr2 + 1))}>Y</button>
    </>
  );
}
```

위의 버튼 함수 중 X 버튼을 클릭할 때 'ex' 라는 상태값이 변화하는 코드이다.    
`useMemo(() => {console.log(ex)}, [ex]) `  
