---
title: 쿠키와 세션
date: 2024-11-25 00:00:00
categories: [Node.js, 쿠키와 세션]
tags:
  [
    Node.js,라이브러리
  ]
---

# 쿠키와 세션
---
1. 쿠키
브라우저가 서버로부터 응답으로 Set-Cookie 헤더를 받은 경우 해당 데이터를 저장한 뒤 모든 요청에 포함하여 보낸다.  
  - 쿠키는 사용자가 `naver.com`과 같은 웹 사이트를 방문할 때마다 이전에 방문했던 정보를 기억하는 데이터 파일이다.
  - 데이터를 여러 사이트에 공유할 수 있기 때문에 보안에 취약할 수 있다.
  - 쿠키는 `userId=user-1321;userName=sparta` 와 같이 문자열 형식으로 존재하며 쿠키 간에는 세미콜론`(;)` 으로 구분된다.

2. 세션
쿠키를 기반으로 구성된 기술. 단, 클라이언트가 마음대로 데이터를 확인 할 수 있던 쿠키와는 다르게 세션은 데이터를 서버에만 저장.  
  - 세션은 일반적으로 세션 Id를 쿠키를 이용해 클라이언트에게 전달하여, 서버는 이 세션 Id를 사용해 저장된 세션 데이터를 조회한다.
  - 세션을 통해 사용자의 상태 정보를 서버에 저장하면, 서버는 사용자의 상태를 추적할 수 있게 된다.
  - 보안성은 좋으나, 반대로 사용자가 많은 경우 서버에 저장해야 할 데이터가 많아져서 서버 컴퓨터가 감당하지 못하는 문제가 생기기 쉽다.
  - 쿠키와 마찬가지로 세션 역시 만료 기간이 있다.


#### 사용법
---

1. 쿠키 설정

    ```javascript
    // 'Set-Cookie'를 이용하여 쿠키를 할당하는 API
    app.get("/set-cookie", (req, res) => {
      let expire = new Date();
      expire.setMinutes(expire.getMinutes() + 60); // 만료 시간을 60분으로 설정합니다.

      res.writeHead(200, {
        'Set-Cookie': `name=sparta; Expires=${expire.toGMTString()}; HttpOnly; Path=/`,
      });
      return res.end();
    });
    ```

    ```javascript
    // 'res.cookie()'를 이용하여 쿠키를 할당하는 API
    app.get("/set-cookie", (req, res) => {
      let expires = new Date();
      expires.setMinutes(expires.getMinutes() + 60); // 만료 시간을 60분으로 설정합니다.

      res.cookie('name', 'sparta', {
        expires: expires
      });
      return res.end();
    });
    ```

2. 쿠키 조회

    ```javascript
    // 'req.headers.cookie'를 이용하여 클라이언트의 모든 쿠키를 조회하는 API
    app.get('/get-cookie', (req, res) => {
      const cookie = req.headers.cookie;
      console.log(cookie); // name=sparta
      return res.status(200).json({ cookie });
    });
    ```

#### cookie-parser(미들웨어)
---

1. 설치

    ```bash
    # yarn을 이용해 cookie-parser를 설치합니다.
    yarn add cookie-parser
    ```

2. cookie-parser(미들웨어)를 이용한 쿠키조회

    ```javascript
    import cookieParser from 'cookie-parser';

    app.use(cookieParser());

    // 'req.cookies'를 이용하여 클라이언트의 모든 쿠키를 조회하는 API
    app.get('/get-cookie', (req, res) => {
      const cookies = req.cookies;
      console.log(cookies);
      return res.status(200).json({ cookie: cookies });
    });
    ```