---
title:  "javascript 뒤죽박죽 기록하기(WesBos day 14 reference-copy)"
excerpt: ""

categories:
  - javascript

last_modified_at: 2021-05-11
---


### ***WesBos javascript day30을 공부하면서 모던js로 개념 정리***
* * *

## copy와 reference 공부 (단어는 다를 수도)




```javascript
let age = 100;
let age2 = age;
console.log(age, age2); // 100, 100이 출력
age= 200;
console.log(age, age2); // 200, 100이 출력 

let name = 'Wes';
let name2 = name;
console.log(name, name2); // Wes, Wes 출력
name = 'wesley';
console.log(name, name2); // wesley, wes 출력
```


```javascript
const players = ['Wes', 'Sarah', 'Ryan', 'Poppy'];

// reference
const team = players

console.log(players, team); // ['Wes', 'Sarah', 'Ryan', 'Poppy'], ['Wes', 'Sarah', 'Ryan''Poppy'];

// team과 players가 참조하는 것이 같으니 
// team[3]을 바꾸든, players[3]을 바꾸든 둘 다 바뀐다
team[3] = 'Lux';
console.log(players, team);// ['Wes', 'Sarah', 'Ryan', 'Lux'], ['Wes', 'Sarah', 'Ryan', 'Lux'];

players[3] = 'Song';
console.log(players, team); // ['Wes', 'Sarah', 'Ryan', 'Song'], ['Wes', 'Sarah', 'Ryan', 'Song'];

```

다시 처음 예시 요소로 돌리기

```javascript
const test = ['Wes', 'Sarah', 'Ryan', 'Poppy'];
    
for(let i=0; i<test.length; i++){
   players[i] = test[i];
}

console.log(players, team); //['Wes', 'Sarah', 'Ryan', 'Poppy'], ['Wes', 'Sarah', 'Ryan', 'Poppy'];
```

## **arr.slice()**

```javascript
arr.slice([start], [end]) // start 인덱스 부터 end인덱스 전 까지 복사하여 새로운 배열을 반환

const sliceArray = [1,2,3,4,5,6,7];
const sliceAnotherArray = sliceArray.slice(2,4); // sliceArray[2] 부터 sliceArray[4]전 까지 복사하여 sliceAnotherArray에 새로운 배열로 반환

console.log(sliceAnotherArray); // 3,4
    
// slice는 참조가 아닌 완전히 복사하는 것 따라서 위와 다르게 team2의 요소를 바꾼다고 players의 요소가 바뀌지 않는다
const team2 = players.slice(); // team2와 players는 같은 요소를 가진 배열이지만 다른 배열임
    
//team2[3]만 바뀌고 players[3]은 바뀌지 않는다.
team2[3] = 'Song';

console.log(team2, players);
// team2 = ['Wes', 'Sarah', 'Ryan', 'Song'];
// players = ['Wes', 'Sarah', 'Ryan', 'Poppy'];
```

## **arr.concat()**

```javascript

arr.concat(arg1, arg2...) // 기존 배열의 요소를 사용해 새로운 배열을 만들거나 기존 배열에 요소를 추가할 때 사용
// 인수(arg1, arg2...)에는 배열이나 값이 올 수 있는데, 개수에는 제한 없음, 배열이면 모든 요소가 복사됨
    
const arr = [1,2];
    
alert(arr.concat([3,4])) //1,2,3,4
    
alert(arr.concat([3,4], [5,6])) //1,2,3,4,5,6
    
alert(arr.concat([3,4],5,6)) //1,2,3,4,5,6

// 배열의 요소를 복사하지 객체가 통으로 넘어오면 분해하지 않고 통으로 받음
let arrayLike = {
    0:'something',
    length:1
};

alert(arr.concat(arrayLike)); // 1,2, [object Object]

// Symbol.isConcatSpreadable를 사용하면 객체를 배열처럼 취급하여 객체 프로퍼티의 값을 더함
let arrLike1 = {
    0:'something',
    1:'else',
    [Symbol.isConcatSpreadable] : true,
    length:2
};

alert(arr.concat(arrLike1)) // 1,2,something,else



// players = ['Wes', 'Sarah', 'Ryan', 'Poppy'] 임

const team3 = [].concat(players);
    
console.log(team3); //['Wes', 'Sarah', 'Ryan', 'Poppy']

```

