---
title:  "javascript 뒤죽박죽 기록하기(WesBos day 16 css shadow)"
excerpt: "mouse 이벤트, e.target, 배열과 객체의 구조 분해 할당 "

categories:
  - javascript

last_modified_at: 2021-05-11
---


### ***WesBos javascript day30을 공부하면서 모던js로 개념 정리***
* * *

## **offset**

offsetX, Y, Top, Right, Left, Bottom 등 사진을 보고 이해해야함   
  https://ko.javascript.info/size-and-scroll   


* * *

## **event.target VS event.currentTarget(=this)**
  이벤트가 발생한 가장 안쪽의 요소를 target이라 부르고 event.target를 사용해 접근 가능      
   event.target은 실제 이벤트가 시작된 target요소임, 버블링이 진행되어도 변화하지 않음   
  event.currentTarget(=this)은 '현재'요소로, 현재 실행중인 핸들러가 할당된 요소를 참조   
    https://ko.javascript.info/article/bubbling-and-capturing/bubble-target/ 에서   
  P 눌렀을 때 target = P, this = FORM   
  DIV 눌렀을 때 target = DIV, this = FORM   
  FORM 눌렀을 때 target = FORM, this = FORM   


  * * *

## **mouse event**
###  mouse 이벤트 : '마우스'라는 장치를 통해서만 생기는 것이 아니라 핸드폰이나 태블릿 같은 장치에서도 생김

**mouse 이벤트**   
* mousedown / mouseup : 요소 위에서 마우스 왼쪽 버튼을 누를 때 / 마우스 버튼을 누르고 있다가 뗄 때 발생

* mouseover / mouseout : 마우스 커서가 요소 바깥에 있다가 요소 안으로 움직일 때 / 커서가 요소 위에 있다가 요소 밖으로 움직일 때

* mousemove : 마우스를 움직일 때 발생

* click : 마우스 왼쪽 버튼을 사용해 동일한 요소 위에서 mousedown와 mouseup 이벤트를 연달아 발생시킬 때 실행됨

* contextmenu : 마우스 오른쪽 버튼을 눌렀을 때 발생     


마우스 버튼 : 클릭과 연관된 이벤트는 정획히 어떤 버튼에서 이벤트가 발생했는지를 알려주는 button 프로퍼티를 지원   
  click, contextmenu 이벤트를 다룰 땐 보통 button 프로퍼티 사용안함

mousedown, mouseup 이벤트를 다룰 땐 event.button 프로퍼티를 사용해 어떤 버튼에서 이벤트가 발생했는지를 알 수 있음   



* * *


## **배열과 객체의 구조 분해 할당**

구조 분해 할당 : 객체나 배열을 변수로 연결할 수 있다.

## **배열 분해하기**
```javascript
let [item1 = dafault, item2, ...rest] = array
//array의 첫 번째 요소는 item1, 두 번째 요소는 변수 item2에 할당되고 이어지는 나머지 요소들은 배열 rest에 저장됨
```

```javascript
let arr = ["Bora", "Lee"];

let firstaName = arr[0];
let surname = arr[1];

//위의 할당과 동일
// firstName엔 arr[0]이 surname엔 arr[1]을 할당함
let [firstName, surname] = arr;

alert(firstName); // Boar
alert(surname); // Lee


// 인덱스를 이용해 배열에 접근하지 않아도 변수로 접근이 가능해짐
// 배열 메서드인 split을 이용해도 좋음
let [firstName, surname] = "Bora Lee".split(' ');
```

쉼표를 사용하여 요소 무시하기
```javascript
let [firstName, , title] = ["song", "han", "kim", "alex", "tonny", "apple"];

alert(title); // kim출력

/*
firstName에는 song이 할당(첫 번째 요소), title에는 han이 할당(세 번째 요소)
두 번째 요소는 쉼표로 할당하는 것을 무사했다. 그리고 세 번째 이후로의 할당 역시 생략
*/
```

