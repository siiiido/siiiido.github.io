---
title:  "css position"
excerpt: "position"

categories:
  - html,css
last_modified_at: 2021-07-15
---
[](https://www.zerocho.com/category/CSS/post/5864f3b59f1dc000182d3ea1)
 

* position은 디폴트로 static임 → position 따로 적지 않아도 모든 부분이 static으로 시작한다.  static은 left, top, right, bottom 영향 안 받음

* position : relative 

* position : absolute → position: static 속성을 가지고 있지 않은 부모(즉, 부모가 position: relative, absolute, fiexd인 것들)를 기준으로 움직임
  *만약 부모 중에 포지션이 relative, absolute, fixed인 태그가 없다면 가장 위의 태그(body)가 기준이 된다.* 그리고 부모의 css가 기준점으로 적용이 안됨(예를 들면 부모가 padding을 20px넣으면 relative는 부모의 20px까지 적용하여 기준점을 계산하는데 absolute는 css를 무시하고 딱 부모를 기준으로 움직임)

* position : fixed → 기준을 윈도우 창(뷰포트)으로 하여 움직임

* position : sticky 참고하기 -> https://deeplify.dev/front-end/markup/position-sticky

left : 20px은 왼쪽으로 부터 20px 떨어지는 것
right, left, top, bottom 다 같은 원리 (즉, 각자의 방향에서 안쪽으로 이동)

바깥쪽으로 이동하고 싶으면 음수 붙이기


참고
* https://www.zerocho.com/category/CSS/post/5864f3b59f1dc000182d3ea1
