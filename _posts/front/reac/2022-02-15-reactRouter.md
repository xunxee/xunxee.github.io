---
layout: single
title: "React-Router"
# categories: Git
categories:
  - React # HTML CSS JavaScript Server Algorithm Wecodes Programmers CS Github Blog
tag: [router] #tag는 여러개 가능함
toc: true #table of content 기능!
toc_sticky: true
author_profile: true #blog 글안에서는 author_profile이 따라다니지 않도록 설정함
# sidebar:
# nav: "docs" #네비게이션에 있는 docs를 의미함
---

# SPA(Single Page Application)

SPA는 `페이지가 1개`인 애플리케이션을 말한다.  
html로 작업했던 Westagram에서는 main.html과 login.html 페이지로 구성되어  
<u>페이지 수만큼 html 파일이 존재</u>했지만, `React에서는 1개만 존재`한다.  
이렇게 <u>1개의 웹페이지(HTML) 안에서 여러 개의 페이지를</u> 보여주는 방법을 <span style="color:red">'Routing'</span>이라고 한다.  
<span style="color:red">React Router</span>란, React `컴포넌트로 페이지를 나누는 라이브러리`를 말한다.

# Routing

<span style="color:red">Routing</span> 이란 `다른 경로(url 주소)에 따라 다른 View`를 보여주는 것을 말한다.  
React는 라이브러리로 이러한 기능이 내장되어 있지 않으므로 -> Router를 이용하는 것이다!

React의 핵심 컴포넌트에는 <u>Router, Routes, Route, Link</u> 이렇게 4가지가 있다.

**Router**  
리액트 router에서 사용하는 데이터들을 모두 가지고 있는 부분이다.  
현재 주소나 페이지 기록 등의 데이터를 가지고 있다.  
이게 없다면 react router를 사용할 수 없게 된다.

router 컴포넌트도 내부적으로는 context 프로바이드이다.  
그래서 react router에서 제공하는 기능을 사용하려면,  
반드시 router 컴포넌트 안에서 사용해야한다.

- Routes, Route

**react router 설치하기**  
패키지를 설치하기 위해서는 <u>package.json이 있는 폴더</u>에서 `terminal`을 실행하고 ->  
(package.json은 우리 프로젝트의 요약본!)

```bash
npm install react-router-dom --save
```

위의 명령어에서 react-router-dom은 패키지의 이름이고,  
@6은 -> 버전을 6점대로 설치하겠다는 의미를 갖는다!  
`잘 설치되었는지 확인하는 방법`은 <u>package.json 파일</u>에서 'dependencies' 아래에 ->  
react-router-dom 이란 것이 추가되어있는지 확인해본다.

# React Router

**Router 컴포넌트 구현하기**

```java
import React from "react";
import { BrowserRouter, Routes, Route } from "react-router-dom";

import Nav from "./components/Nav/Nav";
import Footer from "./components/Footer/Footer";
import Login from "./pages/Login/Login";
import Signup from "./pages/Signup/Signup";
import Main from "./pages/Main/Main";

function Router() {
  return (
    <BrowserRouter>
      <Nav />
      <Routes>
        <Route path="/" element={<Login />} />
        <Route path="/signup" element={<Signup />} />
        <Route path="/main" element={<Main />} />
      </Routes>
      <Footer />
    </BrowserRouter>
  );
}

export default Router;
```

<u>path는 경로를 입력받는 곳</u>이다.  
예를 들어 사이트의 'gunhee.github.io/main'  
이런식으로 사이트 주소의 페이지 경로라고 이해하 수 있다.

**index.js**  
CRA로 만든 앱에 routing 기능을 적용하려면 <u>index.js를 수정</u>해야 한다.  
`<App /> 컴포넌트` 대신에 routing을 설정한 컴포넌트<Router />로 변경해야 한다.

```java
ReactDOM.render(<Router />, document.getElementById('root'));
```

**Route 이동하기**  
Route 이동하는 방법에는 2가지가 존재한다.  
`<Link> 컴포넌트`와 `useNavigate로 구현`하는 방법!

왜 방법을 구분해놓았을까?  
&nbsp; Link는 단순 컴포넌트이므로 무조건 이동시킨다.  
&nbsp; useNavigate는 <u>자바스크립트 코드</u>이므로 -> 조건문! `조건에 따라 이동`시킬 수 있다.

- `<Link> 컴포넌트`  
  a 태그는 외부사이트로!, Link태그는 우리가 구현한 프로젝트 내에서 페이지를 전환하는 경우!

```java
import React from "react";
import { Link } from "react-router-dom";

function Login() {
  return (
    <div>
      <Link to="/signup">회원가입</Link>
    </div>
  );
}

export default Login;
```

Router.js에서 설정한 path로 이동하는 첫 번째 방법은 `<Link/> 컴포넌트`를 사용하는 방법이다.  
react-router-dom 에서 제공하는 `<Link />` 컴포넌트는 DOM에서 `<a>`로 변환(compile) 된다.

- useNavigate 로 구현하는 방법

```java
import React from "react";
import { useNavigate } from "react-router-dom";

function Login() {
  const navigate = useNavigate();

  const goToMain = () => {
    navigate("/main");
  };

  // 실제 활용 예시
  // const goToMain = () => {
  //   if(response.message === "valid user"){
  //     navigate('/main');
  //   } else {
  //     alert("가입된 회원이 아닙니다. 회원가입을 먼저 해주세요.")
  //     navigate('/signup');
  //   }
  // }

  return (
    <div>
      <button className="loginBtn" onClick={goToMain}>
        로그인
      </button>
    </div>
  );
}

export default Login;
```