할당 연산자 우측에는 모든 이터러블(iterable, 반복 가능한 객체이 올 수 있음
```javascript
let [a, b, c] = "abc"; // ["a", "b", "c"]
let [one, two, three] = new Set([1,2,3]);
```

할당 연산자 좌측에는 할당할 수 있는(assignables)것이라면 뭐든지 올 수 있음
```javascript
let user = {};
[user.name, user.surname] = "Bora Lee".split(' ');

alert(user.name); //Bora
```   


변수 교환 트릭(두 변수에 저장된 값을 교환할 때 구조 분해 할당 사용가능)
```javascript
let guest = "Jane";
let admin = "Pete";

//변수 guest엔 Pete, 변수 admin에는 Jane이 저장되도록 값을 교환함
[guest, admin] = [admin, guest];

alert(`${guest} ${admin}`);// Pete Jane
```

'...'로 나머지 요소 가져오기
```javascript
// 배열 앞쪽에 위치한 값 몇 개만 필요하고 그 이후 이어지는 나머지 값들은 한데 모아서 저장하고 싶을 때

let [name1, name2, ...rest] = ["song", "han", "kim", "alex", "tonny"];

alert(name1); // song
alert(name2); // han

//rest는 배열임
alert(rest[0]); // kim
alert(rest[1]); // alex
alert(rest[2]); // tonny
alert(rest.length); // 3

/*rest는 나머지 배열 요소들이 저장된 새로운 배열임. rest 대신에 다른 이름을 사용해도 되는데,, 변수 앞의 점 세 개(...)와 변수가 가장 마지막에 위치해야 한다는 점은 꼭 지켜야함*/
```

기본값
```javascript
/*할당하고자 하는 변수의 개수가 분해하고자 하는 배열의 길이보다 크더라고 에러가 발생하지 않음. 할당된 값이 없으면 undefined로 취급됨*/

let [firstName, surname] = [];

alert(firstName); // undefined
alert(surname); // undefined



// = 을 이용하면 할당된 값이 없을 때 기본으로 할당해 줄 값인 기본값(default value)를 설정할 수 있음

// 기본값
let = [name = "Guest", surname = "Anonymous"] = ["Julius"];

alert(name); // Julius(배열에서 받아온 값)
alert(surname); // Anonymous (기본값)



// 복잡한 표현식, 함수 호출도 기본값이 될 수 있음. 기본식으로 표현식이나 함수를 설정하면 할당할 값이 없을 때 표현식이 평가되거나 함수가 호출됨
// name의 promp만 실행됨
let [surnam = prompt('성을 입력하세요'), name = promt('이름을 입력하세요')] = ["김"];

alert(surname); // 김(배열에서 받아온 값)
alert(name); // prompt에서 받아온 값

// 값이 제공되지 않을 때만 함수가 호출되므로, prompt는 한 번만 호출됨
```



## **객체 분해하기**

```javascript
let {prop : varName = default, ...rest} = object
//  object의 프로퍼티 prop의 값은 변수 varNamr에 할당되는데, object에 prop이 없으면 default가 varName에 할당됨.
//  연결할 변수가 없는 나머지 프로퍼티들은 객체 rest에 복사됨

// 기본 문법
// 할당 연산자 우측에는 분해하고자 하는 객체를, 좌측에는 상응하는 객체 프로퍼티의 '패턴'을 넣음
let {var1, var2} = {var1:..., var2:...}
```


```javascript
let options = {
    title : "Menu",
    width : 100,
    height : 200
};

let {title, width, height} = options;

alert(title); // Menu
alert(width); // 100
alert(heigth); // 200


// 프로퍼티 options.title과 options.width, options.height에 저장된 값이 상응하는 변수에 할당된 것을 확인할 수 있음
// 순서는 중요하지 않음

// let {...} 안의 순서가 바뀌어도 동일하게 동작함
let {heigth, width, title} = {title : "Menu", heigth : 200, width : 100}
```
할당 연산자 좌측에는 복잡한 패턴이 올 수 있음. 분해하려는 객체의 프로퍼티와 변수의 연결을 원하는대로 조정가능
```javascript
let options = {
    title : "Menu",
    width : 100,
    heigth : 200
  };

// {객체 프로퍼티 : 목표 변수}
let {width : w, heigth : h, title} = options;  

// 프로퍼티 width를 변수 w에 width -> w
// 프로퍼티 heigth 변수 h에 heigth -> h
// 프로퍼티 title은 동일한 이름을 가진 변수 title에 저장함 title -> title

alert(title); // Menu
alert(w); // 100
alert(h); // 200
```

프로퍼티가 많은 복잡한 객체에서 원하는 정보만 뽑는거 가능
```javascript
// title만 변수로 뽑아내기
let {title} = options;

alert(title); //Menu

// title = 이름이 title인 프로퍼티
// rest = 나머지 프로퍼티들
let {title, ...rest} = options;

// title엔 "Menu", rest엔 {heigth : 200, width : 100}이 할당됨
alert(rest.heigth); // 200
alert(rest.width); // 100
```

프로퍼티가 없으면 =를 사용해 기본값 설정 가능
```javascript
let options = {
    title : "Menu"
  };

let {width = 100, heigth = 200, title} = options;

alert(title); // Menu
alert(width); // 100
alert(heigth); // 200



// 표현식이나 함수 호출을 기본값으로 할당 가능. 표현식이나 함수는 값이 제공되지 않았을 때 평가 혹은 실행됨
let { width = prompt("width?"), title = prompt("title?")} = options;

alert(title); // Menu
alert(width); // prompt 창에 입력한 값



// 클론과 할당 연산자를 동시에 사용가능
let {width: w = 100, heigth: h = 200, title} = options;

alert(title); //Menu
alert(w); //100
alert(h); //200
```

let 없이 사용 가능
```javascript
let title, width, heigth;

{title, width, heigth} = {title: "Menu", width : 200, heigth : 100}; // 에러 발생 !!

({title, width, heigth} = {title : "Menu", width : 200, heigth : 100}); // 에러 발생하지 않음
```


중첩 구조 분해
```javascript
let options = {
    size : {
      width : 100,
      heigth : 200
    },
    items : ["Cake", "Donut"],
    extra : true
  };


// 코드를 객체와 같은 구조로하여 의도를 말해줘야함
let {
    size : {  // size는 여기
      width,
      heigth
    },
    items: [item1, item2], //items는 여기에 할당
    title = "Menu" // 분해라겨는 객체에 title 프로퍼티가 없으므로 기본값을 사용
  } = options;

alert(title); // Menu
alert(width); // 100
alert(heigth); // 200
alert(item1); // Cake
alert(item2); // Donut
```
함수 매개변수
```javascript
function showMenu(title = "Untitled", width = 200, heigth = 100, items = []){
    // ...
  }

// Untitled은 기본값임,기본값을 사용할려면 undefined을 넘겨줘야함 
showMenu("My Menu", undefined, undefined, ["Item1", "Item2"])

// 이렇게 함수를 호출하면 가독성도 떨어지고 할 떄마가 매개변수를 다 적어줘야함 

// 구조 분해로 리팩토링
let options = {
    title : "My menu",
    items : ["Item112", "Item234"]
  };

function showMenu({title = "Untitled", width = 200, heigth = 100, items = []}){
    // title, items는 객체 options에서 가져옴
    width, heigth는 기본값임
    alert(`${title} ${width} ${heigth}`); // My menu 200 100
    alert(items); // Item112, Item234
  }

showMeny(options); // 매우 깔끔



/*중첩 객체와 콜론을 조합하여서도 가능*/
function showMeny({
    title = "Untitled",
    width : w = 100,  /*width는 w에*/
    heigth : h = 200 /*heigth는 h에*/
    items : [item1, item2] /*items의 첫 번째 요소는 item1에, 두 번째 요소는 item2에 할당함*/
  }){
    alert(`${title} ${w} ${h}`); // My meny 100 200
    alert(item1); // Item112
    alert(item1); // Item234
  }

showMeny(options); // 매우 깔끔



/*인수 전체를 기본값으로 할려면 빈 객체를 설정*/
function showMenu({title = "Menu", width = 100, heigth = 200} = {} ){
    alert(`${title} ${width} ${heigt}`);
  }

showMeny(); // Menu 100 200
```
할당 연산자 좌측의 패턴과 우측의 구조가 같으면 중첩 배열이나 중첩 객체가 있는 복잡한 구조에서도 원하는 데이터를 뽑아낼 수 있음



