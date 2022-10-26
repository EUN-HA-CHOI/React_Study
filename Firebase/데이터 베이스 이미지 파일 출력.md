### 데이터 베이스에서 이미지 파일 올리기/저장하기  

-데이터 베이스에서 이미지 파일을 올려보자-     
  file 을 배열로 불러와 브라우저의 API 인 FileReader 객체를 만들어 파일을 출력할 수 있게 하고,   
파일을 readAsDataURL로 url를 반환하게 한다.  

<img src="https://user-images.githubusercontent.com/97012561/198006985-f341ac51-a5ed-45fd-8f61-de66da976a61.png" width="700" height="200">  

웹 브라우저에서는 파일을 인식하는 시점과 끝나는 시점이 발생한다. 이를 위해 `reader.readAsDataURL(theFile);`를 아래와 같이 바꿔준다.  
theFile 에서 가져온 파일이 result에 이미지 url 을 넣어줌 (이미지 저장됨)   
```
reader.onloadend = (finishedEvent) => {
   const {currentTarget:{result}} = finishedEvent;
   setAttachment(result);
}
reader.readAsDataURL(theFile);
```

렌더링이 될 때 onSubmit에서 attachment 이 true 일 때 이미지를 불러오는 코드를 작성할 수 있다.  
```
{attachment && < img src={attachment} width="50" height="50" />} 
```

이미지 파일을 불러오면 화면에 다음과 같이 나타단다.  
<img src="https://user-images.githubusercontent.com/97012561/198008545-51a06c7b-dc0d-4121-b235-bf6af8dc8be0.png" width="400" height="200">

finishedEvent 가 콘솔창에 떴고, result 에서 이미지 주소를 확인 할 수 있다.  
<img src="https://user-images.githubusercontent.com/97012561/198008886-e31fff78-93a0-402c-b6a6-e742bd715989.png" width="400" height="200">

<hr>

### 이미지 삭제하기   

똑같이 onFileChange 코드 밑에  const onClearAttachment = () => setAttachment("");를 입력 해 준다.  setAttachment("");가 false이다.  


위와 같은 코드로 화면에 이미지를 출력 할 수 있지만, 용량이 큰 사진이나 동영상을 저장 할 땐 파이어 베이스에서 Storage 을 이용하여 파일을 저장 한다.  

<img src="https://user-images.githubusercontent.com/97012561/198021941-baa95be3-30b6-4314-b569-5c5a48d5e89f.png" width="600" height="300">  


Storage를 사용하기 전에 설정을 해 줘야 한다. getStorage 를 import 해 준다. Storage는 Cloud Firestore 와는 달리 자동으로 문서 이름이 생성되지 않는다.   
<img src="https://user-images.githubusercontent.com/97012561/198022288-123ad147-6ff0-4c1c-ba15-f98dc910efd9.png" width="600" height="300">  


먼저 ID 를 만들어주기 위해 설정을 해 줘야된다. 연결링크 => [ID 라이브러리](https://github.com/uuidjs/uuid)   
`npm install uuid` (: id 생성해주는 라이브러리) 를 설치하고, import { v4 as uuidv4 } from 'uuid'; 를 import 해 준다.    

다음으로 home 컴포넌트에  import { getStorage, ref, uploadString } from "firebase/storage"; 를 삽입한다.그 밑에 코드는 나의 코드에 맞게 형태를 변형 시켜준다.   
<img src="https://user-images.githubusercontent.com/97012561/198023657-360a7a5e-9989-4a00-9406-6dbe6ce1271d.png" width="600" height="300"> 

아래와 같은 코드로 변형 시켜줬다. attachment 이 경로를 따라서 사진을 업로드 한다. 라는 의미.  
```
const storageRef = ref(storage, `${userObj.uid}/${uuidv4()}`);
const response = await uploadString(storageRef,attachment ,'data_url');
```

Storage 에 이미지를 저장하기 위해 코드를 다 작성 해 줬지만 올라가지 않고 있다.   
그 이유는 Storage는 규칙이 있는데 여기서 읽기/쓰기를 false -> true로 바꿔야 한다.  

<img src="https://user-images.githubusercontent.com/97012561/198024860-f299f502-c5b1-44b2-8c28-8d2e8b7f1357.png" width="500" height="300"><img src="https://user-images.githubusercontent.com/97012561/198024982-f9d8f576-5a67-45f2-ae61-9e6577b5b305.png" width="380" height="200">

그럼 이렇게 이미지가 저장된 것을 확인 할 수 있다.  
<img src="https://user-images.githubusercontent.com/97012561/198025296-48741bfc-be11-46cb-9e34-edb498cd26f0.png" width="600" height="300">

