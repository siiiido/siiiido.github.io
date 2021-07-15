---
title:  "form study"
excerpt: "form"

categories:
  - javascript

last_modified_at: 2021-07-16
---
form
```html
<form name="my">
  <input name="one" value="1">
  <input name="two" value="2">
</form>

<script>
  // 폼 얻기
  let form = document.forms.my; // <form name="my"> 요소

  // 요소 얻기
  let elem = form.elements.one; // <input name="one"> 요소

  alert(elem.value); // 1
</script>
```

***form 이름 같을 수도 있음. 주로 radio를 다룰 때 발생***
```html
<form>
  <input type="radio" name="age" value="10">
  <input type="radio" name="age" value="20">
</form>

<script>
let form = document.forms[0]; //html에서 첫 번째 form을 얻음

let ageElems = form.elements.age;

alert(ageElems[0].value); // 10 
</script>
```