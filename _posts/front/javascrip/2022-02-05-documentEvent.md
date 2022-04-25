---
layout: single
title: "event"
# categories: Git
categories:
  - JavaScript # HTML CSS JavaScript Server Algorithm Wecodes Programmers CS Github Blog
tag: [event, document, dom] #tag는 여러개 가능함
toc: true #table of content 기능!
toc_sticky: true
author_profile: true #blog 글안에서는 author_profile이 따라다니지 않도록 설정함
# sidebar:
# nav: "docs" #네비게이션에 있는 docs를 의미함
---

# event란?

수많은 웹페이지들 중 쇼핑몰에서 사진 위에 마우스를 올리면 다른 각도의 제품 사진으로 바꿔서 보여주는 것들을  
우리는 JavaScript를 이용하여 코딩한다.  
이렇게 특정 요소에 <u>interactive한 반응</u>을 할 수 있게 하는 것을 `event`라고 한다.

- 클릭 이벤트
- 마우스 이벤트
- 스크롤 이벤트
- 터치 이벤트
- resize(화면 크기 변화) 이벤트

## addEventListener

이벤트를 달 때 사용하는 함수는 <span style="color:red">addEventListener</span>이다.  
addEventListener는 특정 이벤트가 언제 발생하는지 듣고 있다가, 발생하면 인자로 받은 함수를 실행한다.

```java
요소.addEventListener(이벤트종류, function() {
  //이벤트가 일어났을 때 실행될 내용
})
```

- <u>특정한 요소에 addEventListener 함수를 붙이고</u>, `argument`로 <u>이벤트 종류</u>와,  
  이벤트가 발생했을 때 `실행할 함수를 전달`한다.
- <u>두 번째 argument로 들어오는 함수</u>에서 항상 `event와 관련된 정보를 argument로 받을 수` 있다.

## 이벤트 종류

cllick, contextmenu, dblclick, mouseenter, mouseleave, mousemove, mouseout,  
mouseup, pointerlockchange, pointerlockerror, select, wheel

## 클릭 이벤트

버튼, 사진, 글 등 웹사이트에서 이루어지는 이벤트 중 가장 많은 것은 클릭 이벤트이다.

- 로그인 버튼 클릭 -> 로그인 API 호출
- 상품 사진 클릭 -> 상품 상세 화면으로 이동
- 자세히 보기 버튼 클릭 -> 팝업화면 출력

## 키이벤트

키 이벤트는 사람이 키보드를 누르면 발생되는 이벤트이다.

- 키보드를 눌렀을 때 발생하는 keydown
- 키보드를 누르고 떼는 순간 발생하는 keyup
- 키보드를 눌러 어떤 텍스트가 작성되는 순간 발생하는 keypress

## 실습

html

```html
<body>
  <div class="login-container">
    <input type="password" id="password" placeholder="비밀번호" />
    <input type="password" id="re-password" placeholder="비밀번호 확인" />
    <button class="login-btn">로그인</button>
    <p class="alert"></p>
    <p>key code: <span id="code"></span></p>
  </div>

  <script src="index.js"></script>
</body>
```

### 클릭 이벤트

```java
const thisIsButton = document.getElementsByClassName('login-btn')[0];

thisIsButton.addEventListener('click', function() {
  const password = document.getElementById('password').value;
  const rePassword = document.getElementById('re-password').value;

  if (!password) {
    alert('비밀번호를 입력해주세요!');
    return;
  }

  if (!rePassword) {
    alert('비밀번호 확인을 입력해주세요!');
    return;
  }

  if (password !== rePassword) {
    alert('비밀번호가 맞지 않습니다!');
    return;
  }

  alert('회원가입 성공!!');
});
```

const thisIsButton = document.getElementsByClassName('login-btn')[0];

- getElementsByClassName 함수로 'login-btn'라는 클래스 이름이 있는 요소를 찾는다.  
  <u>class의 이름에는 여러 요소들에 중복해서 이름을 부여</u>할 수 있는데  
  이때 'login-btn'라는 class 이름을 가진 요소들이 모두 배열로 반환되므로 `[0] 이렇게 index를 붙여`준다.

### 키 이벤트

```java
const thisIsPw = document.getElementById('password');
const thisIsCode = document.getElementById('code');

thisIsPw.addEventListener('keydown', function(event) {
  thisIsCode.innerHTML = event.code;
});

thisIsPw.addEventListener('keydown', function(e) {
  if (e.code === 'Enter') {
     //로그인 함수로 이동
  }
});
```

const thisIsPw = document.<span style="color:green">getElementById('password')</span>;

- <u>id는 html 작성시 전체 페이지에서 중복될 수 없는 유일한 것</u>이기 때문에, array로 반환되지 않고  
   그 요소가 return된다

thisIsPw.addEventListener('keydown', function(e){}

- input에 키보드로 뭔가를 누르면 두 번째 argument인 function이 실행되고 ->  
  span #code의 내용에 e.code가 들어간다.(여기서 code는 입력된 키의 이름 정보를 의미함)

### Assignment

이벤트가 발생하면 실행될 함수에 아래의 기능을 만들어주세요.  
input#re-password에 keyup 이벤트를 추가해주세요.  
input#password 와 input#re-password의 value 속성을 통해 input에 작성된 값을 가져오고,  
서로 같은지 아닌지 확인해서 같지 않다면  
p태그의 .alert 클래스에 "비밀번호가 일치하지 않습니다" 라는 문구를 넣어주세요.

나의 코드

```java
const thisIsPw = document.getElementById('password');
const thisIsButton = document.getElementsByClassName('login-btn')[0];
const thisIsRePw = document.getElementById('re-password');
const pAlert = document.getElementsByClassName('alert')[0];

thisIsRePw.addEventListener('keyup', function() {
  const password = thisIsPw.value;
  const rePassword = thisIsRePw.value;

  if(password !== rePassword) {
    pAlert.innerHTML = '비밀번호가 일치하지 않습니다';
  } else if(password == rePassword) {
    pAlert.innerHTML = '';
  }

})
```

<img src="https://user-images.githubusercontent.com/87808288/152669639-3a1ef2a7-6320-401d-b4d3-3628a98ccc8c.png" width="200" height="200">

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