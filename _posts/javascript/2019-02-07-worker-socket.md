---
title: 자바스크립트 WebWorker & WebSocket & Axios
date: 2019-02-18
category: javascript
tags:
  - WebWorker
  - WebSocket
  - Axios
---

# WebWorker & WebSocket & Axios

실시간 데이타 처리 해야할 일이 생긴다면 생각하는게 웹소켓이다.
자바스크립트는 싱글 스레드로 돌아가기 때문에 실시간 데이터 처리에 계산 복잡도가 높아 진다면
웹 워커도 사용을 해야한다.

그래서 실무에서 실제로 사용하기 위해 작성한 코드 리뷰를 하려고 한다.

나는 웹워커 위에 비동기 서블릿(axios), 웹소켓 데이타 처리를 워커에서 사용 했다.
(참고 Vue 프로잭트에서 사용)

### 사용한 파일 리스트
- WebCaller.js (워커를 생성하는 파일)
- PCMworker.js (다른 스레드에서 동작하는 웹 워커 파일)
- PCMSocket.js (웹소켓을 생성하는 파일)
- PCMAxios.js  (Axios 비동기 라이브러리 생성 하는 파일)
- PCMServlet.list.js (비동기 서블릿 API를 관리하는 파일)
- SocketDataModule.js (워커 스레드를 이용하여 소켓 데이터 파서 및 가공 처리하는 파일)
- ServletDataModule.js (워커 스레드를 이용하여 서블릿 데이터 파서 및 가공 처리하는 파일)


## WebCaller.js

WebCaller는 worker를 생성하고 워커에 서블릿 정보, 소켓정보를 전달하게 된다. 또 워커에서 데이타를 받아서 화면에 뿌려준다.
(vue.js에서 사용 했다.)

```javascript
// WebCaller.js
import * as apiTypes from "./PCMServlet.list";
import WebWorker from "worker-loader!./PCMworker";
// import WebWorker from "./PCMworker";

// import WebWorker from "./PCMWoker";

class WebCaller {
    constructor(props) {
        // 헤더정보
        this.initProperty();
        Object.keys(props).forEach(key => {
            this[key] = props[key];
        });
        this.initWorker();
    }

    initProperty() {
        this.name = "worker";
        this.ip = "10.10.30.20";
        this.port = "8080";
        this.servletPath = "/v1/api/";
        this.vm = null;
        // this.worker = null;
    }


    initWorker() {
        this.worker = new WebWorker();
        // 아이피 정보 전달
    }

    initSocket() {
        const config = {ip: "10.10.30.20", port: "8080", type: "connect"};

        this.worker.postMessage(config);

        this.worker.addEventListener("message", data => {

            console.dir(data);
        });
    }

    initServlet(callList) {
        const config = {
            ip: this.ip,
            port: this.port,
            servletPath: this.servletPath,
            type: "servlet",
            list: [...callList],
        };

        this.worker.postMessage(config);

        this.worker.addEventListener("message", event => {
            // console.dir(this.vm);
            this.workerOnMsg(event.data);
        });
    }

    initServlet2(callList) {
        const config = {
            ip: this.ip,
            port: this.port,
            servletPath: "/api/",
            type: "servlet",
            list: [...callList],
        };

        this.worker.postMessage(config);

        this.worker.addEventListener("message", event => {
            // console.dir(this.vm);
            this.workerOnMsg(event.data);
        });
    }

    workerOnMsg = data => {
        const dataType = data.type;

        switch (dataType) {
            case apiTypes.COMMMON:
                // vuex 코드에 넣기
                this.vm.$store.dispatch("store/common", data);
                break;
            case apiTypes.NODE_SUMMARY:
                this.vm.$store.dispatch("store/node_summary", data);
                break;
            case apiTypes.RUNNING_PODS_BY_DEPLOYMENT:
                this.vm.$store.dispatch("store/running_pods_by_deployment", data);
                break;
            case apiTypes.TOP_RESOURCE:
                // debugger;
                this.vm.$store.dispatch("store/top_resouce", data);
                break;
            case apiTypes.TOPOLOGY:
                // debugger;
                this.vm.$store.dispatch("store/topologyData", data);
                break;
            case apiTypes.DATACENTER:
                this.vm.$store.dispatch("store/dataCenter", data);
                break;
            default:
        }
    }

    workerClose() {
        this.worker.terminate();
    }
}

export default WebCaller;

```

