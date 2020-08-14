---
title: "[python] 들여쓰기 오류(unindent does not match any outer indentation level)"
categories: python
tags: python
toc: true
toc_sticky: true
toc_lavel: "목차"
---
들여쓰기 오류는 파이썬 IDLE에서 기초문법을 학습하면서 겪었던 에러중 가장 잦았던,  
파이썬을 처음 접하는 사람이라면 꼭 한 번쯤 봤을법한 에러다.  

# 들여쓰기
---
### Python에서 들여쓰기란
----
C,C++,JAVA,PHP 등 지금까지 접한 언어는 `{ }`과 `;`으로 구분짓는다.   
하지만 파이썬에서는 `공백(스페이스)`, `tap`, `:`으로 영역이 나뉘게 된다.
그럼으로 코드 한줄 한줄 들여쓰기가 아주 중요한 역할이다.  


### 잘못된 들여쓰기
----
먼저 에러가 난 코드부터 보자
![이미지](https://github.com/saeyeoni/saeyeoni.github.io/blob/master/_images/indentation_error.png?raw=true "repo")

unindent does not match any outer indentation level에러가 발생했다. 보면 파란색 블럭만큼 위의 단락보다 안으로 더 들어와저있다.
 위의 if문과 elif문은 같은 레벨의 제어문임으로 같은 정도의 들여쓰기를 해주어야 한다.   

### 들여쓰기 방법
---
들여쓰기는 한칸, 두칸, 4칸, 탭 등 여러 방식이 있고 혼용해서 사용하면 안된다.

### 주의할점
---
* 들여쓰기 방법을 혼합하지 않을 것
  - 처음부터 탭으로 시작했으면 끝까지 탭으로 구분, 처음부터 공백(스페이스)였다면 끝까지 스페이스로 구분
* 같은 레벨(블록)에서는 같은 칸 수 사용
* 함수 생성 후 다음 아랫줄은 반드시 들여쓰기 사용
