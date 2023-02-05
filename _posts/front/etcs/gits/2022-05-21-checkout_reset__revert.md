---
layout: single
title: "checkout & reset & revert"
# categories: Git
categories:
  - Github # HTML CSS JavaScript Server Algorithm Wecode Programmers CS Github Blog
tag: [생활코딩] #tag는 여러개 가능함
toc: true #table of content 기능!
toc_sticky: true
author_profile: true #blog 글안에서는 author_profile이 따라다니지 않도록 설정함  
date: 2022-05-21T16:00:00+09:00
# sidebar:
# nav: "docs" #네비게이션에 있는 docs를 의미함
---
# 1장 checkout
`checkout`은 <span style="color:red">HEAD를 직접적으로</span> 바꾸게 된다.  
checkout은 <span style="color:blue">master branch가 가리키는 commit은 바꾸지 않았고</span>, HEAD가 가리키는 것만 바꾸었다.  
<!-- 그렇기 때문에 우리의 <span style="color:tomato">working directory</span>는 <span style="color:blue">B commit을 기반</span>으로 하게 된다.   -->
<img src="https://user-images.githubusercontent.com/87808288/179531585-1524d76b-b8e5-46b0-b4e2-0feb3448a555.png" width="300">
<img src="https://user-images.githubusercontent.com/87808288/179532034-6c34a923-2d57-4ba4-80c4-7f254958892e.png" width="310">  

`reset`은 <u>HEAD는 바꾸지 않고</u> <span style="color:tomato">branch가 가리키는 것을 바꾸게</span> 된다.  
reset의 결과로는 HEAD와 branch 모두 C commit을 가리키지 않고 있어  
"git log"를 해보면 결과적으로 마치 <span style="color:blue">삭제된 것처럼 보이게</span> 된다.  
<img src="https://user-images.githubusercontent.com/87808288/179532362-5659964e-88bb-4593-a1ec-eaf613f67c20.png" width="300">
<img src="https://user-images.githubusercontent.com/87808288/179532627-a59bd53e-987f-46ed-a588-ab33e5af5b80.png" width="315">  

# 2장 reset
## 1. 트리
`Git`을 <span style="color:blue">서로 다른 세 트리를 관리하는 컨텐츠 관리자</span>로 생각하면 reset을 좀 더 쉽게 이해할 수 있다.  
'<span style="color:blue">트리</span>'라는 말은 '<span style="color:royalblue">파일의 묶음</span>'을 개념적으로 이르는 말이다.  

Git은 일반적으로 세 가지의 트리를 관리하는 시스템이다.  
- HEAD: 마지막 커밋 스냅샷, 다음 커밋의 부모 커밋
- Index: 다음에 커밋할 스냅샷
- 워킹 디렉토리: 샌드박스

### (1) HEAD
<span style="color:red">HEAD</span>는 <span style="color:tomato">현재 브랜치를 가리키는 포인터</span>이며, 브랜치에 담긴 커밋 중 <span style="color:tomato">가장 마지막 커밋</span>을 가리킨다.  

### (2) Index
Index는 <span style="color:tomato">바로 다음에 커밋할 것들</span>이다.  
<span style="color:red">Staging Area</span>는 사용자가 git commit 명령을 실행했을 때 Git이 처리할 것들이 있는 공간이다.  

Index는 워킹 디렉토리에서 마지막으로 checkout 한 브랜치의 파일 목록과 파일 내용으로 채워진다.  
이후 파일 변경작업을 하고 변경한 내용으로 Index를 업데이트할 수 있다.  
이렇게 업데이트 하고 'git commit' 명령을 실행하면 Index는 새 커밋으로 변환된다.  

Index는 엄밀히 말해 트리구조는 아니다. 사실 Index는 평평한 구조로 구현되어 있다.  

### (3) 워킹 디렉토리
위의 두 트리(HEAD, Index)는 파일과 그 내용을 효율적인 형태로 .git 디렉토리에 저장한다.  
하지만, 사람이 알아보기 어렵다.  
`워킹 디렉토리`는 실제 파일로 존재한다. <span style="color:royalblue">바로 눈에 보이기 때문에 사용자가 편집하기 수월</span>하다.  
워킹 디렉토리는 <span style="color:blue">샌드박스</span>로 생각하자.  
커밋하기 전에는 Index(Staging Area)에 올려놓고 얼마든지 변경할 수 있다.  

## 2. 워크플로우
Git의 주목적은 프로젝트의 스냅샷을 지속적으로 저장하는 것이다.  
<img src="https://user-images.githubusercontent.com/87808288/169641987-fdcf1178-4904-479d-bfc1-b1e9991783dd.png" width="500">  

