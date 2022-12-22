## React Hook  

* 리액트 훅 함수란?  
함수형 컴포넌트에서도 클래스형 컴포넌트의 기능을 사용할 수 있게 하는 기능이다.  
훅을 통해 함수형 컴포넌트에서도 컴포넌트 상태값을 관리 할 수 있고, 컴포넌트의 생명 주기 함수를 이용할 수 있다.    

* 장점  
재사용 가능한 로직을 쉽게 만든다. 훅이 단순한 함수이므로 함수 안에서 다른 함수를 호출하는 것으로 새로운 훅과 다른 사람들이 만든 여러 커스텀 훅을 조립하여 쉽게 훅을 만들 수 있다.   
같은 로직을 한곳으로 모을 수 있어 가독성이 좋다.   

* 기본 Hook 
  * useState.  
  * useEffect. 
  * useContext.   


* 추가 Hook  
  * useReducer.   
  * useCallback.   
  * useMemo.   
  * useRef.   
  * useImperativeHandle.   
  * useLayoutEffect.  
  * useDebugValue.   

* 사용 규칙   
  * 하나의 컴포넌트에서 훅을 호출하는 순서는 항상 같아야 한다. -> 내부적으로 훅 처리를 호출된 순서로 관리한다.   
  * 함수형 컴포넌트, 커스텀 훅 안에서만 호출되어야 한다.    

<hr>

* useState.    
함수형 컴포넌트에서 state 를 사용할 수 있게 해 준다. 증가,감소 액션에 따라 state가 바뀌고 업데이트 된다.  

* useEffect   
useEffect 훅을 이용하면 비슷한 기능을 한곳으로 모을 수 있어서 가독성이 좋아진다.  

```
function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `업데이트 횟수:${count}`;
  });

  const onClickIncrement = () => {
    setCount(count + 1);
  };

  return (
    <div className="App">
      <div>{count}</div>
      <button onClick={onClickIncrement}> 증가</button>
    </div>
  );
}
```
증가를 클릭 할때마다 랜더링이 다시되고, 랜더링이 끝나면  useEffect 콜백함수가 호출 된다.  
useEffect는 랜더링 할때마다 콜백함수가 호출되기 때문에 불필요한 호출을 방지하기 위해 두번째 매개변수로 배열을 입력하고 ,   
배열의 값이 변경되는 경우에만 useEffect 매개변수로 전달 콜백함수가 호출된다.    

```
  useEffect(() => {
    document.title = `업데이트 횟수:${count}`;
  }, [count]);//count가 변경시에만 콜백함수가 호출된다.
```

이벤트 함수 등록하고 해제하기   

```
function App() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const onResize = () => {
      setWidth(window.innerWidth);
    };

    window.addEventListener('resize', onResize);

    return () => {//1
      window.removeEventListener('resize', onResize);
    };
  }, []);

  return <div className="App">{`Width is ${width}`}</div>;
}

```
  
useEffect 두번째 매개변수로 빈배열[] 을 넣음으로써 처음 랜더링될때만 호출된다.    
1) 리턴하는 함수는 컴포넌트가 언마운트 될때 호출되서, resize 이벤트를 제거한다.     

* useContext.     
자식 컴포넌트에서 부모 컴포넌트에서 전달된 컨텍스트 데이터사용시 Consumer 컴포넌트를 사용하지 않고 쓸수 있다.   

