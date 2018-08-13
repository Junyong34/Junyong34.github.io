---
title: css selector
date: 2018-08-13
category: selector
tags:
  - css
  - selector
---
# CSS: 선택자(Selector)

## 1.CSS selector

특정 요소를 선택을 해주는 요소 css 규칙에 따라 선택을 할 수 있다.

## 2. Selector의 종류

### 2-1 전체 선택자

패턴 | 설명
----------  | ---- |
&#42; | HTML페이지 내부의 모든 태그를 선택 한다.


```css
* {
  margin: 0px;
  padding: 0px;
}
```
> 전체 선택자를 사용하게 되면 모든 요소를 읽어가야 하기 때문에 로딩속도가 느려질 수 있다. 자주 사용하지 않는것이 좋다.

### 2-2 태그 선택자

패턴 | 설명
----------  | ---- |
E | 태그명이 E인 특정 태그를 선택한다.

```css
div{
  background-color:black;
}
```

> HTML 태그에 스타일을 적용 시킨다 하지만 개발자가 클래스에 스타일 지정하는게 우선순위가 높다.

### 2-3 클래스 선택자

패턴 | 설명
----------  | ---- |
.className | 엘리먼트에 부여된 클래스 속성 이름을 선택한다.

```css
 .className-01 {
   background:yellow;
   width:400px;
   height:200px;
 }
 div.className-02{
   background:black;
   width:400px;
   height:200px;
 }
```

> class 앞에 .을 직어주면 해당 속성값이 지정된다. 또 class명 앞에 엘리먼트를 사욯하면 해당 엘비먼트들 중에 클라스명이 같은 곳에 스타일이 지정된다.


### 2-4 ID 선택자

패턴 | 설명
----------  | ---- |
#ElementID | 엘리먼트에 부여된 ID 값이 일치하는 요소를 선택한다.

```css
 #id-01{
   position:relative;
   top:10px;
   left:30px;
 }
```
> ID 선택자는 #를 사용한다. ID선택자의 우선순위가 클래스 선택자의 우선순위보다 높다. 우선 적용되어야 할 경우 ID선택자를 사용하는것이 더 좋다.

### 2-5 복합 선택자
패턴 | 설명
----------  | ---- |
E F | E요소의 자손인 F요소를 선택한다.
E > F | E요소의 자식인  F요소를 선택한다.

```css
/* 하위 선택자*/
div p {
  border:1px solid black;
}

/* 자식 선택자 */
div > p {
  border:1px solid black;
}

```

>하위 선택자 경우 div 밑에 있는 모든 P를 전부 선택 한다.
>자식 선택자 경우 div 밑 바로 자식 p만 선택 한다.

### 2-6 인접 형제 선택자

패턴 | 설명
----------  | ---- |
E + F | E요소를 뒤따르는 F요소를 선택한다. (E요소가 앞에 전재하면 F를 선택한다.)
E ~ F | E요소의 자식인  F요소를 선택한다.  (E가 F보다 먼저 등장하지 않으면 선택하지 않는다.)

```css
/* 인접 형제 선택자 */
label + p {
  border:1px solid black;
}

/* 일반 형제 선택자 */
label ~ p {
  border:1px solid black;
}
```
같은 부모 요소를 가지는 요소들을 형제 관계라고 말한다.

>인접 형제 선택자는 형제들 중 첫번째 동생 요소가 조건을 충족시킬 때에만 스타일 적용한다.
>(동생이 여러명 있다고 하도 첫번째 요소만 적용)
>일반 형제 선택자는 조건을 충족하는 모든 동생 요소에 스타일을 적용 한다.

두 선택자 모두 형요소에는 적용이 되지 않는다.


### 2-7 속성 선택자

