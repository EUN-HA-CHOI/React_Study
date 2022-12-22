## useMemo 와 useCallback 의 차이  

* 메모이제이션(memoization)  
useMemo 함수에 대해서 알아보기 전에 메모이제이션(memoization) 개념에 대해 알아 보겠다.  
**메모이제이션(memoization)** 이란 기존에 수행한 연산의 결괏값을 어딘가에 저장 해 두고 동일한 입력이 들어오면 재활용 하는 프로그래밍 기법을 말한다. 메모이제이션(memoization)을 잘 적용하면 중복 연상을 피할 수 있기 때문에 메모리를 조금 더 쓰더라도 애플리케이션의 성능을 최적화 할 수 있다.  

* useMemo  
메모이제이션된 '값'을 반환한다.  
`useMeno(() => fn, deps)`   
useMemo 는 **deps 가 변한다면, () => fn 이라는 함수를 실행**하고, 그 함수의 반환 값을 반환 한다.   
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

위의 코드에서 deps 는 [ex] 이다. 즉, **ex 가 변할 때에만 ()=> {console.log(ex)} 이 실행**된다.  
따라서 **X 버튼을 누를 때에만 콘솔창에 ex 값이 출력** 된다.  
Y 버튼을 누르더라도 APP 이라는 함수 컴포넌트가 전부 재실행(리랜더링)되지만, ex 라는 값은 변하지 않았기 때문에 useMemo 에는 변화가 없다.  

* useCallback  
메모이제이션된 '함수'를 반환한다.  

`useCallback(fn, deps)`   
useCallback 은 **deps 가 변한다**면, **fn 이라는 새로운 함수를 반환**한다.   

```
import React, { useState, useCallback, useMemo } from "react";

export default function App() {
  const [ex, setEx] = useState(0);
  const [why, setWhy] = useState(0);

  // useCallback 이 () => {console.log(why)} 라는 함수를 반환한다.
  const useCallbackReturn = useCallback(() => {console.log(why)}, [ex]);

  // useCallback 이 담겨있는 함수를 실행
  useCallbackReturn()

  return (
    <>
      <button onClick={() => setEx((curr) => (curr + 1))}>X</button>
      <button onClick={() => setWhy((curr2) => (curr2 + 1))}>Y</button>
    </>
  );
}
```

첫 예시와 같은 버튼을 클릭할 때 이벤트가 발생되는 코드이다. 위의 **useCallback 은 () => {console.log(why)} 라는 함수를 반환** 해 주고 있다. 위의 useCallback은 다음의 순서로 진행된다.    

1. 처음 컴포넌트가 시작될 때 실행 () => {console.log(0)}
2. ex 가 변할 때까지 함수는 () => {console.log(0)}
3. ex 가 변한다면 그제서애 why 의 값을 가져와서 () => {console.log(새로운값)}

예를 들어 Y 버튼을 다섯번 누른다고 가정 해 보자. Y 버튼이 눌리면 'why' 라는 상태의 값이 1씩 증가 한다.  

Y 를 다섯번 누를 때 동안에는 계속 함수가 **() => {console.log(0)}** 이다. (why 라는 상태값을 계속 값이 증가한다.)    
그러다가 **X 버튼**을 누르면서 ex 라는 변수 (deps)가 변하자마자, **() => {console.log(5)}** 를 반환된다.  
**deps 가 변해야 함수 컴포넌트와 상태 값(why)을 공유**하는 것이다.   

따라서 useCallback은 함수와는 상관없는 상태 값이 변할 때, **함수 컴포넌트에서 불필요하게 함수를 업데이트 하는 것을 방지** 해 준다.    
다만, deps를 잘 못 설정하면 아무리 함수 컴포넌트를 재실행해도 함수가 변하지 않으면서 내가 원치 않는 상황이 올 수도 있으므로 **섬세한 컨트롤**이 필요하다.  
이러한 상황은 **useMemo** 도 똑같다. useMemo 역시 Y 버튼을 5번 눌러도, X 버튼을 안 누르면 실행이 되지 않는다.   

아래의 두 식은 같은 것이다.  
`useMemo((...)=> fn, deps) === useCallback(fn, deps)`  
