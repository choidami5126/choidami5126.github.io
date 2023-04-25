---
layout: single
title: "Styled-Components"
categories: React
tag: Styled-Components
toc: true
toc_label: "목차"
toc_sticky: true
---

# Styled-Components란?

React 애플리케이션에서 사용할 수 있는  
**<span style='color: #00ADB5'>CSS-in-JS 라이브러리</span>** 중 하나입니다.  
CSS-in-JS는 컴포넌트와 함께 CSS 스타일을 작성하는 방식으로,  
스타일을 더욱 모듈화하고  
**<span style='color: #00ADB5'>컴포넌트와 스타일을 하나의 파일에서 관리</span>**할 수 있며,  
className의 충돌을 피하고,  
재사용 가능한 스타일 컴포넌트를 만들 수 있습니다.

또한 컴포넌트 내부에서 CSS를 작성할 때,  
key : value 형식으로 작성하는 것이 어색하고  
자동완성 태그도 제시되지 않아  
약간 불편했지만 Styled-Components를 사용하여  
**<span style='color: #00ADB5'>본래 CSS 문법으로 작성이 가능합니다.</span>**

간단하게 요약하면, CSS-in-JS는  
CSS 코드를 JS 코드로 작성하고,  
이것으로 컴포넌트를 꾸민다로 정리할 수 있겠습니다.

또한 높고, 꾸준한 npm trends 점유율로  
신뢰성이 높은 패키지라고 생각 됩니다.

![npm_trend]({{site.url}}/images/npm_trend.png)

## Styled-Components 패키지 설치

- vscode-styled-components 확장 프로그램 설치.

- 터미널에서 아래 명령어로 설치 후  
   package.json 파일에 정상적으로 설치되었는지 확인하기.

```javascript
vscode add styled-components
```

- import styled

## Styled-Components 사용 예시

### 기본 사용 예시

```javascript
import "./App.css";
import styled from "styled-components";
// 패기지를 import 해야 사용 가능하다.

// 변수를 선언한다. styled 뒤에는
// 항상 html 요소가 온다.(커스텀할 컴포넌트를 입력함)
// 문자열을 사용하기 위해 백틱으로 열고 닫는다.
const Info = styled.div`
  width: 100px;
  height: 100px;
  border: 1px solid red;
  margin: 20px;
`;
// 완전한 css 방식으로 작성가능
// 태그로서 사용이 될 수 있는 컴포넌트가 됨

function App() {
  return (
    <div>
      <Info>박스</Info>
    </div>
  );
}
// CSS를 설정한 컴포넌트를 바로 배치 할 수 있다.
```

### 조건부 스타일링 예시

위 코드에서 Info는 App 컴포넌트의 자식 컴포넌트가 되므로  
**<span style='color: #00ADB5'>props를 내려받는 것으로 값을 동적으로 할당</span>** 하는  
아래 코드와 같은 **<span style='color: #00ADB5'>조건부 스타일링</span>**이 가능해진다.

```javascript
const StBox = styled.div`
  width: 100px;
  height: 100px;
  border: 1px solid ${(props) => props.borderColor};
  // props를 받아 borderColor를 동적으로 할당
  margin: 20px;
`;

function App() {
  return (
    <>
      <StBox borderColor="red">빨간박스</StBox>
      // props borderColor를 내려준다.
      <StBox borderColor="blue">파란박스</StBox>
      <StBox borderColor="green">초록박스</StBox>
    </>
  );
}
```

### JS 문법 사용 예시

Styled-Components 에서는  
**<span style='color: #00ADB5'>JS 문법을 사용가능</span>** 하므로,  
위 코드를 소프트 코딩으로 바꿔 보도록 하자

```javascript
const StBox = styled.div`
  width: 100px;
  height: 100px;
  border: 1px solid ${(props) => props.borderColor};
  margin: 20px;
`;

// 박스의 색을 담는 배열
const boxList = ["red", "blue", "green", "black", "yellow"];

// 인자로 받은 색상에 따른 return 값을 반환하는 함수
const getBoxName = (color) => {
  switch (color) {
    case "red":
      return "빨간 박스";
    case "blue":
      return "파란 박스";
    case "green":
      return "초록 박스";
    default:
      return "검정 박스";
  }
};

function App() {
  return (
    <div>
      {boxList.map((box) => (
        // 배열에 map 함수를 돌리며, 인자로 box를 받는다
        <StBox borderColor={box}>{getBoxName(box)}</StBox>
        // props로 box를 내려주며고
        // 인자로 받은 box에 따라 return값을 반환하는
        // 함수를 호출한다.
      ))}
    </div>
  );
}
```

**<span style='color: #FF0000'>단점으로는 초기 렌더링 속도가 느려질 수 있다는 점</span>**이 있지만  
단점 보다는 장점이 훨씬 많은 패키지로 보인다.  
숙달하면 숙달할 수록 사용 범위가 무궁무진해 보이니  
빠르게 익혀보자...
