---
title:  "css 헷갈리는 것들"
excerpt: ""

categories:
  - html,css
last_modified_at: 2021-07-15
---
## none과 hidden
* display : none → 공간 차지 안함

* visibility : hidden → 공간 차지함

--------------------------------


## css margin, padding 순서

* **margin: 10px 20px 15px 5px;**

순서는 상우하좌  즉, 시계방향으로 돌아간다.


* **margin: 10px 5px;** 

상하의 margin 이 같고, 좌우의 margin 이 같을 때  


* **margin: 10px 5px 25px;**

좌우는 같은데, 상하가 다를 때

이때, 처음나오는 10px는 margin-top 의 값이고, 마지막에 나오는 25px는 margin-bottom이고 가운데, 5px 가 좌우 마진이다.

padding의 경우도 마찬가지

정리하면, 아래 4가지.

* margin: [margin-top] [margin-right] [margin-bottom] [margin-left];

* margin: [margin-top] [margin-left = margin-right] [margin-bottom];

* margin: [margin-top=margin-bottom] [margin-left = margin-right];

* margin: [margin-top = margin-bottom = margin-left = margin-right];

--------------------------

## 반응형 nav
* https://www.youtube.com/watch?v=X91jsJyZofw&list=PLv2d7VI9OotQ1F92Jp9Ce7ovHEsuRQB3Y&index=15&ab_channel=드림코딩by엘리

* https://www.youtube.com/watch?v=gXkqy0b4M5g&t=447s&ab_channel=DevEd