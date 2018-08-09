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
<br>

```css
@keyframes move {}
```
keyframes 뒤에 이름을 정했다.

```css
@keyframes move {
  /* 애니메이션 시작 시점 */
  from { left: 0; }
  /* 애니메이션 종료 시점 */
  to   { left: 300px; }
}
```

## animation-name

 @keyframes 뒤에 애니메이션을 대표하는 임의의 이름를 부여한다.

 ```css
  @keyframes move {
      from { left: 0; }
      to   { left: 300px; }
    }
    @keyframes fadeOut {
      from { opacity: 1; }
      to   { opacity: 0; }
    }
    @keyframes changeColor {
      from { background-color: red; }
      to   { background-color: blue; }
    }

 ```

0부터 left로 300px 이동하고 도착하면 투명도를 1에서 0으로 전환하며 색삭을 변경하는
애니메이션이다.

## animation-duration

 싸이클의 애니메이션에 소요되는 시간을 초 단위(s) 또는 밀리 초 단위(ms)로 지정한다.

```
animation-duration: .5s;
animation-duration: 500ms;
```
animation-duration은 반드시 지정해야 한다. 지정하지 않는 경우 기본값 0s가 셋팅되어 어떠한 애니메이션도 실행되지 않는다.

## animation-timing-function
애니메이션 효과를 위한 수치 함수를 지정한다

##  animation-delay
요소가 로드된 시점과 애니메이션이 실제로 시작하는 사이에 대기하는 시간을 초 단위(s) 또는 밀리 초 단위(ms)로 지정한다.

```
animation-delay: 2s;
```

## animation-iteration-count

```
animation-iteration-count: 3;
```
애니메이션 주기의 재생 횟수를 지정한다. 기본값은 1이며 infinite로 무한반복 할 수 있다.


## animation-direction

애니메이션이 종료된 이후 반복될 때 진행하는 방향을 지정한다.

속성값|설명|
-----|----|
normal|기본값으로 from(0%)에서 to (100%)방향으로 진행한다.|
reverse|to에서 from방향으로 진행|
alternate|홀수번째는 nomarl, 짝수번째는 reverse으로 진행|
alternate-reverse|홀수번째는 reverse, 짝수번째는 nomarl으로 진행|

```css
 div {
      width: 100px;
      height: 100px;
      background: red;
      position: relative;
      animation: myAnimation 5s infinite;
      /*홀수번째는 normal로, 짝수번째는 reverse로 진행*/
      animation-direction: alternate;
    }
    @keyframes myAnimation {
      0%   { background: red;    left: 0px;   top: 0px; }
      25%  { background: yellow; left: 200px; top: 0px; }
      50%  { background: blue;   left: 200px; top: 200px; }
      75%  { background: green;  left: 0px;   top: 200px; }
      100% { background: red;    left: 0px;   top: 0px; }
    }
```

## animation-fill-mode
애니메이션 미실행 시(대기 또는 종료) 요소의 스타일을 지정한다.

속성값|상태|설명
----|:---:|-----
none|대기|시작 프레임(from)에 설정한 스타일을 적용하지 않고 대기한다.|
″   |종료|애니메이션 실행 전 상태로 애니메이션 요소의 프로퍼티값을 되돌리고 종료한다.|
forwards|대기|시작 프레임(from)에 설정한 스타일을 적용하지 않고 대기한다.|
″   |종료|종료 프레임(to)에 설정한 스타일을 적용하고 종료한다.|
backwards|대기|시시작 프레임(from)에 설정한 스타일을 적용하고 대기한다.|
″   |종료|애니메이션 실행 전 상태로 애니메이션 요소의 프로퍼티값을 되돌리고 종료한다.|
both|대기|시작 프레임(from)에 설정한 스타일을 적용하고 대기한다.|
″   |종료|종료 프레임(to)에 설정한 스타일을 적용하고 종료한다.|


```html
<!DOCTYPE html>
<html>
<head>
  <style>
    div {
      width: 100px;
      height: 100px;
      font: bold 1em/100px san-serif;
      text-align: center;
      color: #fff;
      background: red;
      margin-bottom: 10px;
      position: relative;
      /*name duration timing-function delay iteration-count direction fill-mode play-state*/
      animation: myAnimation 2s linear 2s;
    }
    div:nth-of-type(1) {
      animation-fill-mode: none;
    }
    div:nth-of-type(2) {
      animation-fill-mode: forwards;
    }
    div:nth-of-type(3) {
      animation-fill-mode: backwards;
    }
    div:nth-of-type(4) {
      animation-fill-mode: both;
    }
    @keyframes myAnimation {
      0%   { left: 0px;   background: yellow; }
      100% { left: 200px; background: blue; }
    }
  </style>
</head>
<body>
  <h1>animation-fill-mode</h1>

  <div>none</div>
  <p>대기 : 시작 프레임(from)에 설정한 스타일을 적용하지 않고 대기한다.</p>
  <p>종료 : 애니메이션 실행 전 상태로 애니메이션 요소의 프로퍼티값을 되돌리고 종료한다.</p>

  <div>forwards</div>
  <p>대기 : 시작 프레임(from)에 설정한 스타일을 적용하지 않고 대기한다.
  <p>종료 : 종료 프레임(to)에 설정한 스타일을 적용하고 종료한다.

  <div>backwards</div>
  <p>대기 : 시작 프레임(from)에 설정한 스타일을 적용하고 대기한다.
  <p>종료 : 애니메이션 실행 전 상태로 애니메이션 요소의 프로퍼티값을 되돌리고 종료한다.

  <div>both</div>
  <p>대기 : 시작 프레임(from)에 설정한 스타일을 적용하고 대기한다.
  <p>종료 : 종료 프레임(to)에 설정한 스타일을 적용하고 종료한다.
</body>
</html>
```

## animation-play-state
애니메이션 재생 상태(재생 또는 중지)를 지정한다. 기본값은 running이다.

```css
div {
      width: 100px;
      height: 100px;
      background: red;
      position: relative;
      /*name duration timing-function delay iteration-count direction fill-mode play-state*/
      animation: move 5s infinite;
    }
    div:hover {
      background: blue;
      animation-play-state: paused;
    }
    div:active {
      background: yellow;
      animation-play-state: running;
    }
    @keyframes move {
      from { left: 0px; }
      to   { left: 200px; }
    }
```

## animation
모든 애니메이션 프로퍼티를 한번에 지정한다. 값을 지정하지 않은 프로퍼티에는 기본값이 지정된다. 지정 방법은 다음과 같다.
```
animation: name duration timing-function delay iteration-count direction fill-mode play-state
```

animation-duration은 반드시 지정해야 한다. 지정하지 않는 경우 기본값 0s가 셋팅되어 어떠한 애니메이션도 실행되지 않는다. 기본값은 아래와 같다.
```
none 0 ease 0 1 normal none running
```

## reference

- [참고1-css 자동완성](http://www.css3generator.com/)
- [참고2](https://css3gen.com/wp-content/cache/all//index.html)
- [참고3](https://www.cssmatic.com/box-shadow)
