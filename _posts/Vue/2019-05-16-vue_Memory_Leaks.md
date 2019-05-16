---
title: Vuejs 사용 Memory leaks
date: 2019-05-16
category: javascript
tags:
  - vue
  - vuex
  - SPA
---

# vue 대용량 데이터 처리 및 메모리 관리

vuejs를 이용해 모니터링 화면을 개발 진행을 했었다. 개발을 1/3 정도 했을 때 갑자기 생각도 못했던 크롬에서 앗!이런이라는 문구가 뜨면서 웹페이지가 뻗어버리는 현상을 겪었다. 해당 이슈로
인해 여기저기 검색한 노하우를 정리 해봤다.


1.SPA(single Pageee Application)
SPA를 하게 되면 메모리누수가 당연히 발생하기 마련이다. 페이지 이동이 따로 없기 때문에
메모리를 자동으로 초기화가 되지 않는데 그렇게 때문에 항상 변수, 객체등을 초기화를 반드시 해줘야 한다.

2.대량 데이터 및 대량에 Vuex 데이터 처리
VueX를 사용하거나 component *.vue 파일 data를 사용하게 되면 내부적으로 getter, setter가
생성이 된다. 실제 console.log로 vue에 할당한 데이터를 찍어보면 Observer 패턴이 물려 있고
각 key값마다 getter/setter가 생성 되어있다 .
이로인해 데이터가 변경이 되는걸 프록시를 통해 감지하여 바로바로 화면에 반영을 할 수 있다.

문제는 하나에 객체 데이터가 엄청나게 많은 자식데이터 및 array 데이터가 있다면 해당 데이터 모든 key값에 getter/setter를 다 생성한다 여기서 고비용의 메모리가 사용이 된다. 그렇게 되면 자동으로 js Heap Memory 사이즈도 증가 되게 된다.

그래서 대량의 데이터를 가지고 있는 화면은 반응형 관리 대상에서 제외를 시켜서 처리하는 방법이
있다. 일반적으로 나 같은경우는 대량에 데이터를 Table에 row마다 뿌려주는 경우 메모리가 엄청 많이 쌓이는 걸 볼 수 있었다.

vue GitHub가서 이슈를 검색해보게 되면 에반유가 반응성을 끊기위해서는 Object.freeze()를 사용 하라고 알려준다.

Object.freeze() 사용하게 되면 readOnly 처리가 되기 때문에 연결 되어있던 Observer 패턴 연결이 다 사라진걸 볼 수 있다.

상용 Grid에서도 메모리관련해서 Object.freeze() 사용해서 처리하라는 예시들이 있다

예시 코드를 보게되면


### 반응형 패턴 제거해서 메모리 소비랑 감소하기.
```javascript
mounted() {
    const rowData = this.$store.getters.rowData;
    this.rowData = Object.freeze(
        rowData.map(row => {
            return {
                ...row
            }
        })
    )
}.
```

getter에서 받은 데이터를 반응형을 끊고 table에 데이터를 사용하고 있다.


3. 비동기 호출 타이머를 사용시 꼭 제거

4. 무분별한 Closures의 사용

5. new 키워드를 통한 인스터스 생성 반드시 beforeDestroy Hook에서 제거 및 Null 처리

6. SPA  환경에서 단일 Vue-router를 사용하는데 이때 메모리 누수 실제 발생 가능 주의

7. 크롬 개발자도구 vue 확장프로그렘 레코딩를 사용하면 메모리를 많이 사용하게됨, 메모리 관련
체크할때 vue 확장프로그램 해제 및 레코딩 중지하고 테스트 해야한다.

8. keep-alive 태그 사용시 캐싱이 되기 떄문에 Destroy Hook evnet를 타지 않는다. 그렇기 떄문에 다른 2개 라이프사이클인 activated, deactivated Hook에서 초기화 로직을 수행해 줘야한다.



## reference
- [참고 vue 문서](https://vuejs.org/v2/cookbook/avoiding-memory-leaks.html)
- [참고 vue github issues](https://github.com/vuejs/vue/issues/4384)
- [참고 vue github issues](https://github.com/vuejs/vuex/issues/1507)
- [참고 vue github issues](https://github.com/vuejs/vue-devtools/issues/210)


