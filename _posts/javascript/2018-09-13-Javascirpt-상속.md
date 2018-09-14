---
title: 자바스크립트 WebWorker
date: 2018-09-13
category: javascript
tags:
  - 상속
---

# Javascript 상속

Line Chart를 만든다고 했을때
공통적인 요소 객체를 만들고
해당 객체가 LineChart를 상속 받는 구조를 만든다고 했을 때,

**공통영역 스크립트 (부모)**
```javascript
var XMCanvas = (function () {
    function XMCanvas(args) {
        if (!(this instanceof XMCanvas)) {
            return new XMCanvas(args);
        }

        this.width = 0; // 캔버스 넓이
        this.height = 0; // 캔버스 높이


    }

    XMCanvas.prototype.initProperty = function (arg) {
        this.target = null;                                       // 차트가 생성될 레이어
        this.displayCanvas = document.createElement('canvas');    // 메인 뷰 캔버스
        this.displayCtx = this.displayCanvas.getContext('2d');
        this.bufferCanvas = document.createElement('canvas');     // 버퍼 캔버스
        this.bufferCtx = this.bufferCanvas.getContext('2d');
        this.overlayCanvas = document.createElement('canvas');    // overlay 캔버스
        this.overlayCtx = this.overlayCanvas.getContext('2d');

        var devicePixelRatio = window.devicePixelRatio || 1,
            backingStoreRatio =
                this.displayCtx.webkitBackingStorePixelRatio ||
                this.displayCtx.mozBackingStorePixelRatio ||
                this.displayCtx.msBackingStorePixelRatio ||
                this.displayCtx.oBackingStorePixelRatio ||
                this.displayCtx.backingStorePixelRatio || 1;

        this.pixelRatio = devicePixelRatio / backingStoreRatio;
        this.oldPixelRatio = this.pixelRatio;

        if (devicePixelRatio !== backingStoreRatio) {
            this.bufferCtx.scale(this.pixelRatio, this.pixelRatio);
            this.overlayCtx.scale(this.pixelRatio, this.pixelRatio);
        }

        for(var key in arg){
            this[key] = arg[key];
        }

        arg = null;
    };
    XMCanvas.prototype.setWidth = function(width){
        this.displayCanvas.width = width * this.pixelRatio;
        this.displayCanvas.style.width = width + 'px';
        this.bufferCanvas.width = width * this.pixelRatio;
        this.bufferCanvas.style.width = width + 'px';
        this.overlayCanvas.width = width * this.pixelRatio;
        this.overlayCanvas.style.width = width + 'px';
    };

    XMCanvas.prototype.setHeight = function(height){
        this.displayCanvas.height = height * this.pixelRatio;
        this.displayCanvas.style.height = height + 'px';
        this.bufferCanvas.height = height * this.pixelRatio;
        this.bufferCanvas.style.height = height + 'px';
        this.overlayCanvas.height = height * this.pixelRatio;
        this.overlayCanvas.style.height = height + 'px';
    };
    return XMCanvas;
})();

```

실제 lineChart를 그리는 자식 스크립트

부모를 상속 받는다.

```javascript
var XMLineChart = (function () {
        function XMLineChart(args) {
            if (!(this instanceof XMLineChart)) {
                return new XMLineChart(args);
            }
            // XMCanvas.call(this, args);
            this.width = 0; // 캔버스 넓이
            this.height = 0; // 캔버스 높이
            this.maxValueTip = {
                show: true,
                fix: false
            }
        }

        // 상속
        XMLineChart.prototype = Object.create(XMCanvas.prototype);
        XMLineChart.prototype.constructor = XMCanvas;

        XMLineChart.prototype.initProperty = function (arg) {
            this.target = null;																			// 차트가 생성될 레이어

        };
```

부모 객체에 prototype 안에 자식 캔버스를 만들며
constructor를 자식캔버스로 바꾸며 상속 처리를 한다.


```javascript
 XMLineChart.prototype = Object.create(XMCanvas.prototype);
 XMLineChart.prototype.constructor = XMCanvas;
```

에를 들어
XMLineChart 객체에서 부모에 함수를 먼저 찾아간후 없으면 자신의 함수를 실행한다.

xmlLinChart.setWidth(300);
호출하게 되면 XMCanvas에 setWidth를 먼저 찾아가고 없는 경우 XMLineChart에 setWidth를 찾아가서 실행 시킨다.

## reference