## PCMworker.js

PCMWorker는 실제 워커 스레드에서 실행되는 파일이다. 워커 스레드에서 소켓 데이터를 받고, 또 서블릿 비동기 데이타도 받아서 처리한다.

```javascript
import WebSocketCaller from "./PCMSocket";
import Service from "./PCMAxios";
import apiData from "./ServletDataModule";

self.onmessage = function (e) {

    const data = e.data;
    const IP = data.ip;
    const PORT = data.port;
    if (data.type === "connect") {
        const conifg = {
            url: "http://" + IP + ":" + PORT + "/ui-server-websocket",
        };
        const webSocketIns = new WebSocketCaller(conifg);
        webSocketIns.wsOpen();
    }
    if (data.type === "servlet") {
        const callList = data.list;
        const servletPath = data.servletPath;
        const commonInfo = {
            URL: `http://${IP}:${PORT}${servletPath}`,
            // URL: "http://localhost:3000",
            HEADERS: {
                Accept: "application/json",
                "Content-Type": "application/json",
            },
        };
        const serviceIns = new Service(commonInfo);

        apiData.apiCaller(serviceIns, callList);

    } else {
        console.log("not Working Data", data)
    }

};


```

## PCMSocket.js

PCMSocket는 소켓에 관련된 전반적인 이벤트 및 웹 소켓통신에 있어서 기본적인 로직 처리를 할 수 있도록 만들었다.
여기서 부터 케이스별로 확장해 나가면 된다.

```javascript
class SocketModule {
    constructor(props) {
        // 헤더정보
        this.initProperty();
        Object.keys(props).forEach(key => {
            this[key] = props[key];
        });

        if (this.automaticOpen === true) {
            this.wsOpen();
        }
    }

    initProperty() {
        // Default settings

        // 로그 찍어보는 플래그
        this.debug = false;

        // 웹소켓이 바로 연결 시도 여부
        this.automaticOpen = false;

        // 재연결 주기
        this.reconnectInterval = 1000;
        // 재연결를 시도 할 수 있는 최대 시간
        this.maxReconnectInterval = 30000;
        // 재연결 지연 증가율. 문제가 지속될 경우 다시 연결 시도 허용
        this.reconnectDecay = 1.5;

        // 연결을 닫고 다시 시도하기 전에 연결에 성공할 때까지 대기할 최대 시간(밀리초).
        this.timeoutInterval = 2000;

        // 최대 재연결 시도 횟수. null인 경우 제한 없음
        this.maxReconnectAttempts = null;

        // 이진 형식; 가능한 값 'blob' 또는 'arraybuffer'; 기본값 'blob'.
        this.binaryType = "arraybuffer";

        // 연결 url
        this.url = null;

        // 마지막으로 성공?
        this.reconnectAttempts = 0;

        // 연결상태
        this.readyState = WebSocket.CONNECTING;

        // 프로트콜
        this.protocol = null;
    }

    wsOpen() {
        this.ws = new WebSocket(this.url, this.protocol || []);
        this.ws.binaryType = this.binaryType;

        if (this.debug) {
            console.debug("ReconnectingWebSocket", "attempt-connect", this.url);
        }

        const localWs = this.ws;

        this.timeout = setTimeout(() => {
            if (this.debug) {
                console.debug("ReconnectingWebSocket", "connection-timeout", this.url);
            }
            localWs.close();
        }, this.timeoutInterval);


        this.ws.onopen = event => {
            clearTimeout(this.timeout);
            if (this.debug) {
                console.debug("ReconnectingWebSocket", "onopen", this.url);
            }
            this.protocol = this.ws.protocol;
            this.readyState = WebSocket.OPEN;
            this.reconnectAttempts = 0;
        };

        this.wsClose();
        this.wsMessage();
        this.wsError();
    }

