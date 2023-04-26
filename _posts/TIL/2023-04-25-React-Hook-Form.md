---
layout: single
title: "React Hook Form을 사용해 Form 최적화 하기"
categories: TIL #WIL | TIL
toc: true
toc_label: "목차"
toc_sticky: true
author_profile: false
sidebar:
    nav : "counts"
search : true
---

```bash
yarn add react-hook-form
```

가장 많이 사용하는 것은 `mode`와 `defaultValues`

mode 옵션은 `validation` 전략을 설정하는 데 활용합니다.   
onSubmit, onChange, onBlur, all 등의 옵션이 있습니다.   
주의해야 할 점은 mode 를 onChange 에 놨을 때   
**다수의 리렌더링이 발생할 수 있어 성능에 영향을 끼칠 수 있다고 합니다.**

defaultValues 옵션은` form 에 기본 값`을 제공하는 옵션입니다.  
주의해야 할 점은 react-hook-form 을 사용할 때  
기본값을 제공하지 않는 경우 input 의 `초기값은 undefined` 로 관리가 됩니다.  