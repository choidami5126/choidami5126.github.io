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

---기존 컴포넌트---
```typescript
const AdminLogin = () => {
  const { adminLoginInfo, submitLoginHandler, changeInputHandler } = useAdminLogin();
  const navigate = useNavigate();

  const Waiting = () => {
    Swal.fire({
      icon: 'info',
      title: '준비 중인 기능입니다.',
    });
  };

  return (
    <Tap onSubmit={submitLoginHandler}>
      <MaxInput
        types="login"
        type="text"
        name="companyId"
        value={adminLoginInfo.companyId}
        onChange={changeInputHandler}
        placeholder="대표자 아이디를 입력해주세요"
      />
      <MaxInput
        types="login"
        type="password"
        name="password"
        value={adminLoginInfo.password}
        onChange={changeInputHandler}
        placeholder="비밀번호를 입력해주세요."
      />
      <TextWrapper>
        <span onClick={() => navigate('/masterSignup')} style={{ fontSize: '12px' }}>
          미어캣린더가 처음이면! <strong style={{ fontWeight: 'bold' }}>회원가입</strong>
        </span>
        <span onClick={Waiting} style={{ fontSize: '12px' }}>
          아이디 / 비밀번호 찾기
        </span>
      </TextWrapper>
      <Button
        size="login"
        style={{
          color: '#E64042',
          fontSize: '15px',
          fontWeight: 'bold',
          borderRadius: '7px',
          backgroundColor: '#F6F6F6',
        }}
      >
        로그인
      </Button>
    </Tap>
  );
};
```
---리팩토링 후---
```typescript
import { useForm } from 'react-hook-form';
import Swal from 'sweetalert2';
import { useNavigate } from 'react-router-dom';
// 👆 라이브러리
import CustomButton from '../../../components/Atoms/Button/CustomButton';
import CustomInput from '../../../components/Atoms/Input/CustomInput';
// 👆 Atom-component
import { useAdminLogin } from '../hooks/useAdminLogin';
import { TextWrapper, SubmitForm, StSpan } from '../styles';

export type AdminLoginInfo = {
  companyId: string;
  password: string;
};

const AdminLoginForm = () => {
  // react-hook-form의 객체를 생성
  const { register, handleSubmit, reset } = useForm<AdminLoginInfo>();
  // hook에 제출 함수를 가져옴
  const { AdminLoginHandler } = useAdminLogin(reset);
  const navigate = useNavigate();

  //아이디/비밀번호 찾기 클릭 시 표출
  const Waiting = () => {
    Swal.fire({
      icon: 'info',
      title: '준비 중인 기능입니다.',
    });
  };

  return (
    <SubmitForm onSubmit={handleSubmit(AdminLoginHandler)}>
      <CustomInput
        inputType="login"
        placeholder="대표자 아이디를 입력해주세요"
        {...register('companyId', {
          required: true,
        })}
      />
      <CustomInput
        inputType="login"
        type="password"
        placeholder="비밀번호를 입력해주세요"
        {...register('password', {
          required: true,
        })}
      />
      <TextWrapper>
        <StSpan>
          미어캣린더가 처음이라면!&nbsp;
          <StSpan
            onClick={() => navigate('/masterSignup')}
            style={{ fontWeight: 'bold' }}
          >
            회원가입
          </StSpan>
        </StSpan>
        <StSpan onClick={Waiting}>아이디 / 비밀번호 찾기</StSpan>
      </TextWrapper>
      <CustomButton buttonType="login">로그인</CustomButton>
    </SubmitForm>
  );
};

export default AdminLoginForm;
```