    wsClose() {
        this.ws.onclose = () => {
            clearTimeout(this.timeout);
            // 초기화 처리
            this.ws = null;
            self.readyState = WebSocket.CONNECTING;

            if (this.debug) {
                console.debug("ReconnectingWebSocket", "onclose", this.url);
            }

            const timeout = this.reconnectInterval *
                Math.pow(this.reconnectDecay, this.reconnectAttempts);

            setTimeout(() => {
                this.reconnectAttempts++;
                this.wsOpen(true);
            }, timeout > self.maxReconnectInterval ? self.maxReconnectInterval : timeout);
        };
    }

    wsMessage() {
        this.ws.onmessage = event => {
            if (this.debug) {
                console.debug("ReconnectingWebSocket", "onmessage", this.url, event.data);
            }

            // 리로드 패킷 들어올 때 처리
            if (event.data.type === "reload") {
                return;
            }

            self.postMessage(event.data);
            // console.log(event.data);
            // 데이터 처리 로직 추가
            // 로직 끝
        };
    }

    wsError() {
        this.ws.onerror = event => {
            if (this.debug) {
                console.debug("ReconnectingWebSocket", "onerror", this.url, event);
            }
        };
    }

    wsSend(data) {
        if (this.ws) {
            if (this.debug) {
                console.debug("ReconnectingWebSocket", "send", this.url, data);
            }
            return this.ws.send(data);
        } else {
            console.error("Pausing to reconnect websocket");
            return null;
        }
    }

    wsRefresh() {
        if (this.ws) {
            this.ws.close();
        }
    }

    post(path, payload, callback) {
        return this.service.request({
            method: "POST",
            url: path,
            responseType: "json",
            data: payload,
        }).then(response => callback(response.status, response.data));
    }

    conifg({url}) {
        this.url = url;
    }
}


export default SocketModule;

```

## PCMAxios.js

PCMAxios는 비동기 처리 라이브러리를 사용 했다. 현재 비동기 처리 LIB 중에 가장 많은 start를 받았고 쓰는데도 편해서 자주쓰는 api를 class로 만들었다.

```javascript
import axios from "axios";

class ServiceCall {
    constructor(props) {
        // 헤더정보
        this.initProperty();
        Object.keys(props).forEach(key => {
            this[key] = props[key];
        });
        const service = axios.create({
            baseURL: this.URL,
            HEADERS: this.HEADERS,
            // headers: {csrf: "token"},
        });

        service.interceptors.response.use(this.handleSuccess, this.handleError);
        this.service = service;
    }

    initProperty() {
        this.URL = "";
        this.HEADERS = "";
    }

    // response 전에 처리할 로직 집어 넣기
    handleSuccess = response => response
    // 에러코드 처리 vue에서는 라우터에서 할듯 싶음
    handleError = error => {
        switch (error.response.status) {
            case 401:
                this.redirectTo(document, "/");
                break;
            case 404:
                this.redirectTo(document, "/404");
                break;
            default:
                this.redirectTo(document, "/500");
                break;
        }
        return Promise.reject(error);
    }
    // 페이지 이동 처리 현재 사용하지 않음
    redirectTo = (document, path) => {
        // document.location = path;
    }

    get(path, callback) {
        return this.service.get(path).then(
            response => callback(response.status, response.data),
        );
    }

    patch(path, payload, callback) {
        return this.service.request({
            method: "PATCH",
            url: path,
            responseType: "json",
            data: payload,
        }).then(response => callback(response.status, response.data));
    }

    post(path, payload, callback) {
        return this.service.request({
            method: "POST",
            url: path,
            responseType: "json",
            data: payload,
        }).then(response => callback(response.status, response.data));
    }
}

export default ServiceCall;

