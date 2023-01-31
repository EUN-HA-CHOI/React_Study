## useReducer 를 사용하여 상태 업데이트 로직 분리하기.    

* 현재 컴포넌트가 아닌 다른 곳에 state를 저장하고 싶을때 유용한다.  

* reducer 함수 : 현재 상태와 액션 객체를 파라미터로 받아와서 새로운 상태를 반환   
reducer 에서 반환하는 상태는 곧 컴포넌트가 지닐 새로운 상태   

### useReducer를 위한 함수  
 * reducer 라는 함수를 만들고 state와 action 이라는 인자를 받는다.  
   * reducer 라는 함수는 예약어는 아니어서 다른 이름으로 만들수있다. (하지만 reducer로 사용하는게 좋다.)    
 * action에는 객체가 전달되는데 그 안에 type 이라는 프로퍼티를 주로 설정해서 사용한다.    
 * type 프로퍼티를 통해 switch 문으로 분기한다.    
 * state는 useReducer를 통해 저장된 변수다.  
 * 주로 initialState라는 객체에 초기 정보를 담고 useReducer 에게 전달한다.   


### useReducer 형태  
`  const [state, dispatch] = useReducer(reducer, initialState);`   

* state 는 우리가 앞으로 컴포넌트에서 사용 할 수 있는 상태   
* dispatch 는 액션을 발생시키는 함수  
* reducer는 함수  
* initialState는 객체이다   


### useReducer vs useState    
* useState: 컴포넌트에서 관리하는 값이 단순, 숫자, 문자열 , boolean 값   
* useReducer: 컴포넌트에서 관리하는 값이 여러개가 되어서 상태의 구조가 복잡    

