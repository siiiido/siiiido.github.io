---
title:  "javascript 뒤죽박죽 기록하기(WesBos day 16 css shadow)"
excerpt: "localStorage와 sessionStorage, matches(), event.preventDefault()"

categories:
  - javascript

last_modified_at: 2021-05-25
---


### ***WesBos javascript day30을 공부하면서 모던js로 개념 정리***
* * *

## **localStorage와 sessionStorage**

localStorage, sessionStorage 모두 key-value를 저장함   

*sessionStorage*인 경우 페이지 새로 고침, *localStorage*인 당연 새로고침 & 브라우저를 다시 실행해도 데이터가 사라지지 않고 남아 있음   
*sessionStorage*는 오리진이 같은 브라우저 탭, iframe에서 공유됨 ,  *localStorage*는 오리진이 같은 탭, 창 전체에서 공유됨  

**하 지 만**

쿠키를 사용해도 브라우저에 데이터 저장할 수 있는데 굳이 왜 다른 객체를 사용하여 데이터를 저장할까?   
1. 쿠키와 다르게 웹 스토리지 객체(localStorage와 sessionStorage)는 네트워크 용청 시 서버로 전송되지 않음 -> 쿠키보다 더 많은 자료를 보관할 수 있음
2. 쿠키와 다르게 서버가 HTTP 헤더를 통해 객체를 조작할 수 없음 -> 웹 스토리지 객체(localStorage와 sessionStorage)는 모두 자바스크립트를 통해서 수행됨
3. 웹 스토리지 객체(localStorage와 sessionStorage)는 도메인, 프로토콜, 포트로 정의되는 오리진(origin)에 묶여 있음 -> 프로토콜과 서브 도메인이 다르면 데이터에 접근 못함


### **localStorage와 sessionStorage는 동일한 메서드와 프로퍼티를 제공함**
* setItem(key, value) : key, value 쌍을 보관함
* getItem(key) : key에 해당하는 값을 받아옴
* removeItem(key) : key와 해당 value를 삭제함
* clear() : 모든 것을 삭제함
* key(index) : 인덱스(index)에 해당하는 key를 받아옴
* length : 저장된 항목의 개수를 얻음    

스토리지 객체(localStorage와 sessionStorage)는 Map과 유사 (setItem/ getItem/ removeItem)을 지원 하지만 Map은 key(index)을 지원하지 않는다 오직 스토리지 객체만 !


### **localStorage**
* 오리진(domain, port, protocol) 같은 경우 데이터는 모든 탭과 창에서 공유됨
* 브라우저나 OS가 재시작하더라고 데이터가 파기되지 않음

```javascript
localStorage.setItem('test',1); //을 하고 브라우저를 닫고 다른 창에서 페이지를 열어도
  
alert(localStorage.getItem('test')); // 1이 출력됨 

/*why? -> 오리진(domain, port, protocol)만 같으면 url 경로가 달라고 동일한 결과를 볼 수 있음
localStorage는 동일한 오리진을 가진 모든 창에서 공유됨*/
```

**localStorage 일반 객체처럼 사용하기 -> 추천X**

```javascript
//키 설정
 localStorage.test = 2; 

// 키 얻기
alert(localStorage.test); // 2

// 키 삭제
delete localStorage.test;
```
why 추천 X 인가?
1. 사용자는 length나 toString, localStorage의 내장 메서드를 key로 설정할 수 있지만 이렇게 사용하면 setItem, getItem은 사용가능해도 일반 객체처럼 다룰 때 에러 발생
```javascript
// 예시
let key = 'length';
localStorage[key] = 5; // 에러
```
2. 데이터를 수정하면 storage 이벤트가 발생하지만, 이 이벤트는 localStorage를 객체처럼 접근할 때는 일어나지 않음
  

**키 순회하기**   
localStorage는 key를 사용해서 value를 얻고, 설정, 삭제 가능 그렇다면 key-value 전체는 어떻게 얻을까?(스토리지 객체는 iterable 객체가 아님)

