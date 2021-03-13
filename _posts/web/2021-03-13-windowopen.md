---
title: "[javascript]window.open()으로 웹에서 팝업창 띄우기"
categories: web
tags: javascript window.open()
toc: true
toc_sticky: true
toc_lavel: "목차"
---

## 문법
> ### let popUp = window.open("URL", "name", "specs", "replace");

## 파라미터
### 1. popUp 반환값
  팝업창 객체이며 호출에 실패하면 null을 반환한다.  
  새 창의 파라미터들을 제어할 수 있다.  
  `popUp.close()`, `popUp.focus()`등으로 사용할 수 있다.


### 2. URL
  선택사항으로 미입력시 빈 새창을 반환한다.
  새롭게 띄울 주소를 입력하는 파라미터이다.  
  절대경로, 상대경로로 특정 파일을 열 수 있고, 링크로 새창을 열 수 있다.
  ex)  
  ```
  // 경로지정 형태
  window.open("/popUp.html", "","")  
  // 링크값 형태
  window.open("https://naver.com", "","")
  ```

### 3. name
   새 창의 이름으로 같은 이름이면 상태가 새로고침되고 다른 이름일때 새창을 띄워준다.  
   옵션은 다음과 같다.
   * _blank  : URL이 새 창 또는 탭에 로드됩니다. 기본값
   * _parent : URL이 상위 프레임에 로드됩니다.
   * _self   : URL이 현재 페이지를 대체합니다.
   * _top    : URL이 로드될 수 있는 프레임셋을 대체합니다.
   * name    : 창의 이름(참고: name 새 창의 title을 지정하지 않음)

### 4. specs
  window.open()은 다양한 옵션값을 갖고 있다.

| 옵션명 |   속성 값   |  특징  |
|:----------|:-------------:|:------|
| channelmode | yes/no, 1/0 | 전체화면으로 창이 열립니다. IE에서만 동작 |
| directories | yes/no, 1/0 | (사용X)디렉토리 버튼을 추가할지 여부. 기본값 TRUE. IE에서만 동작 |
| fullscreen | yes/no, 1/0 | 전체 화면 모드. IE에서만 동작 |
| height | number (px) | 창의 높이 값 |
| width | number (px) | 창의 너비 값 |
| left | number (px) | 창의 위치 (왼쪽으로부터) |
| top | number (px) | 창의 위치 (위쪽으로부터) |
| location | yes/no, 1/0 | 주소 표시줄 사용여부. Opera에서만 동작|
| menubar | yes/no, 1/0 | 메뉴바 사용여부 |
| resizable | yes/no, 1/0 | 창의 리사이즈 가능 여부.IE에서만 동작 |
| scrollbars | yes/no, 1/0 | 스크롤바 사용여부.  IE, Firefox, Opera에서 동작|
| status | yes/no, 1/0 | 상태바 여부 |
| titlebar | yes/no, 1/0 | 타이틀바 여부. 호출 응용 프로그램이 HTML 응용 프로그램이거나 신뢰할 수있는 대화 상자가 아니면 무시됨|
| toolbar | yes/no, 1/0 | 툴바 여부.  IE, Firefox에서 동작|  


### 5. 사용예시
```
window.open("./popup.html","_self","width=300, height=480,left=800,top=200,location=no");
```
