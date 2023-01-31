## useEffect를 사용하여 마운트/언마운트/업데이트 시 할 작업 설정

1. 화면이 처음 떴을때 실행.  
   * deps에 [] 빈배열을 넣을 떄.  
   * life cycle중 componentDidmount처럼 실행.     
2. 화면이 사라질때 실행(clean up함수).  
   * componentWillUnmount처럼 실행  
3. deps에 넣은 파라미터값이 업데이트 됬을때 실행.  
   * componentDidUpdate처럼 실행.  
   
```
useEffect(() => {//마운트
return () => {//언마운트
//useEffect 반환되는 함수는 cleanup 함수 (뒷정리)
//deps 가 비어있는 경우에는 컴포넌트가 사라질 때 cleanup 함수가 호출
};
}, []);
```

* useEffect 의 구조  
  * 함수이다.   
  * 첫 번째 인자는 함수, 두번째 인자는 배열(주로 deps 라고 칭한다.) 이 들어간다.   
  
  
* deps  
  * deps 에 특정값을 넣게 되면, 컴퍼넌트가 마운트 될 때, 지정한 값이 업데이트 될 때 useEffect 실행된다.  
  * deps에 값이 없다면 useEffect가 최신 값을 가리키지 않게 된다.  
  * deps에 값이 없다면 컴포넌트가 리렌더링 될 때마다 호출이 된다.  
  * deps에 값을 넣는것을 기본이라고 생각하는게 좋다.   
  