* Object.keys를 사용해 자기 자신의 key를 받아서 순회하기
* Object.keys는 해당 객체에서 정의한 key만 반환하고 프로토타입에서 상속받은 key는 무시함
```javascript
let keys = Object.keys(localStorage);
  for(let key of keys){
    alert(`${key} : ${localStorage.getItem(key)}`);
  }
```


**문자열만 사용**   
localStorage의 key와 value는 반드시 문자열이어야 한다.
  *즉 숫자나 객체 등 다른 자료형을 사용하면 문장열로 자동 변환됨*
```javascript
localStorage.user = {name : "Jhon"};
alert(localStorage.user); // [object Object]


// JSON을 사용하면 객체를 쓸 수 있음

localStorage.user = JSON.stringfy({name : "Jhon"});

let user = JSON.parse(localStorage.user);
alert(user.name); //Jhon


// 디버깅 목적으로 스토리지 객체 전체를 문자열로 변환도 가능

alert(JSON.stringfy(localStorage, null, 2));
```


### **sessionStorage**
sessionStorage는 localStorage에 비해 자주 사용 X
why? -> 제공하는 프로퍼티, 메서드는 같지만 제한적임
* sessionStorage는 현재 떠 있는 탭 내에서만 유지됨
    - 같은 페이지라도 다른 탭에 있으면 다른 곳에 저장됨
    - 하 지 만 하나의 탭에 여러 개의 iframe이 있는 경우에는 동일한 오리진에서 왔다고 취급되기 때문에 sessionStorage가 공유됨
* 페이지를 새로 고침할 때 sessionStorage에 저장된 데이터는 사라지지 않음 하지만 탭을 닫고 새로 열면 사라짐

```javascript
sessionStorage.setItem('test',1);

alert(sessionStorage.getItem('test')); // 페이지 새로고쳐도 1출력

//  하지만 다른 탭에서 페이지 열고 실행하면 null이 반환 즉 sessionStorage는 오리지뿐만 아니라 브라우저 탭에도 종속됨
```

**localStorage와 sessionStorage의 데이터가 갱신될 때, storage 이벤트가** 실행됨    
setItem, removeItem, clear를 호출할 때 발생

* key : 변경된 데이터의 key(.clear()를 호출해다면 null)
* oldValue : 이전 value( key가 새롭게 추가되었다면 null)
* newValue : 새로운 value( key가 삭제되었다면 null)
* url : 갱신이 일어난 문서의 url
* storageArea : 갱신이 일어난 localStorage, sessionStorage 객체   

storage 이벤트가 이벤트를 발생시킨 스토리지를 제외하고 스토리지에서 접근 가능한 window 객체 전부에서 일어남   
*즉 두 개의 창에 같은 사이트를 띄웠다고 가정 -> 창은 다르지만 localStorage는 서로 공유됨*

이벤트가 생성된 곳을 제외하고 스토리지에 접근하는 모든 window 객체에서 일어남(sessionStorage는 탭 내에서, localStorage는 전역에서)

그리고 storage 이벤트는 event.url이 있어 데이터가 갱신된 문서의 URL을 알 수 있음

sessionStorage, localStorage가 변경될 때 스토리지 종류에 상관없이 storage 이벤트가 발생하기 때문에 event.storageArea는 스토리지 종류에 상관없이 실제 수정이 일어난 것을 참조함


* * *
## **matches()**
  
elem.matches()는 요소 elem이 주어진 CSS 선택자와 일치하면 true 반환, 아니면 false 반환

* * *

## **브라우저 기본 동작 막기**

1. event 객체를 사용하기 이때 event 객체에 구현된 event.preventDefault() 사용하기
2. 핸들러가 addEventListener가 아닌 on<event>를 사용해 할당되었으면 false를 반환해 기본 동작 막을 수 있음
  