패턴 | 설명
----------  | ---- |
E[attr] | attr을 정의한 요소 E를 선택한다.
E[attr='val'] | attr 속성의 값이 정확하게 val과 일치하는 요소 E를 선택한다.
E[attr~='val'] | attr 속성 값에 val이 포함되는 요소를 선택한다.(띄어 쓰기를 통해 여러개 올 수있는 속성값중 하나만 일치해도 선택 된다.)
E[attr^='val'] | attr 속성 값에 val으로 시작하는 요소를 선택한다.
E[attr$='val'] | attr 속성 값에 val으로 끝나는 요소를 선택한다.
E[attr*='val'] | attr 속성 값에 val이 포함되는 모든요소를 선택한다.
E[attr&#124;='val'] | attr 속성 값이 정확하기 val이거나 val-으로 시작되는 요소 E를 선택한다.

```css
/* E[attr]형식 */
a[href] { background: yellowgreen; color: black; }

/* E[attr="val"]형식 */
input[type="text"] { width: 150px; border: 1px solid #000; }

/* 라디오 버튼이 체크된 것들 선택*/
input[type="radio"]:checked { width: 150px; border: 1px solid #000; }

/* E[attr$="val"]형식 */
a[href$=".xls"] { background: darkgreen; }

```

### 2-8 가상 클래스 선택자

가상클래스는 웹 문서의 소스에는 실제 존재하지 않지만 필요에 의해 임의로 가상의 선택자를 지정한다.


> 링크 선택자(The link pseudo-classes)와 동적 선택자(The user action pseudo-classes)




패턴 | 설명
----------  | ---- |
E:link | 방문하지 않은 링크 E를 선택한다.
E:visited | 방문한 링크 E를 선택한다.
E:active | E요소에 마우스 클릭이나 키보드 엔터가 눌린 동안 E를 선택한다.
E:hover | E요소에 마우스가 올라가 있는 동안 E를 선택한다.
E:focus | E요소에 포커스가 있는 동안 E를 선택한다.

링크 엘리먼트 사용시 링크선택자를 통해 스타일을 줄수 있다.

---

>구조적 가상 클래스 선택자(Structural pseudo-classes)

패턴 | 설명
----------  | ---- |
E:root | 문서의 최상의 요소를 선택한다.
E:nth-child(n) | 앞으로 지정된 순서와 일치하는 요소가 E면 선택한다.
E:nth-last-child(n) | 뒤로부터 지정된 순서와 일치하는 요소 E면 선택한다.
E:nth-of-type(n) | E요소 중 앞으로부터 순서가 일치하는 E요소를 선택한다.
E:nth-last-of-type(n) | E요소 중 끝으로 부터 순서가 일치하는 E요소를 선택한다.
E:first-child | 첫 번째 등장하는 요소가 E면 선택한다.
E:last-child | 마지막에 등장하는 요소가 E면 선택한다.
E:first-of-type | E요소 중 첫번째 E를 선택한다.
E:last-of-type | E요소 중 마지막 E를 선택한다.
E:only-child | E요소가 유일한 자식이면 선택한다.
E:only-of-type | E요소가 유일한 타입이면 선택한다.
E:empty | 텍스트 및 공백을 포함하여 자식요소가 없는 E를 선택한다.


nth-child와 nth-of-type의 가장 큰 차이점은 바로 해당하는 태그의 순서를 말하는지 아니면 부모 속성에서 특정 태그를 가진 자식 속성에서 몇번째 해당하는지의 차이라고 보면된다.

---
- nth-child : 모든 자식의 순서에서 찾음
- nth-of-type: 해당하는 자식 태그 요소에서의 순서를 찾음
---

### 그 외의 선택자

언어선택자

패턴 | 설명
----------  | ---- |
E:lang(ko) | HTML LANG 속성의 값이 'ko'으로 지정된 요소를 선택한다.

부정선택자

패턴 | 설명
----------  | ---- |
E:not(S) | S가 아닌 E요소를 선택 한다.

목적 선택자

패턴 | 설명
----------  | ---- |
E:target | E의 URI가 요청되면 선택합니다. (따라서 E는 ID가 지정되어 있어야 합니다.)

UI 요소 선택자.

패턴 | 설명
----------  | ---- |
E:enabled | 사용 가능한 폼 콘트롤(input, textarea, select, button) E를 선택합니다.
E:disabled | 사용 불가능 폼 콘트롤(input, textarea, select, button) E를 선택합니다.
E:checked | 선택된 폼 컨트롤(checkbox,radio)) E를 선택합니다.

가상 엘리먼트 선택자

패턴 | 설명
----------  | ---- |
E:first-line | E 요소의 첫 번째 라인을 선택
E:first-letter | E 요소의 첫번째 문자를 선택
E:before | E 요소의 시작 지점에 생선된 요소를 선택
E:after | E 요소의 끝 지점에 생선된 요소를 선택


### css 적용 우선 순위

>!important > id [ 100 ] > class [ 10 ] > tag [ 1 ] > * [ 0 ]

## reference

- [참고블고그](http://circlash.tistory.com/570)