```

## PCMServlet.list.js
PCMServlet.list는 비동기 api를 사용하다 보면 url이 자주 변경되어 레거시코드가 점점 증가하여 관리하기 힘들어 지는 상태가 온다. 그래서 따로 api 서블릿 type을 관리하여 레거시 코드를 쉽게 정리하고 관리 할 수 있도록 추가 했다.

```javascript
// API List
export const COMMMON = "common";
export const NODE_SUMMARY = "node_summary";
export const RUNNING_PODS_BY_DEPLOYMENT = "running_pods_by_deployment";
export const TOP_RESOURCE = "top_resource";
export const TOPOLOGY = "topology";
export const DATACENTER = "datacenter";


```

## SocketDataModule.js && ServletDataModule.js
SocketDataModule, ServletDataModule는 워커 스레드 영역에서 데이터 파싱 및 데이터 가공 처리를 위한 영역을 따로 나누어서 관리했다. 현재는 가공처리 로직이 들어 있지 않지만 추후에 발생되는 계산로직은 워커 스레드를 이용하기 위해 만들어 놨다.

```javascript
import * as apiTypes from "./PCMServlet.list";

const DataMoudule = {
        async _fnCall(caller, call) {
            await caller.get(call, (status, data) => {
                // console.log(data.type, "!!");
                switch (data.type) {
                    case apiTypes.COMMMON:
                        this.fn_common(status, data);
                        break;
                    case apiTypes.NODE_SUMMARY:
                        this.fn_node_summary(status, data);
                        break;
                    case apiTypes.RUNNING_PODS_BY_DEPLOYMENT:
                        this.fn_running_pods_by_deployment(status, data);
                        break;
                    case apiTypes.TOP_RESOURCE:
                        this.fn_top_resource(status, data);
                        break;
                    case apiTypes.TOPOLOGY:
                        this.fn_topology(status, data);
                        break;
                    case apiTypes.DATACENTER:
                        this.fn_data_center(status, data);
                        break;
                    default:
                        this.fn_noneTypeData(status, data);
                }
                // Vuex.$store.dispatch("store/topologyData", {data});
            });
        },

        // 순서가 보장되는 caller
        async apiCaller(caller, callList) {
            // const iterator = callList[Symbol.iterator]();
            // const _call = call => new Promise(res => {
            //     this._fnCall(caller, call);
            // });


            for (const call of callList) {
                // const call = iterator.next();
                // if (call.done) break;
                // for문안에 await 써도 되는지 나중에 확인 필요
                await this._fnCall(caller, call);
                // await this._fnCall(caller, call);
            }

            // for (; ;) {
            //     const call = iterator.next();
            //
            //     if (call.done) break;
            //     console.log(call.value);
            //     const bb = url => {
            //         this._fnCall(caller, url);
            //     };
            //
            //     bb(call.value).then(d => console.log("a"));
            //     // await this._fnCall(caller, call.value);
            // }
        },
// 순서 보장되지 않는 caller
        apiNoGuaranteeCaller(caller, callList) {
            for (const call of callList) {
                // const call = iterator.next();
                // if (call.done) break;
                this._fnCall(caller, call);
            }
        },
        fn_data_center(status, data) {
            self.postMessage(data);
        },
        fn_common(status, data) {
            // 아래 데이터 파싱 로직 추가
            self.postMessage(data);
        },
        fn_node_summary(status, data) {
            // 아래 데이터 파싱 로직 추가
            self.postMessage(data);
        },
        fn_running_pods_by_deployment(status, data) {
            // 아래 데이터 파싱 로직 추가
            self.postMessage(data);
        },
        fn_top_resource(status, data) {
            // 아래 데이터 파싱 로직 추가
            self.postMessage(data);
        },
        fn_topology(status, data) {
            // 아래 데이터 파싱 로직 추가
            self.postMessage(data);
        },
        fn_noneTypeData(status, data) {
            // 아래 데이터 파싱 로직 추가
            self.postMessage(data);
        }
        ,

    }
;


export default DataMoudule;

```

웹 프론트 데이타 관리 모듈은 웹 개발에 있어서 반드시 필요하고 새로 프로젝트가 시작되면 다시 사용 해야하는 경우가 많아 기본셋팅 껍데기를 만들어 놓으면 좋을거 같아서 따로 정리 한다.

## reference