## **spread(...)와 Array.from**

```javascript
// speard(...) : 배열을 통째로 매개변수로 넘겨줄 때 사용
    
// 내장 함수 Math.max는 인수 중 가장 큰 숫자를 반환, 숫자를 인수로 받지 배열을 받지는 않음
alert(Math.max(3,5,1)); // 5
    
let mathArr = [3,5,1];
    
alert(Math.max(arr)); // NaN 출력, Math.max는 배열을 받지는 않는다.
    
// 그래서 ...을 사용하자
    
let spreadArr = [3,5,1];

alert(Math.max(...spreadArr)); // 5 (...이 배열을 인수로 바꿈)

// 여러개 전달도 가능
let arr1 = [1,-2,3,4];
let arr2 = [8,3,-8,1];

alert(Math.max(...arr1, ...arr2)); // 8

//평범한 값과 혼합해서 사용가능

alert(Math.max(1, ...arr1, 2, ...arr2, 25)); //25

//배열을 합칠 때도 사용 가능

let testArr = [3,5,1];
let testArr1 = [8,9,15];

let merged = [0, ...testArr, 2, ...testArr1];

alert(merged); // 0,3,5,1,2,8,9,15 (0, testArr, 2, testArr1 순서로 합쳐짐 )

// 문자열을 문자 배열로 반환 가능

let str = "Hello";

alert([...str]); // H, e, l, l, o
```
Array.from 역시 문자열 같은 이터러블 객체를 배열로 바꿔줌   
(iterable 객체 : 반복 가능한 객체로 배열을 일반화한 객체임   
즉 어떤 객체든 for..of 반복문을 적용할 수 있으면 iterable 객체임)

```javascript
alert(Array.from(str));// H, e, l, l, o
```

즉 배열에서는 [...str]과 Array.from(str)은 동일한 결과를 출력함
    
**하 지 만**

Array.from(obj) 와 [...obj]에는 차이가 있음   
Array.from(obj)는 유사 배열 객체와 이터러블 객체 둘 다 사용 가능   
[...obj]는 이터러블 객체에만 사용 가능   


```javascript
// players = ['Wes', 'Sarah', 'Ryan', 'Poppy'] 임

// ...역시 참조가 아닌 복사를 하는 것이여서 team4의 값을 바꿔도 players는 바뀌지 않는다.
const team4 = [...players];
team4[3] = 'heee haww';
console.log(players,team4);
// players : ["Wes", "Sarah", "Ryan", "Poppy"] 
// team4 : ["Wes", "Sarah", "Ryan", "heee haww"]
    
const team5 = Array.from(players);
console.log(players,team5);
// players : ["Wes", "Sarah", "Ryan", "Poppy"] 
// team5 : ["Wes", "Sarah", "Ryan", "Poppy"]
```


* * *
## **객체의 referenc, copy**

```javascript
const person = {
    name: 'Wes Bos',
    age: 80
};

// 참조에 의한 복사
const captain = person;
    
captain.number = 99; // captain과 person은 같은 객체를 참조하고 있으니 person역시 number : 99가 추가된다.

console.log(person,captain); // number:99 추가, 둘 다 같은 값 출력
```

## **Object.assign**

Object.assign을 사용하여 객체를 참조가 아닌 새로운 객체로 복사할 수 있다   
하지만 중첩객체는 copy가 아닌 reference