파일이 하나 있는 디렉토리로 이동한다.  
이것을 <u>파일의 v1이라고 하고 파란색으로</u> 표시한다.  
<span style="color:green">git init</span> 명령을 실행하면 Git 저장소가 생기고 HEAD는 아직 없는 브랜치를 가리킨다.  
<img src="https://user-images.githubusercontent.com/87808288/169642084-94d77ef0-6ac3-40c1-a235-6b783549f215.png" width="500">  
이 시점에는 `워킹 디렉토리` 트리에만 데이터가 있다.  
이제 <span style="color:green">git add</span> 명령으로 <u>워킹 디렉토리의 내용을</u> <span style="color:blue">Index(staging area)로 복사</span>한다.  

<img src="https://user-images.githubusercontent.com/87808288/169642199-98570a4f-1539-4f0e-ad6c-ab6506de33e0.png" width="500">  

그리고 <span style="color:green">git commit</span> 명령을 실행한다.  
그러면 <span style="color:royalblue">Index(staging area)의 내용을 스냅샷으로 영구히 저장</span>하고 그 <span style="color:blue">스냅샷을 가리키는 커밋 객체를 만든다</span>.  
그리고 `master가 그 커밋 객체를 가리키도록` 한다.  
<img src="https://user-images.githubusercontent.com/87808288/169642369-4c97473e-ea4c-446b-97b9-a3225b483d51.png" width="500">  

이때 <span style="color:green">git status</span> 명령을 실행하면 <u>아무런 변경 상황이 없다고</u> 나온다.  
그 이유는 <span style="color:royalblue">세 트리가 모두 같기 때문</span>이다.  

다시 파일의 내용을 바꾸고 commit해보자.  
<img src="https://user-images.githubusercontent.com/87808288/169642542-56c53e7c-4c0b-40c1-a359-55f546f2da90.png" width="500">  
그러면 우선 워킹 디렉토리의 파일을 고친다. 이를 파일의 v2라고 하며, 빨간색으로 나타낸다.  

<span style="color:green">git status</span> 명령을 실행해보면  
“Changes not staged for commit,” 아래에 빨간색으로 된 파일을 볼 수 있다.  
<u>Index와 워킹 디렉토리가 다른 내용을 담고 있기 때문</u>이다.  
<span style="color:green">git add<span> 명령을 실행해서 변경 사항을 Index에 올려주자.  
<img src="https://user-images.githubusercontent.com/87808288/169642734-be2df0b2-7a8e-4582-8ff8-aefae5db6eda.png" width="500">  

위 그림에서의 시점으로 <span style="color:green">git status</span> 명령을 실행하면 Changes to be committed” 아래에 파일 이름이 녹색으로 변한다.  
Index와 HEAD의 다른 파일들이 여기에서 표시된다.  
즉 <span style="color:blue">다음 커밋할 것과 지금 마지막 커밋이 다르다</span>는 말이다.  
마지막으로 <span style="color:green">git commit</span> 명령을 실행해 커밋한다.  

<img src="https://user-images.githubusercontent.com/87808288/169642858-62890bc3-d66b-4511-a3db-d945056ca987.png" width="500">  
위의 사진의 시점에서 <span style="color:green">git status</span> 명령을 실행하면 아무것도 출력되지 않는다.  \
세 개 트리의 내용이 일치하기 때문이다.  

브랜치를 바꾸거나 Clone 명령도 내부에서는 비슷한 절차를 밟는다.  
`브랜치를 Checkout` 하면, <u>HEAD가 새로운 브랜치를 가리키도록</u> 바뀌고, 새로운 커밋의 스냅샷을 Index에 놓는다.  
그리고 Index의 내용을 워킹 디렉토리로 복사한다.  

## 3. Reset 알아보기
### (1) reset --soft
예를 들어 file.txt 파일 하나를 수정하고 커밋한다. -> 이것을 세 번 반복한다.  
<img src="https://user-images.githubusercontent.com/87808288/169646158-49719a13-6734-4114-ac61-22b6a4216c32.png" width="500">  

reset 명령은 이 세 트리를 간단하고 예측 가능한 방법으로 조작한다.  

[1단계: HEAD 이동]  
<span style="color:red">reset 명령</span>이 하는 첫 번째 일은 <span style="color:tomato">HEAD 브랜치를 이동</span>시킨다.  
checkout 명령처럼 <u>HEAD가 가리키는 브랜치를 바꾸지는 않는다</u>.  
<span style="color:blue">HEAD는 계속 현재 브랜치를</span> 가리키고 있고, <span style="color:tomato">현재 브랜치가 가리키는 커밋을 바꾼다</span>.  
HEAD가 master 브랜치를 가리키고 있다.  
(즉 master 브랜치를 Checkout 하고 작업하고 있다면) <span style="color:green">git reset 9e5e6a4</span> 명령은 master 브랜치가  
`9e5e6a4`를 가리키게 한다.  
<img src="https://user-images.githubusercontent.com/87808288/169646400-197ebdce-53cc-49b2-b519-e10a255db945.png" width="500">  
<span style="color:green">reset --soft</span> 옵션을 사용하면 딱 여기까지 진행하고 동작을 멈추게 된다.  

