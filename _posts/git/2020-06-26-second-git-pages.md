---
title: "[GitHub 블로그 만들기] 2. Jekyll를 사용하여 GitHub 블로그 설정하기 "
categories: github블로그만들기
tags: blog github Jekyll
toc: true
toc_sticky: true
toc_lavel: "목차"
---

GitHub pages를 올린 후 Jekyll를 사용하여 테마를 적용하고 배포 과정을 포스팅하려 한다.  

# Jekyll 이란?
---
## 장점  
* 정적인 사이트(서버가 필요없고 html/css로만 이루어져있다.)  
=> __빠르다, 가볍다__
* 텍스트 변환엔진, HTML로만 작성하게 되면 생산성이 무척 떨어진다. 마크다운 문법으로 작성 하면 규칙에 맞는 레이아웃을 생성해준다.  
=> __생산성 향상__  
* github에 commit과 push만으로 배포가 가능하다
=> __편리성__

## 단점  
단점으로는 장점을 100%의 효율로 올리기 위해서는 `마크다운 문법`과 `github사용법`을 익혀야 하기 때문에
진입장벽이 높다.

# 1. Jekyll 테마
---
지킬은 루비언어로 되어있고, 오픈소스여서 많은 테마를 제공하고 있다.  
## 테마 제공 사이트
 * http://jekyllthemes.org/
 * https://jekyllthemes.io/free
 * http://themes.jekyllrc.org/
 * https://github.com/topics/jekyll-theme  

이 테마들 중에서 원하는 테마를 고르면 된다. 나는 __minimal-mistakes__ 선택했다.

# 2. 테마 적용하기
---
먼저 [minimal-mistakes github](https://github.com/mmistakes/minimal-mistakes)에서 로컬로 가져온다.
`직접 다운로드`하거나 `clone`으로 가져온다.

직접 다운로드를 한다면 다음의 위치에서 다운로드 받을 수 있다.
![이미지](https://github.com/saeyeoni/saeyeoni.github.io/blob/master/_images/second-git-pages-1.png?raw=true "repo")

clone하는 방법으로 테마를 가져오겠다.
clone할 위치로 이동한 `init` `clone`을 실행한다.
```
git init
git clone https://github.com/mmistakes/minimal-mistakes
```
위를 실행한 위치에 테마가 다운로드된걸 확인 할 수 있다.
![이미지](https://github.com/saeyeoni/saeyeoni.github.io/blob/master/_images/second-git-pages-2.png?raw=true "repo")
# 3. GitHub Pages에 적용하기
---
clone한 테마를 github에 올려보겠다.

1. 가져온 테마의 프로젝트 이름을 적절하게 변경한다. [username.github.io]를 추천한다.
```
md minimal-mistakes [변경할폴더명]
```

2. 테마프로젝트 안에서 GitHub(username.github.io)으로 push 해주면 된다.

clone시 이미 init되어있기 때문에 remote 주소를 확인 후 자신의 원격저장소로 변경한 후 진행하면 된다.
```
git removte -v
git remote remove origin
git remote add origin https://github.com/username/refository
git add .
git commit -m "first myblog"
git push -u origin master
```

push 완료 후 블로그를 새로고침해보면 사이트가 올라간것을 볼 수 있을 것이다.
다만 원격에 올릴 시 _몇 분의 시간이 필요_ 하다.
마크다운으로 작성한 것을 에디터의 뷰어를 통해 확일 할 수 있지만 전체적인 부분을 바로바로 확인 하기 어렵다는 문제가 있다.
로컬환경도 구축하여 작업하는 것이 좋겠다.
