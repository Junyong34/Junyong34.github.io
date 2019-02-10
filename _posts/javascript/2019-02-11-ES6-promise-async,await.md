---
title: 자바스크립트 비동기 관리 Promise async , await
date: 2018-02-11
category: javascript
tags:
  - Promise
  - async
  - await
---

# 비동기 처리 Promise, async, await

비동기 함수 setTimeout, ajax, axios 등등 실제로 많이 사용 하게 된다.
개발을 하다 보면 비동기 함수를 동기적으로 처리해야 상황이 자주 발생한다.
특히 데이터 처리에 있어서 merge를 해야하는 경우 A,B 데이터를 서버에서 가져오는데
A가 우선순위가 높은 경우 A데이터 호출이 끝나는 시점에 B를 호출 해야한다. 이런 경우 사용 하게 된다.

> 실무에서 자주 사용하는 코드를 예시로 작성했다. Promise, async, await 개념 관련 글은 아니다.

## 비동기 함수 setTimeout를 가지고 예시를 작성했다.

### Promise 사용 동기처리
```javascript
// 비동기 함수
function delay(sec, callback) {
        setTimeout( () => {
            callback(new Date().toISOString());
        }, sec * 1000);
    }

// 아래와 같이 비동기 함수 3개를 1초 딜레이를 주고 호출한다.
delay(1, (r) => {
        console.log(1, r);
});
delay(1, (r) => {
        console.log(2, r);
});
delay(1, (r) => {
        console.log(3, r);
});

// 비동기 함수가 아니라면 1초 2초 3초 순서대로 실행이 되지만
// 비동기 함수이기 떄문에 3개가 바로 동시에 호출이 된다.

// 해당소스를 순서대로 호출 하기 위해서는 아래와 같이 코드를 작성하면 된다.
// Promise를 알지 못 할 떄 나도 아래와 같은 콜백 방식을 사용했다.

delay(1, (r) => {
        console.log(1, r);
        delay(1, (r) => {
            console.log(2, r);
            delay(1, (r) => {
                console.log(3, r);
            });
        });
    });

// 위와 같이 호출하게 되면 순서는 보장 되지만 콜백 함수가 증가 하게 되면 관리도 힘들고
// 눈으로 따라가기도 힘들며 콜백지옥에 빠지게 된다.


// 콜백지옥을 해결하기 위해 Promise가 있다. pRomise를 return을 하게되면 된다.
// delay 함수를 Promise를 리턴하게 변경 하였다.

function delayP(sec) {
  // Promise를 리턴한다.
        return new Promise( (resolve, reject) => {
            setTimeout( () => {
                resolve(new Date().toISOString());
            }, sec * 1000);
        })
    }
// 정상처리 resolve, 오류 발생 reject 콜백함수를 실행하게 된다.
// Promise를 리턴하게 되면 체인함수 then을 사용하여 결과값을 받을 수 있다.
delayP(1).then( (r) => {
  console.log(r);
});
delayP(1).then( (r) => {
  console.log(r);
});
delayP(1).then( (r) => {
  console.log(r);
});

// 위와 같이 Promise를 사용했지만 위와 같이 사용하면 동기적인 처리가 불가능하고 동시에 실행이 된다.

// Promise는 항상 리턴을 값을 넘겨줘야 동기적인 처리가 가능하다.
// 아래와 같이 사용하게 되면 1,2,3 순서대로 실행하게 된다.

delayP(1).then( (r) => {
  console.log(1,r);
  return delayP(1);
}).then( (r) => {
  console.log(2,r);
  return delayP(1);
}).then( (r) => {
  console.log(3,r);
});

// Promise를 리턴하고 then으로 체인 처리하여 1,2,3 동기적으로 호출이 된다.
```

### aysnc , await 이용한 동기 처리
```javascript

// 동기처리를 해야하는 함수는 async를 함수앞에 작성하면 된다.
async function my_Fn() {
        return "async"
    }
    function myFn() {
        return "func"
    }
// 실제로 함수를 console을 찍게되면
    console.log(my_Fn()); // Promise 리턴 한다.
    console.log(myFn()); // func를 리턴한다.

    // async를 사요하면 Promise를 리턴한다는걸 확인 할 수 있다.


// 비동기 함수를 하나 만든다.
    function delayP(sec) {
  // Promise를 리턴한다.
        return new Promise( (resolve, reject) => {
            setTimeout( () => {
                resolve(new Date().toISOString());
            }, sec * 1000);
        })
    }

    async function my_Fn2() {
      // 비동기함수에 awiat를 사용하여 처리가 다 끝날때까지 대기 시긴다.
      const time = await delayP(1);
      console.log(time);
        return "async"
    }

    my_Fn2().then( (r) => {
      console.log(r);
    })
    // 위 함수가 실행되면 time이 찍히고 async 다음으로 찍히게 된다.

    // Promise같은 경우 resolve, reject 함수를 통해 성공, 실패를 처리했지만 async같은 경우는 resolve만 리턴하기 때문에 에러처리 같은경우는 try ... catch함수를 통하여 catch에서 실패 콜백함수를 작성하여 사용하게 된다.

async function another() {
  try {
    let result = await delayP(1);
  } catch (err) {
    // 에러 처리 작성
    console.error(err);
  }
}

```
## reference