<span style="color:red">reset 명령</span>은 <span style="color:tomato">가장 최근의 git commit 명령을 되돌린다</span>.  
git commit 명령을 실행하면 Git은 새로운 커밋을 생성하고  
HEAD가 가리키는 브랜치가 새로운 커밋을 가리키도록 업데이트한다.  
reset 명령 뒤에 <span style="color:green">HEAD~</span>(HEAD의 부모 커밋)를 주면 <span style="color:royalblue">Index나 워킹 디렉토리는 그대로 놔두고</span>  
<span style="color:blue">브랜치가 가리키는 커밋만 이전으로</span> 되돌린다.  
Index를 업데이트한 다음에 git commit 명령을 실행하면 git commit --amend 명령의 결과와 같아진다.  

[2단계: Index 업데이트 (--mixed)]  
여기서 <span style="color:green">git status</span> 명령을 실행하면 Index와 reset 명령으로 이동시킨 HEAD의 다른 점이 녹색으로 출력된다.  
<span style="color:red">reset 명령</span>은 여기서 한 발짝 더 나아가 <span style="color:tomato">Index를 현재 HEAD가 가리키는 스냅샷으로</span> 업데이트할 수 있다.  
<img src="https://user-images.githubusercontent.com/87808288/169646805-b5724518-39c4-44fd-a89f-6816451d1874.png" width="500">  
<span style="color:green">--mixed</span> 옵션을 주고 실행하면 reset 명령은 여기까지 하고 멈춘다.  
reset 명령을 실행할 때 <u>아무 옵션도 주지 않으면 기본적으로 --mixed 옵션으로 동작</u>한다.  
(예제와 같이 git reset HEAD~ 처럼 명령을 실행하는 경우)  
위의 그림을 보고 어떤 일이 일어날지 생각해보자면,  
가리키는 대상을 가장 최근의 커밋으로 되돌리는 것은 같다. 그러고 나서 Staging Area를 비우기까지 한다.  
<span style="color:blue">git commit 명령도 되돌리고 git add 명령까지 되돌리는 것</span>이다.  

[3단계: 워킹 디렉토리 업데이트 (-&nbsp;-hard)]  
<span style="color:red">reset 명령</span>은 세 번째로 <span style="color:tomato">워킹 디렉토리까지 업데이트</span>한다.  
<span style="color:green">-&nbsp;-hard</span> 옵션을 사용하면 reset 명령은 아래의 그림의 단계까지 수행한다.  
<img src="https://user-images.githubusercontent.com/87808288/169647301-6719bb27-5485-4aea-b644-474a271bedc9.png" width="500">  
위 그림의 과정은 reset 명령을 통해 <span style="color:green">git add</span> 와 <span style="color:green">git commit</span> 명령으로 생성한 마지막 커밋을 되돌린다.  
그리고 <span style="color:tomato">워킹 디렉토리의 내용까지도 되돌린다</span>.  
<span style="color:red">- -hard 옵션은 매우 중요</span>하다. <u>reset 명령을 위험하게 만드는 유일한 옵션</u>인 것이다.  
Git에는 데이터를 실제로 삭제하는 방법은 많이 없는데, 위의 방법이 그중 하나인 것이다.  
- -hard 옵션은 되돌리는 것이 불가능한 방법이다. 이 옵션을 사용하면 워킹 디렉토리의 파일까지 강제로 덮어쓴다.  
(위 예제는 Git이 커밋으로 v3번을 아직 보관하고 있기 때문에 reflog를 이용해서 다시 복원할 수 있다.)  

## 경로를 주고 Reset 하기
<span style="color:green">git reset file.txt</span> 명령을 실행한다고 가정해보자.  
이 형식은 <span style="color:green">git reset - -mixed HEAD file.txt</span>를 짧게 쓴 것이다.  
<img src="https://user-images.githubusercontent.com/87808288/169648003-a8b0c745-44c5-42c7-b873-31ea530a446d.png" width="500">  
본질적으로는 file.txt 파일을 HEAD에서 Index로 복사하는 것뿐이다.  
이 명령은 해당 파일을 Unstaged 상태로 만든다.  
<span style="color:green">git add</span> 명령을 비교해보면 <span style="color:royalblue">정확히 반대인 것</span>을 확인할 수 있다.  
HEAD의 브랜치를 옮기는 것은 건너뛰고, <span style="color:tomato">Index를 HEAD가 가리키는 상태로</span> 만든다.  
<img src="https://user-images.githubusercontent.com/87808288/169648087-07eeeb4a-2185-4c06-a0c4-1a67b87b101d.png" width="500">  

