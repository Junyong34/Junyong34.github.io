---
title: HTML Element Properties & Window Properties (Width, Height)
date: 2018-08-08
category: javascript
tags:
  - javascript
  - Element
  - Window
---

# Element Properties

## offsetHeight, offsetWidth
>한 객체의 픽셀 단위의 너비와 높이 값으로, margin을 제외한 스크롤 바와 padding, border 값을 포함하고 있다. 웹 표준 규약은 아니지만 여러 브라우저에서 지원하고 있다.

![offset](https://user-images.githubusercontent.com/25451713/43885713-95ceab52-9bf4-11e8-96bd-4aa38eb6a811.png)

---


## clientHeight, clientWidth
>한 객체의 픽셀 단위의 너비와 높이 값으로 padding 값이 포함되나, 그 외의 스크롤 바와 border, margin 값은 포함되지 않는다. 웹 표준 규약은 아니지만 여러 브라우저에서 지원하고 있다.

![client](https://user-images.githubusercontent.com/25451713/43885715-96edcc7a-9bf4-11e8-9dc1-fd8d49ad37b0.png)
---

## scrollHeight, scrollWidth
>보이는 것과 상관 없이 실제 컨텐츠 영역이 얼마만큼의 크기를 갖고 있는지 확인하고 싶다면, scrollWidth와 scrollHeight 속성을 확인하면 된다. 이 속성은 전체 스크롤바를 사용하게 되어 숨겨진 영역까지 포함한 크기를 리턴한다.
---

## scrollLeft, scrollTop

>객체가 스크롤 바를 가지고 있으면, 객체가 얼마만큼 스크롤 되었는지 왼쪽 위 모서리를 기준으로 한 픽셀 단위의 값. 이 속성은 브라우저에 따라 문서의 `<body>` 혹은 `<html>` 태그에도 정의되어 있어서, 문서 전체가 얼마나 스크롤 되었는지를 나타낸다. 주의할 것은, `<iframe>` 태그에서 스크롤 된 값은 무시된다. 웹 표준 규약은 아니지만 여러 브라우저에서 지원하고 있다.

---

## offsetParent
>어떤 특정 객체를 포함하고 있는 상위 객체로서 하위 객체의 offsetLeft 그리고 offsetTop의 기준 좌표 시스템 역할을 한다. 대부분의 경우, offsetParent는 하위 객체를 포함하고 있는 Document object가 되지만, 유동적으로 위치하는(absolute-positioned) 요소의 경우에는 또 이 요소를 포함하고 있는 유동적으로 위치한 상위 요소가 바로 offsetParent가 된다. 복잡한 것은 브라우저마다 offsetParent가 되는 요소를 다르게 가리켜서 주의가 요구된다. (예를 들어, Firefox에서는 root node를 가리키지만, Opera에서는 바로 위의 부모 요소를 가리킨다) 웹 표준 규약은 아니지만 여러 브라우저에서 지원하고 있다.




# Window Properties

## innerWidth, innerHeight
>스크린 영역의 픽셀(pixels) 단위 너비와 높이(읽기 전용). 웹 표준 규약은 아니지만 여러 브라우저에서 지원하고 있다.

---
## innerWidth, innerHeight
>브라우저 창 속 문서가 표시되는 영역의 픽셀 단위 너비와 높이(읽기 전용). 여기에는 메뉴 막대, 도구 막대, 스크롤 바 등이 포함되지 않으나, 예외적으로 Firefox와 Opera에서는 스크롤 바의 영역이 포함된 값을 돌려준다. 이 속성은 IE에서는 지원되지 않고, 버전과 Doctype 선언에 의한 표준 호환 모드냐 아니냐에 따라 document.documentElement 혹은 document.body에 있는 clientWidth와 clientHeight 값을 사용해야 한다.

---

## outerWidth, outerHeight
>브라우저 창 전체의 픽셀 단위 너비와 높이(읽기 전용). 여기에는 메뉴 막대, 도구 막대, 스크롤 바 등이 포함된 값이다. IE에서는 해당 값이 존재하지 않고, 대신 MS 전용 속성인 document.documentElement 혹은 document.body의 offsetHeight, offsetWidth를 써야 한다.

---

## pageXOffset, pageYOffset, scrollX, scrollY
>문서가 얼마 만큼 스크롤 되었는지를 알려주는 픽셀 단위의 값(읽기 전용). 이것도 역시나 Internet Explorer는 지원하지 않는다. 대신 document.documentElement 혹은 document.body의 비표준 scrollLeft와 scrollTop을 사용.
---


# Window Mouse Event Properties
## screenLeft, screenTop, screenX, screenY
>스크린 속 창의 왼쪽 위 구석에 있는 모서리의 위치를 나타내는 좌표 값(읽기 전용). IE, Safari, 그리고 Opera는 screenLeft와 screenTop을 지원하지만, Firefox와 Safari는 screenX와 screenY를 지원한다.
---

### ※Element, Window Size 속성 정리 이미지
![totlalelementsize](https://user-images.githubusercontent.com/25451713/43886034-7c4d506a-9bf5-11e8-9a75-b61d6dbcb5ff.png)

## reference
- [W3schools windobject](https://www.w3schools.com/jsref/obj_window.asp)
- [MDN Web Doc](https://www.w3schools.com/jsref/obj_window.asp)

