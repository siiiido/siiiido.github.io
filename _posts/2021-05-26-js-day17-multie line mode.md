---
title:  "javascript 뒤죽박죽 기록하기(WesBos day 17 Sort Without Articles)"
excerpt: "trim(), ^, $, m플래그"

categories:
  - javascript

last_modified_at: 2021-05-26
---


### ***WesBos javascript day30을 공부하면서 모던js로 개념 정리***
* * *

## **str.trim()**
str.trim() : 문자열 앞과 끝의 공백 문자를 다듬어 줌(즉 공백제거)
 * * *

## **앵커 ^와 $의 여러 행 모드, 'm' 플래그**
m 플래그 사용하면 여러 행 모드를 활성화 가능   
여러 행 모드는 ^와 $의 작동 방식에만 영향을 줌   
여러 행 모드에서는 두 앵커가 전체 문자열의 처음과 끝뿐 아니라 각 행의 시작과 끝에도 대응   


### **^로 행 시작 검색하기**

여러 행으로된 텍스트를 *\d/gm* 패턴으로 각 행이 시작하는 위치에 있는 숫자를 찾을 수 있음   
```javascript
let str = `1st place : Winnie
2nd place : Piglet
3rd place : Eeyore`;

alert(str.match(/^\d/gm)); // 1, 2, 3



// m 플래그 없이 검색하면 맨 앞 숫자만 검색됨

let str = `1st place : Winnie
2nd place : Piglet
3rd place : Eeyore1;

alert(str.match(/^\d/g)); // 1
```

즉 ^ 기호는 텍스트의 시작 위치에만 대응함 이 말은 여러 행으로 되어 있어야 모든 행의 시작 위치에 대응함   

(행의 시작이라는 것은 다시 말해 줄 바꿈 직후와 같음. 그렇다면 ^을 사용한 검사는 줄 바꿈 문자 \n의 바로 뒤 모든 위치와 일치함)




### **$로 행 끝 검색**

정규 표현식 \d$를 사용하면 행 끝에 있는 숫자를 모두 찾음
```javascript
let str = `Winnie : 1
Piglet : 2
Eeyore : 3`;

alert(str.match(/\d$/gm)); // 1, 2, 3
```
m 플래그가 없으면 $ 기호는 전체 텍스트의 끝에만 일치하므로 가장 뒤에 있는 마지막 숫자만 나옴   
(행 끝 이라는 것은 다시 말해 줄 바꿈 직전을 의미함. 여러 행이 있을 때 $을 사용한 검사는 줄 바꿈 문자 \n 바로 앞의 모든 위치와 일치함)




### **^ $ 대신 \n 검색하기**

줄 바꿈을 찾을 때는 ^ $뿐 아니라 줄 바꿈 문자 \n을 사용할 수도 있음
```javascript
let str = `Winnie : 1
Piglet : 2
Eeyore : 3`;

alert(str.match(/\d\n/gm)); // 1\n, 2\n
```
일치하는 결과가 3개가 아니라 2개임 why? -> 3 다음은 줄 바꿈이 없어서 하지만 텍스트의 끝이라서 $에는 일치함   
그리고 일치 결과에 \n이 포함되어 있음. 앵커 ^ $은 행의 시작, 끝이라는 조건만 검사하지만 \n은 문자여서 결과에 포함됨

즉 결과에 줄 바꿈 문자가 필요하면 정규식 패턴에 \n을 사용하기 또는 줄 바꿈 문자 없이 행의 시작.끝에 있는 문자만 찾고 싶으면 ^를 사용하기