# 3장 revert
<img src="https://user-images.githubusercontent.com/87808288/179989610-a31c4932-635f-41bc-85c0-12fdfa2e0bcf.png" width="35%">
<img src="https://user-images.githubusercontent.com/87808288/179988388-6362cedb-1f5f-4db5-ac7f-acb379c5df9b.png" width="30%">
<img src="https://user-images.githubusercontent.com/87808288/179990353-9554b847-b6d7-4c73-8a6f-4c70e30273ca.png" width="30%">  
<span style="color:green">git reset --hard A</span>의 경우에는 HEAD, staging area, working directory 모두를 A로 바꾸는 작업이고,  
<span style="color:green">git revert B</span>는 <span style="color:blue">B commit 내용만을 제거</span>하는 작업이다.  
결과적으로 `reset`은 <u>history를 삭제</u>하는 효과를 가져오고  
`revert`는 <span style="color:tomato">history를 기록</span>하며 reset과 같은 결과를 가져오는 것이다.  

<img src="https://user-images.githubusercontent.com/87808288/179992875-3c18763e-8d8f-4dd4-8409-3a0e0f67ff24.png" width="60%">  
위의 상태에서 <span style="color:green">git reset --hard bbb1ded</span>를 하면 아래의 이미지와 같은 결과가 나온다.  
<u>work 2 - fail</u> commit이 <span style="color:blue">삭제된 것처럼</span> 결과가 나온다.  
<img src="https://user-images.githubusercontent.com/87808288/179993396-9a41ee85-1fb5-4fe7-98fe-2370d39cf41b.png" width="50%">  

다시 아래의 상태에서 이제 <span style="color:green">git revert bbb1ded</span>를 실행하면  
<img src="https://user-images.githubusercontent.com/87808288/179992875-3c18763e-8d8f-4dd4-8409-3a0e0f67ff24.png" width="60%">  
이렇게 아래의 이미지처럼 <u>Revert "work 2 - fail"</u>이라는 commit이 남았다.  
<img src="https://user-images.githubusercontent.com/87808288/179994206-1d4d9f5b-65ea-4349-adb3-735c3a46115c.png" width="60%">  
<u>Revert "work 2 - fail"</u> commit의 내용을 살펴보면 -> "2 - fail"이라는 내용은 삭제되고  
<span style="color:royalblue">work 1 commit의 기록만</span>이 존재하는 것을 확인할 수 있다.  

하지만 아래와 이미지와 같은 조금은 복잡한 상황이 있다.  
<img src="https://user-images.githubusercontent.com/87808288/180004338-bae85cc8-9f8b-42c3-87e9-5ffbd409d9ce.png" width="60%">  
<u>B commit</u>의 코드에서 <span style="color:tomato">fail이라고 하는 버그</span>가 있었는데  
이를 확인하지 못하고 C commit이 진행되었고  
B commit의 내용에 <span style="color:blue">추가적으로 success라는 코드까지 추가</span>되어 commit 되었고 <u>D commit이 완성</u>되었다.  
현재 상황에서 우리는 "2 fail"은 삭제해야하지만 "success"는 살려두어야하는 것이다.  

위의 복잡한 상황을 해결하기 위해서 "<span style="color:red">3 way merge</span>"라는 개념을 알고있어야 한다.  
<img src="https://user-images.githubusercontent.com/87808288/180007514-a10c4d22-6520-49bb-a152-6af2819fe702.png" width="60%">  
git은 merge를 진행할 때 -> <span style="color:tomato">base를 포함</span>하여 3 way merge를 진행한다.  
<span style="color:blue">3개의 commit을 비교</span>하여 <span style="color:tomato">3자가 모두 다를 때</span> git은 merge 중에 <span style="color:red">conflict을 발생</span>시켜  
merge의 결과를 <u>사용자가 지정하도록 설정</u>하는 것이다.  

<img src="https://user-images.githubusercontent.com/87808288/180004338-bae85cc8-9f8b-42c3-87e9-5ffbd409d9ce.png" width="40%">
<img src="https://user-images.githubusercontent.com/87808288/180604627-d6520b5c-1497-4f44-bb84-f1df9de53835.png" width="50%">  

<img src="https://user-images.githubusercontent.com/87808288/180604749-e3397dde-3cdd-4996-9152-243e38cbfeb1.png" width="60%">  


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