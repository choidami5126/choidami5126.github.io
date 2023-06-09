---
layout: single
title: "RESTful API"
categories: CS #WIL | TIL | CS
toc: true
toc_label: "목차"
toc_sticky: true
author_profile: false
sidebar:
    nav : "counts"
search : true
---

# RESTful API란?
Representational State Transfer(표현 상태 전이)를 따르는   
웹 서비스의 `설계 원칙`입니다.   
RESTful API는 자원(Resource)을 URL로 표현하고,   
`HTTP 메소드`를 통해 해당 자원에 대한 조작을 수행합니다.   

## RESTful API
- **GET**   
서버에서 자원의 상태를 요청하는데 사용됩니다.   
주어진 URL에서 데이터를 검색하고 가져오는 역할을 합니다.

- **POST**   
서버에 새로운 자원을 생성하거나 기존 자원을 변경하는데 사용됩니다.      
주로 데이터를 서버로 제출하여 처리하는 데 사용됩니다.  

- **PATCH**   
서버에서 지정된 자원의 부분적인 변경을 수행하는데 사용됩니다.   
주어진 URL에서 특정 자원의 일부만 수정합니다.

- **PUT**   
서버에 새로운 자원을 생성하거나 기존 자원을 업데이트하는데 사용됩니다.   
주어진 URL에서 지정된 자원을 요청된 페이로드로 대체합니다.

- **DELETE**   
서버에서 지정된 자원을 삭제하는데 사용됩니다.   
주어진 URL에서 특정 자원을 제거합니다.

- **HEAD**   
GET 메소드와 유사하지만, 서버는 응답에 본문을 포함시키지 않고   
헤더 정보만을 반환합니다. 이를 통해 리소스의 메타데이터를 요청할 수 있습니다.

- **OPTIONS**   
서버에서 지정된 자원에 대한 지원되는 메소드, 헤더, 인증 등의   
정보를 요청하는데 사용됩니다.      
이를 통해 클라이언트는 서버에서 지원되는 작업을 파악할 수 있습니다.

## RESTful API가 아닌 API 디자인 패턴과 메소드
- **SOAP (Simple Object Access Protocol)**   
XML을 기반으로 하는 웹 서비스 통신 프로토콜입니다.   
SOAP은 명확한 프로토콜 및 데이터 정의를 갖추고 있으며, 복잡한 기능과 메시지, 오류 처리 등을 상세히 정의합니다.

- **RPC (Remote Procedure Call)**   
원격 프로시저 호출을 수행하는 방식으로, 클라이언트가 서버에서 제공되는 메소드를 호출하여 원격으로 작업을 수행합니다.   
RPC는 클라이언트-서버 간의 통신을 위한 프로토콜이나 메커니즘을 제공하며, 주로 메소드 이름과 매개변수를 전송하여 서버에서 처리하고 결과를 반환받습니다.

- **GraphQL**   
클라이언트가 원하는 데이터의 구조와 필드를 지정하여 서버로부터 필요한 데이터를 요청하는 쿼리 언어입니다.   
RESTful API보다 유연하고 효율적인 데이터 요청을 가능하게 합니다.

- **JSON-RPC**   
JSON을 기반으로 하는 원격 프로시저 호출 프로토콜로, RPC와 유사하지만 JSON 형식을 사용하여 데이터를 주고받습니다.

- **gRPC (Google Remote Procedure Call)**    
구글에서 개발한 원격 프로시저 호출 프레임워크로, 다양한 언어와 플랫폼 간의 효율적인 통신을 지원합니다.