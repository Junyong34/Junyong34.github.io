---
title: css Animation
date: 2018-08-07
category: Animation
tags:
  - css
  - Animation
  - keyframe
---
# Animation


애니메이션은 애니메이션을 나타내는 css스타일과 애니메이션의 sequence를 나타내는 복수 키프레임(`@keyframes`)들로 이루어진다.

transition 프로퍼티는 단순히 요소의 프로퍼티 변화에 주안점이 있다면 animation 프로퍼티는 하나의 줄거리를 구성하여 줄거리 내에서 세부 움직임을 시간 흐름단위로 제어할 수 있고 전체 줄거리의 재생과 정지,반복까지 제어 가능하다.

---

<iframe id="cp_embed_oKxep" src="//codepen.io/TaniaLD/embed/oKxep?height=846&amp;theme-id=0&amp;slug-hash=oKxep&amp;default-tab=result&amp;user=TaniaLD" scrolling="no" frameborder="0" height="846" allowtransparency="true" allowfullscreen="true" allowpaymentrequest="true" name="CodePen Embed" title="CodePen Embed 1" class="cp_embed_iframe " style="width: 100%; overflow: hidden;"></iframe>

---

일반적으로 CSS 애니메이션을 사용하면 기존의 JavaScript 기반 애니메이션 실행과 비교하여 더 나은 렌더링 성능을 제공한다고 알려져 있다. 그러나 경우에 따라서는 JavaScript를 사용하는 것이 나을 수도 있다. jQuery 등의 애니메이션 기능은 CSS보다 간편하게 애니메이션 효과를 가능케 한다.

- 비교적 작은 효과나 CSS만으로도 충분한 효과를 볼 수 있는 것은 CSS를 사용한다. 예를 들어 요소의 width 변경 애니메이션은 자바스크립트를 사용하는 것보다 훨씬 간편하며 효과적이다.
- 세밀한 제어를 위해서는 자바스크립트 사용이 바람직하다. 예를 들어 바운스, 중지, 일시 중지, 되감기 또는 감속과 같은 고급 효과는 자바스크립트가 훨씬 유용하다.
<br>

속성 | 설명   |defalut   |
----------  | ---- | :----:
 animation-name|@keyframes 애니메이션 이름을 지정한다. ||
 animation-duration|한 싸이클의 애니메이션에 소요되는 시간을 초 단위(s) or 밀리 초 (ms)로 지정한다. | 0s|
 animation-timing-function|애니메이션 효과를 위한 타이밍 함수를 지정 | ease|
 animation-delay|요소가 로드된 시점과 애니메이션이 실제로 시작하는 사이에 대기하는 시간을 초 단위(s) 또는 밀리 초(ms)로 지정한다. | 0s|
 animation-iteration-count|애니메이션 재생 횟수를 지정한다.|1 |
 animation-direction|애니메이션이 종료된 이후 반복될 때 진행하는 방향 지정 |normal |
 animation-fill-mode|애니메이션 미실행 시(종료or대기)요소읫 스타일 지정 | |
 animation-paly-state| 애니메이션의 재생상태(재생or중지)를 지정한다|running |
 animation|모든 애니메이션의 속석을 한번에 지정한다. | |

<br>

## @keyframes

CSS 애니메이션과 트랜지션 방식의 주된 차이는 @keyframes rule에 있다. 이 rule을 사용하면 애니메이션의 흐름(sequence) 중의 여러 시점(breakpoint)에서 CSS 프로퍼티값을 지정할 수 있다..

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    div {
      position: absolute;
      width: 100px;
      height: 100px;
      background-color: red;
      animation-name: move;
      animation-duration: 5s;
      animation-iteration-count: infinite;
    }
    /* @keyframes rule */
    @keyframes move {
      /* keyframe */
      from {
        left: 0;
      }
      /* keyframe */
      to {
        left: 300px;
      }
    }
  </style>
</head>
<body>
  <div></div>
</body>
</html>
```

>Reslut
><iframe src="animation-01.html"  />

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
