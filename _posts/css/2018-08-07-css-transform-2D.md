---
title: css transform 2D
date: 2018-08-07
category: transform 2D
tags:
  - css
---
# translate() 속성 (2D)


웹요소의 위치를 옮기거나 크기를 조절하고 회전, 변형시키는 것을 transform 이라고 합니다.
예전에는 포토샵을 사용했어야 했는데 CSS3로 넘어오면서 포토샵의 기능과 비슷하게 웹요소를 순수 CSS 기술만으로 변형할 수 있게 되었습니다.

웹요소를 변형하려면 transfrom 속성을 사용해야 하며 기본형식은 아래와 같습니다.
여기에서 ‘변형 함수 값’이란 오늘 알아볼 ‘scale, rotate, skew, translate’ 등을 말합니다.


```
transform: 변형 함수 값;
```


translate() |   |   |
----------  | ---- | ----
transform:translate() | translate (x, y) 함수는 요소를 왼쪽에서부터 x거리(距離), 위에서부터 x 거리만큼 상대적으로 위치를 정하거나, 이동 및 재배치를 지정합니다. Y 방향의 거리는 생략할 수 있지만, 이 경우의 Y방향의 거리는 "0"이 됩니다.
transform:translateX() |translateX(거리) 함수는 좌우(수평 방향)의 이동 거리 값을 지정합니다.
transform:translateY() |translateY(거리) 함수는 상하(수직 방향)의 이동 거리 값을 지정하십시오.
transform:translateZ() |translateZ(거리) 함수는 Z 방향의 거리로 이동을 지정합니다. 이 함수는 백분율 값으로 지정할 수 없습니다. 백분율로 값을 지정해도 "0"이 됩니다.


## transform: scale() – 요소를 X축이나 Y축으로 확대/축소하기

scale은 지정한 크기만큼 x축 또는 y축으로 해당요소를 확대 또는 축소 시킵니다.
scale의 사용법은 아래와 같습니다.

```
transform: scaleX(x축 비율);          // x축으로 확대/축소

transform: scaleY(y축 비율);          // y축으로 확대/축소

transform: scale(x축 비율, y축 비율); // x축, y축으로 확대/축소
```

>scale은 원본크기를 1로 기준으로 하고, 1보다 크면 확대, 1보다 작으면 축소됩니다.
비율값은 소수점이하로도 설정할 수 있습니다.

```css
ul {
  list-style-type: none;
  width: 640px;
  margin: 30px auto 0;
}
ul li {
  float: left;
}
ul li + li {
  margin-left: 20px;;
}
ul li a img {
  width: 200px;
  height: auto;
  box-shadow: 5px 5px 15px #999;
  transition: all 0.4s ease-in-out 0.1s;  /* 마우스 호버하면 자연스럽게 변형되게 */
}
ul li:first-child a:hover img {
  transform: scale(1.2);  /* 마우스 호버하면 세로길이만 1.2배 확대 */
}
ul li:nth-child(2) a:hover img {
  transform: scaleX(2);   /* 마우스 호버하면 가로길이만 2배 확대 */
}
ul li:nth-child(3) a:hover img {
  transform: scale(0.5);  /* 마우스 호버하면 전체 0.5배 축소 */
}
```
## transform: rotate() – 요소 회전하기

rotate는 지정한 각도만큼 웹요소를 회전시킵니다.
rotate의 사용법은 아래와 같습니다.

```
transform: rotateX(ndeg);  // x축을 기준으로 n도 만큼 회전

transform: rotateY(ndeg);  // y축을 기준으로 n도 만큼 회전

transform: rotate(ndeg);   // n도 만큼 회전
```

>회전각도가 플러스값일 때는 시계 방향, 마이너스값일 때는 반시계 방향으로 회전합니다.

```css
ul li:first-child a:hover img {
  transform: rotateY(180deg);  /* 마우스 호버하면 Y축 기준으로 180도 회전 */
}
ul li:nth-child(2) a:hover img {
  transform: rotateX(180deg);  /* 마우스 호버하면 X축 기준으로 180도 회전 */
}
ul li:nth-child(3) a:hover img {
  transform: rotate(-230deg);  /* 마우스 호버하면 반시계방향으로 230도 회전 */
}
```
## transform: skew() – 요소를 X축이나 Y축으로 기울이기

skew는 지정한 각도만큼 웹요소를 기울입니다.
skew의 사용법은 아래와 같습니다.

```
transform: skewX(ndeg);           // x축으로 n도 만큼 기울이기

transform: skewY(ndeg);           // y축으로 n도 만큼 기울이기

transform: skew(x축ndeg, y축ndeg); // x축, y축으로 n도 만큼씩 기울이기
```

```css
ul li:first-child a:hover img {
  transform: skewY(40deg);       /* 마우스 호버하면 Y축으로 40도 기울임 */
}
ul li:nth-child(2) a:hover img {
  transform: skewX(-50deg);      /* 마우스 호버하면 X축으로 -50도 기울임 */
}
ul li:nth-child(3) a:hover img {
  transform: skew(40deg, 20deg); /* 마우스 호버하면 X축으로 40도, Y축으로 20도 기울임 */
}
```
## transform: translate() – 요소를 X축이나 Y축으로 이동

translate는 지정한 각도만큼 웹요소를 기울입니다.
translate의 사용법은 아래와 같습니다.

```
transform: translateX(x축 이동거리);             // x축으로 이동

transform: translateY(y축 이동거리);             // y축으로 이동

transform: translate(x축 이동거리, y축 이동거리); // x축, y축으로 동시 이동
```

```css
ul li:first-child a:hover img {
  transform: translateY(50px);        /* 마우스 호버하면 Y축으로 50px 이동 */
}
ul li:nth-child(2) a:hover img {
  transform: translateX(50px);        /* 마우스 호버하면 X축으로 50px 이동 */
}
ul li:nth-child(3) a:hover img {
  transform: translate(-50px, -50px); /* 마우스 호버하면 X축으로 -50px, Y축으로 -50px 이동 */
}
```



---

## reference
