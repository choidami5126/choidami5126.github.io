---
layout: single
title: "json-server"
categories: React
tag: json-server
toc: true
toc_label: "목차"
toc_sticky: true
---

# json-server

아주 간단한 DB와 API를 생성해주는 패키지

FE <-> BE

테스트 환경을 제공함

fake server
mock data

```javascript
yarn add json-server
// 해당 코드로 설치
```

경로 파일 생성 db.json
json 형식의 내용을 작성하고

```javascript
yarn json-server --watch db.json --port nnnn
// --port nnnn은 생략 가능(포트를 지정하는 코드)
```

페이지 링크/"키"/"id:n"으로 데이터 표출이 가능하다.

http 통신 서버와 클라이언트의 통신 프로토콜

웹에서 서버 클라이언트간 주고 받은 상호간의 약속을 프로토콜이라 하며 http 프로토콜이라 한다.

Request(요청) 와 Response(응답)
클라이언트의 요청 서버의 응답
HTTP Request를 요청하고, HTTP Response를 받는다

url

HTTP 메서드
클라이언트가 서버에게 요청한다
GET
POST
PUT, PATCH
DELETE

상태코드

- **1xx(정보) :** 요청을 받았으며 프로세스를 계속 진행합니다.
- **2xx(성공) :** 요청을 성공적으로 받았으며 인식했고 수용하였습니다.
- **3xx(리다이렉션) :** 요청 완료를 위해 추가 작업 조치가 필요합니다.
- **4xx(클라이언트 오류) :** 요청의 문법이 잘못되었거나 요청을 처리할 수 없습니다.
- **5xx(서버 오류) :** 서버가 명백히 유효한 요청에 대한 충족을 실패했습니다.