* Object.assign(dest, [src1, src2, src3...])   
* dest : 목표로 하는 객체   
* src1, src2, src3... : 복사하고자 하는 객체 (수는 제한 없음)   
* src1, src2, src3...의 프로퍼티를 dest객체에 복사함, 즉 dest를 제외한 인수(객체)의 프로퍼티 모두 dest로 복사됨   
* 마지막으로 dest를 반환함

```javascript
let user = {name : 'Jhon'};

let permission1 = { pepsi : true};
let permission2 = { coke : true};

// permission1과 permission2의 프로퍼티를 user로 복사함
Object.assign(user, permission1, permission2);

console.log(user) // {name: "Jhon", pepsi: true, coke: true}

 // 목표 객체(user)에 동일한 이름을 가진 프로퍼티가 있는 경우에는 기존 값을 덮어 씌움


let user1 = {name : 'Jhon'};

Object.assign(user1, {name:'Pete'});
    
console.log(user1); // user = {name : 'Pete'}


//Object.assign을 사용하면 반복문 없이 간단하게 객체 복사 가능

let user2 = {
    name : 'Jhon',
    age : 30
};

let clone = Object.assign({}, user2);

console.log(clone); //{name: "Jhon", age: 30}
```

```javascript

 /*
const person = {
    name: 'Wes Bos',
    age: 80,
    number : 99
};
*/
   
// person객체의 number : 99로 수정하고 age를 12로 수정하고, height : 178를 추가하여 cap2로 복사 (복사할 때 새롭게 추가해도 된다.)
const cap2 = Object.assign({}, person, {number : 99, age : 12, height : 178});
console.log(cap2); //{name: "Wes Bos", age: 12, number: 99, height: 178}


// We will hopefully soon see the object ...spread
    
// reference가 아닌 copy!! 즉 다른 객체이다!!
const cap3 = {...person};

cap3.number = 10000;

console.log(person,cap3);
// person : {name: "Wes Bos", age: 80, number: 99}
// cap3 : {name: "Wes Bos", age: 80, number: 10000}
```


```javascript
const wes = {
    name:'Wes',
    age : 100,
    social:{
    twitter : '@wesbos',
    facebook : 'wesbos.developer'
    }
}

// 1 level copy (중첩 객체는 copey가 아닌 reference한다)
const dev = Object.assign({}, wes);
dev.name = 'Wesly';
// wes의 name은 바뀌지 않고, dev의 name만 wesly로 바뀜
console.log(wes.name, dev.name); //Wes Wesly

// 하지만
dev.social.twitter = '@coolman';
// 둘 다 @coolman이 출력됨 
console.log(wes.social.twitter, dev.social.twitter); //@coolman, @coolman
```
객체인 Object.assign은 무슨 단어를 써야할지 모르겠지만   
 1. level인 name, age는 값을 바꿔도 wes, dev가 따로따로 바뀌지만    
 2. level인 social은 참조의 의미처럼 dev를 바꾸면 wes도 바뀐다 , 즉 중첩 객체는 참조 상태를 유지한다.


## **JSON.stringify, JSON.parse**
JSON.stringify : 객체를 JSON으로 바꿔줌   
JSON.parse : JSON을 객체로 바꿔줌

```javascript
let student = {
    name: 'John',
    age: 30,
    isAdmin: false,
    courses: ['html', 'css', 'js'],
    wife: null
};

alert(typeof student); // Object(객체임)


let json = JSON.stringify(student);



 alert(typeof json); // string(문자열로 바뀜)

console.log(json)
/* JSON으로 인코딩된 객체:
{
    "name": "John",
    "age": 30,
    "isAdmin": false,
    "courses": ["html", "css", "js"],
    "wife": null
}
*/
```


student가 문자열로 바뀜 이렇게 변경된 문자열을   
* JSON으로 인코딩된(JSON-encoded)    
* 직렬화 처리된(serialized)
* 문자열로 변환된(stringified)   
* 결집된(marshalled)   
객체라고 부름, 객체는 이렇게 문자열로 변환 후 네트워크를 통해 전송하거나 저장함

