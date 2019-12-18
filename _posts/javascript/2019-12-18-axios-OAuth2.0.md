---
title: axios Token / refreshToken 처리
date: 2019-12-18
category: javascript
tags:
  - OAuth 2.0
  - Token / refreshToken
  - Axios
  - interceptors
---

# OAuth 2.0 / JWT 토큰 방식 axios에서 처리하기

프로젝트를 진행하다 보니 외부에 있는 restFul API 접근해서 데이터를 가져와서 사용해야하는 경우가 있다.

restful api를 통해서 데이터를 받아 올떄 상대 서버에서는 토큰 인증이라는 OAuth 2.0 방식을 사용 하고 있었다.

토큰이 어떤식으로 사용되고 있을까? 보게 되면

1. Email Id / PassWord를 가지고 토큰 발번 하는 API를 호출한다.
2. 정상처리가 되면 Token/TokenType/refreshToke등등 데이터를 받게 된다.
3. 앞으로 api 통해 데이터를 가져오기 위해서 header에 토큰을 담아서 api 호출해 데이터를 가져오게 된다.
4. 토큰은 한번 발급 받으면 사용할 수 있는 시간이 정해져있다.
5. 시간이 지나 유효한 토큰이 아닌 경우 더 이상 사용할 수 없게 된다.
6. 사용할수 없는 토큰으로 api를 통해 데이터를 요청하면 401 권한인증 실패라는 error 메시지를 전달 받게 된다.
7. 사용자는 권한인증 오류를 보지 않고 자연스럽게 refreshToken을 이용해 토큰 연장이 되게끔 처리한다.

> Token 만료 --> 재발급 로직을 사용자는 모르게 처리 하기!

아래 이미지는 axios를 통한 로직을 간단하게 그려 봤다. (front는 Vue framework사용)

<br>

![image](https://user-images.githubusercontent.com/25451713/70978928-02d76100-20f4-11ea-81c2-64033e0f46af.png)


<br><br>

****
<br>
토큰을 저장하기 위한 localStorage를 사용 했고 통신은 Axios 이용했다.





```javascript
import axios from "axios";

 const service = axios.create({
            baseURL: this.URL,
            timeout: 1000 * 60 * 5,
        });

    service.interceptors.response.use(this.handleSuccess, this.handleError);


```

Axios를 create 하고 interceptors를 사용한다.

interceptors는 실제로 api를 호출 했을 때 reslove / reject 둘중 하나를 먼저 앞에서 가로채서 처리 할 수 있는 유틸이다.

interceptors를 통해 실제 결과 리턴받기 전에 401 권한인증 오류를 받아서 처리를 한다.


this.handleSuccess 함수와 this.handleError 함수를 구현 한다.

<code>handleSuccess</code> 함수는 그대로 response를 리턴한다 <br>
<code>handleError</code> 함수는 error처리를 interceptor해서 앞에서 먼저 처리한다.

```javascript

const handleSuccess = response => response;
const handleError = error => {
        if (error.response.status === 401) {
            return Promise.resolve(this.refreshTokenCall(error));
        }
        return Promise.reject(error);
    };
```
<br>
위와 같이 status가 401인 경우는 refresh토큰을 통해 토큰 재발급 함수를 호출하여 resolve 함수를 호출하여 then을 탈 수 있도록 만들어준다. <br>
401이외 다른 상태 코드 오류인 경우는 reject를 호출하여 catch를 타게 만든다.

<code>refreshTokenCall</code> 보면

```javascript

refreshTokenCall(error) {
        const originalReq = error.response.config;

        if (this.refreshToken === "") return Promise.reject(error);
        const params = {
            refresh_token: this.refreshToken,
        };

        // 리프레쉬 토큰으로 Token 재발급
        this.setApiPath("/refreshToken");
        return this.post(params, (status, response) => {
            this.TokenType = response.token_type;
            this.AccessToken = response.access_token;
            this.refreshToken = response.refresh_token;
            return response;
        }).then(response => {
            originalReq.headers.Authorization = `${response.token_type} ${response.access_token}`;
            return this.reTryOriginalReq(originalReq);
        });
    }
```
<br>

originalReq는 401오류를 발생시킨 URL/param/비동기 환경정보들이 담겨 있다.<br>
refreshToken이 없는 경우는 reject 리턴하게 되고<br>
refreshToken이 있는 경우는 새로 토큰을 발번 받고 토큰을 셋팅하고 originalReq정보를 통해서 다시 호출하여 통신을 끊김없이 진행할수 있다.

<br>

이뿐만 아니라 이외 다른 error처리를 앞단에서 미리 interceptors하여 처리 할 수 있으니 많이 활용하자!

## reference

