---
layout: post
title:  "R-Studio Coding part 2"
subtitle:   "MarkDown"
categories: devlog
tags: rsudio
comments: true
# tags: tip
---

# R - Studio 기본 코딩

> 객체지향 개념으로 접근한다면 이해하기 힘들다.<br>
> person.age / person.name 은 그 자체로 주소가 할당된다.<br>

<br>

---
## Source code
```r
var1 <- 0
var2 <- 100
var3 <- 'aaa'

var1
var2
var3

name <- '홍길동'
age <- 30
hasCar <- TRUE # T만 쳐도 자료형 True로 인식

name
age
hasCar

# NA : Not Available
sum(10, 20, 30)
sum(10, 20, 30, NA)

# na.rm = T : NA는 배제하고 연산하세요.
sum(10, 20, 30, NA, na.rm = T)

help(mean)
mean(10, 20)
mean(10, 20, NA, na.rm = T)

# java의 is메소드 같음.
is.character(name)
is.numeric(age)
is.logical(hasCar)
is.na(var2)

# somedata 벡터에는 정수형 데이터2개와 문자형 데이터 1개 가 들어감
somedata <- c(10, 20, "30")
somedata

# as.numeric() 메소드로 정수형으로 캐스팅 해주고 연산 해야 에러가 안뜸.
result <- as.numeric(somedata) * 3
result

ls() # 현재 사용중인 변수 리스트 보기

data <- c(1, 2)

# 현재는 data 는 기초 자료형이라 mode 와 class 의 결과 값이 같다.
# mode : 자료의 타입
mode(data)

# class : 자료의 구조
class(data)

# 변수.멤버 형태
# 클래스..? 뭐라고 이해해야 하지..?
goods.code <- 101
goods.name <- '냉장고'
goods.price <- 123000
```