JSON으로 인코딩된 객체는 일반 객체와 다른 특징이 있음
1. 문자열은 큰따옴표로 감싼다 -> JSON에서는 작은따옴표나 백틱을 사용할 수 없음( 'Jhon'이 "Jhon"으로 바뀜)
2. 객체 프로퍼티 이름은 큰따옴표로 감싸야함 (age : 30이 "age" : 30으로 바뀜)


JSON.stringify는 객체뿐만 아니라 원시값에도 적용가능   
* 객체 { ... }
* 배열 [ ... ]
* 원시형:
* 문자형
* 숫자형
* 불린형 값 true와 false
* null


```javascript
 // 숫자를 JSON으로 인코딩하면 숫자
 alert( JSON.stringify(1) ) // 1

//  문자열을 JSON으로 인코딩하면 문자열(다만, 큰따옴표가 추가).
 alert( JSON.stringify('test') ) // "test"

//  boolean값을 JSON으로 인코딩하면 boolean
 alert( JSON.stringify(true) ); // true

//  배열을 JSON으로 인코딩하면 배열
 alert( JSON.stringify([1, 2, 3]) ); // [1,2,3]
```

하지만 JSON은 사실 데이터 교환 목적으로 만들어진 것으로 언어에 종속되지 않음   

따라서 자바스크립트 특유의 객체 프로퍼티는 JSON.stringify가 처리할 수 없음   

JSON.stringify 호출 시 무시되는 프로퍼티 (하지만 보통 이 프로퍼티들은 무시되어 괜찮은 프로퍼티들임)   
* 함수 프로퍼티 (메서드)
* 심볼형 프로퍼티 (키가 심볼인 프로퍼티)
* 값이 undefined인 프로퍼티
  
```javascript
let user5 = {
    sayHi() { // 무시
    alert("Hello");
    },
    [Symbol("id")]: 123, // 무시
    something: undefined // 무시
};

console.log( JSON.stringify(user5)) //{} (빈 객체가 출력됨)
```

그리고 JSON.stringify는 중첩 객체도 알아서 문자열로 바꿔준다(very goooood)

```javascript
let meetup = {
    title: "Conference",
    room: {
    number: 23,
    participants: ["john", "ann"]
    }
};

console.log( JSON.stringify(meetup)) // 객체 전체가 문자열로 반환됨
/*{
  "title":"Conference",
  "room":{"number":23,"participants":["john","ann"]},
}*/
```

**하 지 만**   
JSON.stringify 사용할 때 주의할 점 -> 순환 참조가 있으면 원하는 대로 객체를 문자열로 바꾸지 못함
```javascript
let room = {
    number: 23
};

let meetup1 = {
    title: "Conference",
    participants: ["john", "ann"]
};

meetup1.place = room;       // meetup은 room을 참조
room.occupiedBy = meetup1; // room은 meetup을 참조

JSON.stringify(meetup1); //에러남 Error: Converting circular structure to JSON
```
oom.occupiedBy는 meetup을, meetup.place는 room을 참조하기 때문에 JSON으로의 변환이 실패   


**본론으로 와서**
```javascript
/*const wes = {
  name:'Wes',
  age : 100,
  social:{
    twitter : '@coolman',
    facebook : 'wesbos.developer'
  }
}*/

// JSON.stringify는 Object.assign와 다르게 중첩 객체까지 copy한다. 즉 완전히 다른 객체가 되는 것이다.
const dev2 = JSON.parse(JSON.stringify(wes));

dev2.name = 'song';
// wes는 바꾸지 않음 !
console.log(wes.name, dev2.name); //wes, song
dev2.social.twitter = '@hotman';
// wes는 바꾸지 않음 !
console.log(wes.social.twitter, dev2.social.twitter); // @coolman, @hotman
```