`<Link />`를 사용하지 않고 함수 호출을 통해 페이지를 이동하는 방법도 있다.  
<u>goToMain 함수</u>에 구현된 것처럼, <u>useNavigate 훅</u>을 통해 페이지를 이동할 수 있다.  
&nbsp; useNavigate 훅을 실행하면 페이지 이동을 할 수 있게 해주는 함수를 반환한다.  
&nbsp; -> 해당 함수를 navigate라는 변수에 저장한다.

# router 컴포넌트 감싸기

이제 프로젝트의 최상위 컴포넌트인 `Main.js` 파일에서 router 컴포넌트를 적용해야한다.  
`BrowserRouter` 라는 컴포넌트를 불러와서 <u>컴포넌트 전체를 감싸주게</u>된다.  
`<a> 태그`와 마찬가지로 `<Link/> 컴포넌트`도 <u>지정한 경로로 바로 이동시켜주는 기능</u>을 한다.  
&nbsp; a 태그는 -> 외부 사이트로 이동하는 경우, Link 태그는 -> 프로젝트 내에서 페이지를 전환하는 경우!

```java
import { BrowserRouter } from 'react-router-dom';
import App from './components/App';
import HomePage from './pages/HomePage';

function Main() {
  return (
    <BrowserRouter>
      <App>
        <HomePage />
      </App>
    </BrowserRouter>
  );
}

export default Main;
```

<img src="https://user-images.githubusercontent.com/87808288/153963626-f352a8b7-ecfc-4bcd-acc5-9cd347c4d1ec.png" width="500" height="300">  
우선 Routes 컴포넌트로 Route들을 감싸고, 각 페이지마다 Route들을 배치하게 된다.  
각 페이지의 경로는 path props로, 컴포넌트는 element props로 지정한다.  
그리고 이때 element props에는 꼭 <span style="color:red">JSX로 설정</span>하여야 한다!

요약  
index.js에서 react DOM을 통해서

<!-- ### 2. Link 넣기

```

유형 1: (설명어를 입력) : [gunhee's coding blog](https://gunhee-jeong.github.io/)
유형 2: (URL 자동연결) : <https://gunhee-jeong.github.io/>
유형 3: (동일 파일 내 '문단으로 이동') : [1. Header로 이동](###-1-header)

```

유형 1: (설명어를 입력) : [gunhee's coding blog](https://gunhee-jeong.github.io/)
유형 2: (URL 자동연결) : <https://gunhee-jeong.github.io/>
유형 3: (동일 파일 내 '문단으로 이동') : [1. Header로 이동](#1-header)
유형 3의 방법

1. 특수문자를 제거
2. 스페이스는 -로 바꾸고
3. 대문자는 소문자로!
   그래서 ### 1. Header -> #1-header

## Link: [google][https://www.google.com/]

### 3. 수평선

```

---

```

---

### 4. 라인 바꾸기

```

스페이스바를 2번 눌러주면 다음칸으로
이동할 수 있어요!

```

---

스페이스바를 2번 눌러주면
다음칸으로 이동할 수 있어요!

### 5. list 만들기

```

1. 1번
2. 2번
3. 3번

- 순서없는 list
  - 순서없는 list
    - 순서없는 list

```

1. 1번
2. 2번
3. 3번

- 순서없는 list
  - 순서없는 list
    - 순서없는 list

---

### 6. font 관련

```

**진하게** -> 볼드
_기울여서_ -> 이탤릭체
~~취소선~~ -> 취소선

<ul>밑줄넣기</ul> -> 밑줄
<span style="color:red">빨간 글씨</span> -> 글자색
이것이 `인라인` 입니다 -> 인라인 코드
```

**진하게** -> 볼드
_기울여서_ -> 이탤릭체
~~취소선~~ -> 취소선
<u>밑줄넣기</u> -> 밑줄
<span style="color:red">빨간 글씨</span>
이것이 `인라인` 입니다 -> 인라인 코드

---

### 7. 인용구문

```
> coding
>
> > JavaScript
> >
> > > 내가 프짱!
```

> coding
>
> > JavaScript
> >
> > > 내가 프짱!

---

### 8. 이미지 삽입

```
유형1: ('사이즈를 조절' -> HTML 태그 사용) : <img src="https://gunhee-jeong.github.io/assets/images/blogLogo.png" width="300" height="200">
유형2: (이미지 삽입 후 -> 링크 걸기)
[![이미지](https://gunhee-jeong.github.io/assets/images/blogLogo/blogLogo.png)](https://gunhee-jeong.github.io/)
```

유형1: ('사이즈를 조절' -> HTML 태그 사용) : <img src="https://gunhee-jeong.github.io/assets/images/blogLogo.png" width="300" height="200">
유형2: (이미지 삽입 후 -> 링크 걸기)
[![이미지](https://gunhee-jeong.github.io/assets/images/blogLogo.png)](https://gunhee-jeong.github.io/)

### 9. 표 만들기

```
||국어|영어|
| :--- | ---: | :--: |
|건희 | 100점 | 100점
|철수 | 100점 | 100점
```

|      |  국어 | 영어  |
| :--- | ----: | :---: |
| 건희 | 100점 | 100점 |
| 철수 | 100점 | 100점 |

> - header를 넣고 싶은 경우 ---을 사용하고 :을 이용하여 정렬에 사용함!

### 10. 토글 만들기

```
<details>
<summary>여기를 누르세요</summary>
<div markdown="1">
숨겨진 내용
</div>
</details>
```

<details>
<summary>여기를 누르세요</summary>
<div markdown="1">
숨겨진 내용
</div>
</details> -->