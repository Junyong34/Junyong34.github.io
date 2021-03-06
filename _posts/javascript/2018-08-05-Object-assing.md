---
title: 자바스크립트 ES6 Object.assign
date: 2018-08-05
category: javascript
tags:
  - javascript
  - ES6
---

아래 코드는 Object create를 사용한 코드이다.

```javascript
const infoObj = {
  showName : function(){
    console.log("My name is " + this.name);
  }
}

//아래와 같은 방법으로 프로토타입을 사용한 것 보다 간결하게 나타낼 수 있다.
const myInfo = Object.create(infoObj);

myInfo.name = "JunYong";
myInfo.age = 30;

console.log(myInfo);
/*
{name: "JunYong", age: 30}
  age:30
  name:"JunYong"
  __proto__:
    showName: ƒ showName()
    __proto__:Object
*/
console.log(myInfo.showName()); //"My name is JunYong"
```



아래 코드는 위의 코드에 Object assign을 사용한 코드이다.

`myInfo.name = "JunYong";` 와 같이 계속해서 추가해야하는 수고를 덜어줄 수 있다.

```javascript
const infoObj = {
  showName : function(){
    console.log("My name is " + this.name);
  }
}

const myInfo = Object.assign(Object.create(infoObj), {
  name : "JunYong",
  age : 27
});


console.log(myInfo);
console.log(myInfo.showName());
```





## Object assign 으로 Immutable 객체 만들기

```javascript
const previousObj = {
  name : "JunYong",
  age : 27
};

const myInfo = Object.assign({}, previousObj, {
  name : "Park",
  msg : "Hi"
});

const myInfo2 = Object.assign({}, previousObj, {});


console.log(previousObj); //{name: "JunYong", age: 30}

console.log(myInfo); //{name: "JunYong", age: 30}
console.log(previousObj === myInfo); //false

console.log(myInfo2); //{name: "JunYong", age: 30}
console.log(previousObj === myInfo2);
//false
//내용은 같으나 사실상 다른 객체이다, immutable object
```



---

## reference

