---
layout: single
title: "React Router-DOM"
categories: React
tag: Router-DOM
toc: true
toc_label: "목차"
toc_sticky: true
---

yarn add react-router-dom 명령어로 패키지 설치

Router.js 파일 생성. router를 구성하는 설정 코드 작성

App.jsx에 import

src폴더 하위로 pages, shared 폴더 생성

shared 폴더 하위로 Router.js 파일 생성

Router.js 파일에 import BrowserRouter, Route, Routes

아래 코드와 같은 변수 생성 : 기본 폼 그냥 외우기

```javascript
import { BrowserRouter, Route, Routes } from "react-router-dom";

const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route />
        <Route />
        <Route />
        <Route />
      </Routes>
    </BrowserRouter>
  );
};
```

설정할 페이지들 셋팅 해주기

```javascript
import { BrowserRouter, Route, Routes } from "react-router-dom";
import Home from "../pages/Home";
import Contact from "../pages/Contact";
import About from "../pages/About";
import Works from "../pages/Works";

const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="about" element={<About />} />
        <Route path="contact" element={<Contact />} />
        <Route path="works" element={<Works />} />
      </Routes>
    </BrowserRouter>
  );
};

export default Router;
```

App.jsx에서 Router 불러올 때, React-Router-DOM 제공이 아닌 직접 만든 파일을 불러오기

새로 고침 되는 것이 아님

전용 Hook
useNavigate 어떤 버튼을 눌렀을 때 페이지 이동을 할 수 있게 함(a태그 href 속성과 유사)

```javascript
import React from "react";
import { useNavigate } from "react-router-dom";

function Home() {
  const navigate = useNavigate();

  return (
    <div>
      Home
      <br />
      <button
        onClick={() => {
          navigate("/works");
        }}
      >
        works로 이동
      </button>
    </div>
  );
}

export default Home;
```

```javascript
import React from "react";
import { useNavigate } from "react-router-dom";

function Works() {
  const navigate = useNavigate();

  return (
    <div>
      Works
      <br />
      <button
        onClick={() => {
          navigate("/");
        }}
      >
        home으로 이동
      </button>
    </div>
  );
}

export default Works;
```

위 예시 코드 처럼 useNavigate 훅을 사용해 간단히 페이지 이동을 구현할 수 있다.

useLocation
location 객체에 포함된 정보들로 조건부 렌더링 등을 할 수 있다.

Link API
HTML a태그를 대체하는 API

a태그를 사용하면 페이지가 이동하면서 브라우저가 새로고침 되고
모든 컴포넌트가 다시 렌더링 되어야함
또한, Redux, useState 등을 통해 메모리상에 구축한 모든 상태값이 초기화됨

```javascript
import React from "react";
import { Link, useLocation, useNavigate } from "react-router-dom";

function Works() {
  const navigate = useNavigate();
  const location = useLocation();

  // console.log('location', location)
  return (
    <div>
      Works
      <br />
      <button
        onClick={() => {
          navigate("/");
        }}
      >
        home으로 이동
      </button>
      <br />
      <Link to={"/contact"}>contact 페이지로 이동하기</Link>
    </div>
  );
}

export default Works;
```

useParams

동적 Routing : Dynamic Route
