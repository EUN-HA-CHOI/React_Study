## useCallback 을 사용하여 함수 재사용하기.   

useMemo 는 특정 결과값을 재사용 할 때 사용하는 반면, useCallback 은 특정 함수를 새로 만들지 않고 재사용하고 싶을때 사용.     

 한번 만든 함수를 필요할때만 새로 만들고 재사용하는 것은 여전히 중요. 그 이유는, 우리가 나중에 컴포넌트에서 props 가 바뀌지 않았으면 Virtual DOM 에 새로 렌더링하는 것 조차 하지 않고   
 컴포넌트의 결과물을 재사용 하는 최적화 작업을 할건데, 이 작업을 하려면, 함수를 재사용하는것이 필수     
 
 ```
  const onToggle = id => {
    setUsers(
      users.map(user => 
        user.id === id ? {...user, active: !user.active} : user
        )
    );
  };
 ```
 * useCallback ({},[]) 적용 후   
 
 ```
  const onToggle = useCallback (
     id => {
    setUsers(
      users.map(user => 
        user.id === id ? {...user, active: !user.active} : user
        )
    );
  },[users]
  );
 ```
 
 주의 할 점은 함수 안에서 사용하는 상태 혹은 props 가 있다면 꼭, deps 배열안에 포함시켜야 된다는 것       
  
 만약에 deps 배열 안에 함수에서 사용하는 값을 넣지 않게 된다면, 함수 내에서 해당 값들을 참조할때 가장 최신 값을 참조 할 것이라고 보장 할 수 없다.  
 props 로 받아온 함수가 있다면, 이 또한 deps 에 넣어주어야 함.     
