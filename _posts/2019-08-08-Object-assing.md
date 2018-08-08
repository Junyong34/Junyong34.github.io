---
title: 자바스크립트 함수 표현식, 함수 선언식
date: 2018-08-08
category: javascript
tags:
  - javascript
  - function
---

# 함수 선언식

일반적으로 많이 쓰는 함수 선언식
```js
function 함수명() {
  로직 작성
}
```

```js
function fn_js() {
  return '안녕하세요';
}
js();
```

# 함수 표현식
자바스크립트 특징을 사용한 선언식

```js
var 함수명 = function () {
  로직 작성
};
```

```js
// 예시
var fn_js = function () {
    return '안녕하세요';
}
fn_js();
```

## 함수 선언식과 표현식의 차이점
**함수 선언식은 호이스팅에 영향을 받는다(var명령문과 유사하게 함수 선언은 다른코드의 맨위로 올가간다., 함수 표현식은 호이스팅에 영향을 받지 않는다.**




```js
// 실행 전
fn_01();
fn_02();

function fb_01() {
  return '실행';
}

var fn_02 = function () {
  return '실행2';
};
```

호이스팅에 의해 자바스크립트 파서 할때 아래 처럼 해석한다.

```js
// 실행 시
function fn_01() {
  return '실행';
}

var fn_02;

fn_01(); // '실행'
fn_02(); // Uncaught TypeError: sumNumbers is not a function

fn_01 = function () {
  return '실행2';
};
```


## 함수 표현식의 장점
'함수 표현식이 호이스팅에 영향을 받지 않는다'

- 클로져 사용


#### 함수 표현식으로 클로져 생성하기
클로져는 함수를 실행하기 전에 해당 함수에 변수를 넘기고 싶을 때 사용된다.
더 쉽게 이해하기 위해 아래 예제를 살펴보자.

```js
function fn_01(key) {
    return function fn_closure(args) {
        console.log(key);
    };
}
```



 <a href="https://www.sitepoint.com/function-expressions-vs-declarations/">Site Point 의 Function Expressions vs Function Declarations</a>
