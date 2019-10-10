---
layout: post
title:  "R-Studio Coding part 3"
subtitle:   "MarkDown"
categories: devlog
tags: rsudio
comments: true
# tags: tip
---

# R - Studio 기본 코딩

> setwd() 명령어는 \\ 보단 / 로 쓰는게 더 편한다.(필자기준)<br>

<br>

---
## Source code
```r
?mean
help(sum)

# args([function]) : 해당 함수의 매개변수 목록을 보고자 할대 사용한다.
args(mean)
args(sum)

# example([function]) : 해당 함수의 예제 출력.
# c(0:10, 50) : 0:10 은 1, 2, 3, .... 부터 10까지 int형 을 다더한것(55) 에 +50을 하고 평균을 구함.
example(mean)

# wd : working directory(워크 스페이스:현재진행 중인 작업 공간)
getwd() # 현재 진행중인 작업공간

setwd("d:/rwork") # 진행 중인 작업공간 바꿀수 있음.

setwd("d:\\rwork") # 이스케이프 시퀀스

getwd() # 워크스페이스 조회

# 파일 불러오기 (커맨드 이용)
setwd("d:/Rwork") # 경로 지정
getwd()

# CSV : comma separate value를 저장하고 있는 텍스트 파일
# 엑셀에서 접근 가능
myTxt <- read.csv("factor_test.txt")
myTxt

# Factor 테스트
getwd()

# str : Structure
# 10 obs. of 5 variavbles 가 나오는데 10행 5열 이다.
str(myTxt)

# levels : 취할 수 있는 값
# labels : 식별할 수 있도록 임의로 지정한 값
# A, B, O, AB 이 4가지 패턴이 혈액형의 levels 가 된다.
# 이걸 한글로 에이형 비형 이런식으로 표현하면 4가지 levels 의 labels 가 된다.

# 'data.frame':	10 obs. of  5 variables:
#   $ no      : int  1 2 3 4 5 6 7 8 9 10
# $ name    : Factor w/ 10 levels "강감찬","근초고대왕",..: 4 10 6 8 1 5 9 7 2 3
# $ blood   : Factor w/ 4 levels "A","AB","B","O": 4 1 4 3 2 1 3 4 3 3
# $ sex     : Factor w/ 2 levels "남","여": 1 1 2 1 1 2 1 1 1 2
# $ location: Factor w/ 7 levels "강원","부산",..: 3 2 2 4 1 1 7 5 6 3

# 해석해 보면 10행 5열 이고 이름 튜플은 팩터형태이고 10레벨을 기준으로 분류한다.
# blood 튜플은 팩터형태 이고 4가지 레벨을 가지고 있고 각각 4레벨을 기준으로 분류한다.
# 팩터 = 카테고리

# 요약정보
summary(myTxt)

# no              name           blood  sex    location
# Min.   : 1.00   강감찬    :1   A :2   남:7   강원:2  
# 1st Qu.: 3.25   근초고대왕:1   AB:1   여:3   부산:2  
# Median : 5.50   뺑덕어멈  :1   B :4          서울:2  
# Mean   : 5.50   서진수    :1   O :3          전남:1  
# 3rd Qu.: 7.75   신사임당  :1                 전북:1  
# Max.   :10.00   유관순    :1                 제주:1  
# (Other)   :4                 충청:1

# 위는 summary() 의 실행 형태이다.
# no 컬럼은 4분위수 에 의거하여 분류한다.
# Min 최솟값, Max 최댓값, median 중간값, mean 평균값

# 기존 팩터를 이용하여 파생 컬럼 만드는법
# 객체$컬럼 : 해당 변수에 접근시 $를 이용한다.
# mytxt객체에서 blood컬럼을 뽑아서 blood2 를 만들어서 객체에 새로운 컬럼을 추가함.
myTxt$blood2 <- factor( myTxt$blood, levels=c('A', 'B', 'O', 'AB'), labels=c('에이', '비', '오', '에이비') )

# 출력
myTxt

# sex컬럼을 이용하여 male, female으로 시뉵 컬럼 sex2를 만들어 보세요.
# factor함수로 만들고자하는 컬럼을 객체 에서 불러와서 labels 를 붙혀서 다시 객체에 추가한다.
myTxt$sex2 <- factor( myTxt$sex, levels=c('남', '여'), labels=c('male', 'female') )

# 출력
myTxt

# 벡터 를 그래프로 시각화 하기
human <- c('남자', '여자', '여자', '남자', '남자')

# 타입 조회
mode(human)
class(human)

# 형변환 nhuman 에다가 human 객체를 팩터로 캐스팅
nhuman <- as.factor(human)
mode(nhuman) # 바뀐 것 확인하기위해 조회
class(nhuman)

# 시각화
plot(nhuman) # 차트 만들기
table(nhuman) # 테이블 만들기
qplot(nhuman) # 좀더 시각적인 차트(유저 라이브러리 사용)

# factor 인수중 ordered = T 이면 사용자가 지정한 levels = c(인수) 순서대로 정렬시킴
ohuman <- factor(human, levels=c('여자', '남자'), ordered=T)
qplot(ohuman) # 그래프 그림

# 강호동 유재석 김구라 를 유재석 김구라 강호동 순으로 차트 그리기기
# 기준이 되는 소스를 먼저 벡터화 시키고
name <- c("강호동", "강호동", "강호동", "유재석", "유재석", "김구라", "김구라", "김구라", "김구라")

# 카테고리화 시키는 중
oname <- factor(name, levels=c("유재석", "김구라", "강호동"), ordered = T)

# 유저 라이브러리 를 이용해 차트를 좀더 이쁘게 그림
qplot(oname)

```