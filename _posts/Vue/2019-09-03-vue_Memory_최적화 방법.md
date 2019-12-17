---
title: Vuejs 사용 Memory 최적화
date: 2019-09-03
category: javascript
tags:
  - vue
  - vuex
---

# Vue.js 데이터 모델 최적화
## Front-End

### JS Heap Memory 최소화
- 가공이 필요한 데이터
  - back단에서 할 수 있는 작업은 back단에 요청 
  - front에서 해야하는부분 따로 utils.js로 빼서 작업 진행
  - 필터링 변수가 많은 경우 화면 computed / Watch에서 작업  Vuex-getter에서 작업 금지
  - 필터링 없는 단순 데이터 출력이라면 Vuex-getter에서 작업 진행
- 가공이 필요없는 데이터
  - 가공이 필요 없고 단순 데이터 출력이라면 Vuex에 담지 않고 바로 화면에서 바로 출력
  - 글로벌 변수를 사용한 데이터는 Vuex에 담아서 반응하도록 셋팅해서 사용
- beforeDestory
  - Event Bus 제거
  - 사용자 정의 Event 제거
  - 객체 초기화 처리
  - Dom 생성 초기화 처리




### ※ defineReactive 적용이 필요한 데이터( getter/setter)
Vue에서 Data/ computed/ watch / Vuex state를 사용하게 되면 반응형이 자동으로 등록이 된다. 

반응형이 적용이 되는 순간 Memory를 더 많이 사용하게 된다.  Memory 사용량을 줄이기 위해서는 필요 없는 반응형은 제거 해야함.



#### Object.freeze() - 사용
- 대용량 데이터는 Vuex에서 Object.freeze()를 사용하여 객체를 readonly 처리를 한다. ( freeze를 사용하게 되면 객체에 속성 추가 및 제거 / 수정이 불가능 하다.  Vue에 대한 감지 대상에도 제외)
- Vuex에서 Object.freeze() 사용 API를 통해 데이터를 조회 하고 Mutations에서 state에 데이터를 집어 넣는 곳에서 Object.freeze를 사용하면 된다.
> 주의 사항 Object.freeze()를 사용 하더라도 객체를 복사 하게되면 ( map, splice, _.cloneDeep)  freeze 대상이 되지 않기 떄문에 최종적으로 Object.freeze()로 Vue 감지 대상에서 제거를 해야한다.

## Back-End
 <b>API 데이터</b>
- fornt에서 데이터 가공하고 merge하는 작업을 서버단에서 처리 가능한 API 개선필요
- Object depth가 긴 데이터를 데이터 포멧 변경으로 최적화



## reference


