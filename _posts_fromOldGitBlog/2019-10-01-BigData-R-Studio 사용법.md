---
layout: post
title:  "R-Studio 사용법"
subtitle:   "MarkDown"
categories: devlog
tags: rsudio
comments: true
# tags: tip
---

# BigData 기초 자료 형태

> ctrl + shift + N : 새 스크립트 파일 생성(작업 공간)<br>
> 스크립트 작업후 항상 가비지 클린 해주기(오른쪽 변수 List)<br>
> ctrl + L : 콘솔창에서 누르면 콘솔 clear 해줌

<br>

---
## R-Studio 주의사항
- 컴퓨터 이름은 영어로 해야 문제 없다.
- 스크립트 확장자는 .R 이다.
- object를 클리너 할때 inclide hidden objects 꼭 체크 해줘야 한다
- Tools -> option -> code -> Soft-wrap R source files 체크
  - 스크립트 상 코드가 길면 워크스페이스 크기에 따라 코드를 자동 줄개행 해줌
- Tools -> option -> code -> saving탭 -> 문자인코딩 UTF-8 로 변경
- 주석 키는 ctrl+shift+C / 설정법 : tools -> modify -> KeyBoardShotcut -> comment 찾아서 바꾸기
- c 변수는 벡터생성 변수이기 때문에 사용하지 말아야 한다.