---기존 훅---
```typescript
import { useMutation } from '@tanstack/react-query';
import { useNavigate } from 'react-router-dom';
import { AdminLoginInfo } from '../../MasterSignup/interfaces';
import apis from '../../../api/axios/api';
import { setCookie } from '../../../api/auth/CookieUtils';
import React from 'react';

export interface AdminLoginResponse {
  token: string;
  message: string;
}

export const useAdminLogin = () => {
  const navigate = useNavigate();

  const [adminLoginInfo, setAdminLoginInfo] = React.useState<AdminLoginInfo>({
    companyId: '',
    password: '',
  });

  const changeInputHandler = (e: React.ChangeEvent<HTMLInputElement>): void => {
    const { name, value } = e.target;
    setAdminLoginInfo({ ...adminLoginInfo, [name]: value });
  };

  const login = useMutation<AdminLoginResponse, Error, AdminLoginInfo>({
    mutationFn: async (item: AdminLoginInfo) => {
      const data = await apis.post<AdminLoginResponse>('/adminlogin', item);
      return data.data;
    },
    onSuccess: data => {
      const token = data.token;
      setCookie('token', token);
      navigate('/main');
    },
    onError() {
      alert('아이디 혹은 비밀번호가 일치하지 않습니다.');
      setAdminLoginInfo({ companyId: '', password: '' });
    },
  });
  const submitLoginHandler = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    login.mutate(adminLoginInfo);
  };
  return { adminLoginInfo, submitLoginHandler, changeInputHandler };
};
```

---리팩토링 후---
```typescript
import { useMutation } from '@tanstack/react-query';
import { setCookie } from '../../../api/auth/CookieUtils';
import { useNavigate } from 'react-router-dom';
import { AdminLoginInfo } from '../components/AdminLoginForm';
import apis from '../../../api/axios/api';

export type LoginResponse = {
  token: string;
  message: string;
};

export const useAdminLogin = (reset: () => void) => {
  const navigate = useNavigate();

  const login = useMutation<LoginResponse, Error, AdminLoginInfo>({
    mutationFn: async (item: AdminLoginInfo) => {
      const data = await apis.post<LoginResponse>('/auth/admin', item);
      return data.data;
    },
    onSuccess: data => {
      const token = data.token;
      setCookie('token', token);
      navigate('/main');
    },
    onError() {
      alert('아이디 혹은 비밀번호가 일치하지 않습니다.');
      reset();
    },
  });
  const AdminLoginHandler = (data: AdminLoginInfo) => {
    login.mutate(data);
  };

  return { AdminLoginHandler };
};
```

---리팩토링 후 유저로그인 훅---
```typescript
import { useMutation } from '@tanstack/react-query';
import { setCookie } from '../../../api/auth/CookieUtils';
import { useNavigate } from 'react-router-dom';
import { UserLoginInfo } from '../components/UserLoginForm';
import { LoginResponse } from './useAdminLogin';
import apis from '../../../api/axios/api';

export const useLogin = (reset: () => void) => {
  const navigate = useNavigate();

  const login = useMutation<LoginResponse, Error, UserLoginInfo>({
    mutationFn: async (item: UserLoginInfo) => {
      const data = await apis.post<LoginResponse>('/auth/user', item);
      return data.data;
    },
    onSuccess: data => {
      const token = data.token;
      setCookie('token', token);
      navigate('/main');
    },
    onError() {
      alert('아이디 혹은 비밀번호가 일치하지 않습니다.');
      reset();
    },
  });

  const userLoginHandler = (data: UserLoginInfo) => {
    login.mutate(data);
  };

  return { userLoginHandler };
};
```

---리팩토링 후 어드민로그인 훅---
```typescript
import { useMutation } from '@tanstack/react-query';
import { setCookie } from '../../../api/auth/CookieUtils';
import { useNavigate } from 'react-router-dom';
import { AdminLoginInfo } from '../components/AdminLoginForm';
import apis from '../../../api/axios/api';

export type LoginResponse = {
  token: string;
  message: string;
};

export const useAdminLogin = (reset: () => void) => {
  const navigate = useNavigate();

  const login = useMutation<LoginResponse, Error, AdminLoginInfo>({
    mutationFn: async (item: AdminLoginInfo) => {
      const data = await apis.post<LoginResponse>('/auth/admin', item);
      return data.data;
    },
    onSuccess: data => {
      const token = data.token;
      setCookie('token', token);
      navigate('/main');
    },
    onError() {
      alert('아이디 혹은 비밀번호가 일치하지 않습니다.');
      reset();
    },
  });
  const AdminLoginHandler = (data: AdminLoginInfo) => {
    login.mutate(data);
  };

  return { AdminLoginHandler };
};
```

---훅 통합---
```typescript
const
```