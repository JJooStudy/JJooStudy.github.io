---
layout: post
title:  "javascript 파일업로드(formdata)"
author: "JJoo"
comments: true
tags: Javascript
---

### Javascript 파일업로드하기 

file은 json에 포함될 수가 없어서 javascript로 파일업로드를 할때는 `Formdata`를 사용해서 업로드를 한다. 

```javascript
<input type="file" onChange={handleChane}/>

const handleChane = (e) =>{
  const formData = new FormData();
  formData.append('file', e.target.files[0]);
}
```

file type의 `input`에서 `onChange`event로 파일업로드를 다룰 수 있다.

`input`으로 파일 첨부시 `e.target.files[0]`에 파일값이 담기게 된다. 

생성자 함수로 새로운 `FormData` 객체를 만들어주고 거기에 `.append()`해주면 되는 데 꼭 `key`와 `value`를 쌍으로 넣어줘야 하고 첫번째 인자로 `key`값, 두번째 인자로 `value`값을 넣어주면 된다. 

위 코드에서는 `file`이라는 키에 `e.target.files[0]`를 넣어주었다. 

파일을 서버에 넘겨주기 위해서는 이렇게 `formdata`로 파일을 말아서 보내야 한다. 

그리고 이때 `headers`에 `Content-Type`을 `multipart/form-data`로 보내야 한다. 

```
const axiosService = axios.create({
    ...
    headers: {
      ...
      'Content-Type': 'multipart/form-data',
      ...
    },
  });
```

### 여러 파일 업로드 하기 

여러개의 파일을 업로드 할 때는 같은 키값으로 하나하나 반복해서 file을 append 시켜주면 된다. 

```javascript
const formData = new FormData();
for (let i = 0; i < files.length; i++) {
    formData.append('files', files[i]);
}

//or

const formData = new FormData();
formData.append('file', file1);
formData.append('file', file2);
```


### 파일과 json을 같이 업로드 하기 
 
파일 업로드와 함께 여러 데이터를 업로드를 해야할 때가 있다. 

글을 생성 혹은 수정할 때 이미지 파일과 다른 정보를 한번에 저장해야 하는 경우.

이럴 경우 위 여러 파일 업로드처럼 json을 `formdata`에 append 시켜주면 되는 데 json파일은 `application/json` 형식으로 보내야 해서 `Blob`을 사용한다.

```javascript
const formData = new FormData();
formData.append('file',  e.target.files[0]);
formData.append('key', new Blob([JSON.stringify(data.info)], { type: "application/json" }));
```

내가 했던 작업은 두개의 이미지 파일과 하나의 json 파일을 업로드 해야 해서 아래와 같이 작업했다. 

```javascript
const formData = new FormData();
formData.append('image1',  file1);
formData.append('image2',  file2);
formData.append('json', new Blob([JSON.stringify(data)], { type: "application/json" }));
```

그럼 서버에 

```
{
  image1 : file1
  image2 : file2
  json : data
}
```

가 `fomdata` 형식으로 보내지게 된다. 

