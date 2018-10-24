---
title: 자바스크립트 WebWorker
date: 2018-09-11
category: javascript
tags:
  - WebWorker
---

# WebSocket

webWorker란 웹 페이지에서 스크립트가 실행되면, 해당 웹 페이지는 실행 중인 스크립트가 종료될 때까지 응답 불가 상태가 됩니다.

web worker는 스크립트가 웹 페이지의 성능에 영향을 미치지 않도록 백그라운드에서 동작하게 해주는 자바스크립트입니다.

즉, web worker는 스크립트의 다중 스레드(multi-thread)를 지원합니다.

따라서 사용자가 웹 페이지를 이용하면서도, 동시에 시간이 오래 걸리는 자바스크립트 작업도 병행할 수 있도록 해줍니다.


### web worker 지원 여부 확인
web worker를 사용하기 전에, 우선 사용자의 웹 브라우저가 이를 지원하는지 안 하는지 확인해야 합니다.
```javascript
if (typeof(Worker) !== "undefined") {
    // web worker를 위한 코드 부분 }
else {
    // web worker를 지원하지 않는 브라우저를 위한 안내 부분
}
```

### web worker 파일 생성

```javascript
 var worker = new Worker("web_worker.js");
```

web_worker.js 파일에서 계산 로직 처리 시간이 긴 로직들을 넣어서 관리한다.

__web_worker.js__

```javascript
self.postMessage('Worker running');
self.onmessage = (evt) => {
    postMessage("Worker received data: " + JSON.stringify(evt.data));
};

```

postMessage를 통해 브라우저로 전달 시킨다.

브라우저에서는
```javascript
worker.addEventListener("message", function(evt) {
    console.log("Message posted from webworker: " + evt.data);
    //console.log evt.data = Worker running
});
```
message 이벤트를 통해 데이타를 전달 받는다

또 브라우저에서 소켓으로 데이타를 전달할 때
```javascript
worker.postMessage({data: "123456789"});
```
브라우저에서 워커 postMessage를 통해 워커로 데이타를 전달한다.

워커 영역에서는 onmessage로 데이타를 받는다

__web_worker.js__

```javascript
self.onmessage = (evt) => {
    postMessage("Worker received data: " + JSON.stringify(evt.data));
    // console.log(data:'123456789')
};

```

### webworker 객체의 실행 종료

web worker 객체는 생성되고 나서 종료될 때까지 계속해서 메시지를 받을 준비를 합니다.

따라서 웹 브라우저나 컴퓨터의 자원을 돌려주기 위해서는 terminate() 메소드를 사용하여 web worker를 반드시 종료해줘야 합니다.
```javascript
worker.terminate();
```
### web worker 객체의 재사용
web worker 객체가 종료된 후에는 web worker의 값을 undefined로 설정해야만 web worker 객체를 재사용할 수 있습니다.
```javascript
worker = undefined;
```
### web worker 스크립트 삽입 함수
> importScripts
```javascript
importScripts("./WebSocket.js");
```
WebSocket.js 스크립트가 webworker scope에 삽입이 되서 사용 할 수 있다.

---



## reference

