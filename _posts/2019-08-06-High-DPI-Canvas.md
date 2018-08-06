---
title: High DPI Canvas
date: 2018-08-06
category: javascript
tags:
  - canvas
---
## High DPI Canvas

캔버스로 작업한 UI를 일반 모니터와 맥북의 고해상도 모니터에서 비교해서 보게 되면 캔버스 픽셀들이 찌그러지거나 깨져서 보이는 현상이 있다.

canvas에 scaler를 통해 화면 비율에 따라 축소 처리를 할 수있는 메서드를 제공한다.


### devicePixelRatio property

Window.devicePixelRatio 속성은 읽기 전용이고 현재 하나의 CSS 픽셀에 대한 현재 디스플레이 장치 상의 하나의 물리적인 픽셀의 비율을 반환한다. 페이지가 확대되면 CSS 픽셀에 대응하는 장치의 픽셀 수는 증가한다. 그렇기 때문에 devicePixelRatio 또한 증가한다.

픽셀 넓이에 이미지를 만들고 그려지면서 devicePixelRatio에 의해 이미지가 확대되어 흐리게 보입니다.

![canvas-dpi-1](https://user-images.githubusercontent.com/25451713/43692392-7edd908c-9962-11e8-9e03-25ba478d8f67.PNG)

devicePixelRatio로 인해 이미지가 업 스케일되고 흐려짐

위에 이미지가 흐려진거에 대한 해결책은 devicePixelRatio에 의해 확장 된 이미지를 생성 한 다음 CSS를 사용하여 같은 크기로 축소하는 것이다.


### 캔버스가 이미지를 그리는 순서

캔버스는 그림을 그릴때 backing store라는 곳에서 미리 그림을 그리고 실제 캔버스에 그림을 그대로 옴기는 작업을 하게된다.

이때 캔버스 widht는 200px에 devicePixelRatio,backingStoreRatio값이 2가 되면 backing store 넓이는 비율로 인해 400px 만큼 커지게 되면서 캔버스에 그림을 옴길때 이미지가 흐려지는 현상이 발생 하게 된다. 특히 애플 레니타 고해상도에서는 더 흐려짐이 잘보인다.

![canvas-backing-store](https://user-images.githubusercontent.com/25451713/43692376-67436622-9962-11e8-9c66-756876d9b67e.PNG)



우리는 캔버스 생성시 몇가지 코드를 넣어서 해당 문제를 해결할 수 있다.


```javascript
function drawImage(opts) {

    if(!opts.canvas) {
        throw("A canvas is required");
    }
    if(!opts.image) {
        throw("Image is required");
    }

    // get the canvas and context
    var canvas = opts.canvas,
        context = canvas.getContext('2d'),
        image = opts.image,

    // now default all the dimension info
        srcx = opts.srcx || 0,
        srcy = opts.srcy || 0,
        srcw = opts.srcw || image.naturalWidth,
        srch = opts.srch || image.naturalHeight,
        desx = opts.desx || srcx,
        desy = opts.desy || srcy,
        desw = opts.desw || srcw,
        desh = opts.desh || srch,
        auto = opts.auto,

    // finally query the various pixel ratios
        devicePixelRatio = window.devicePixelRatio || 1,
        backingStoreRatio = context.webkitBackingStorePixelRatio ||
                            context.mozBackingStorePixelRatio ||
                            context.msBackingStorePixelRatio ||
                            context.oBackingStorePixelRatio ||
                            context.backingStorePixelRatio || 1,

        ratio = devicePixelRatio / backingStoreRatio;

    // ensure we have a value set for auto.
    // If auto is set to false then we
    // will simply not upscale the canvas
    // and the default behaviour will be maintained
    if (typeof auto === 'undefined') {
        auto = true;
    }

    // upscale the canvas if the two ratios don't match
    if (auto && devicePixelRatio !== backingStoreRatio) {

        var oldWidth = canvas.width;
        var oldHeight = canvas.height;

        canvas.width = oldWidth * ratio;
        canvas.height = oldHeight * ratio;

        canvas.style.width = oldWidth + 'px';
        canvas.style.height = oldHeight + 'px';

        // now scale the context to counter
        // the fact that we've manually scaled
        // our canvas element
        context.scale(ratio, ratio);

    }

    context.drawImage(pic, srcx, srcy, srcw, srch, desx, desy, desw, desh);
}
```


위의 코드에 `devicePixelRatio 과 backingStoreRatio을 통해 ratio 구한다.`

실제 캔버스 속성에 width , height값에는 ratio을 곱하여 사이즈를 정하고
캔버스 css에는 비율을 곱하지 않은 width,height값을 설정한다.
그리고 마지막에 context에 scale 매서드를 통해 다시 비율만큼 축소 시켜서 비율을 맞춰준다

해당로직을 캔버스 생성시 추가하게 되면 확실히 이미지차이를 눈으로 확인 할 수 있다.






---

## reference
- [참고 블로그](https://www.html5rocks.com/en/tutorials/canvas/hidpi/)
- [예제](https://bl.ocks.org/cmgiven/f2100df55e076f386c13ada4988b75e9)

