---
layout: post
title:  "R-Studio Coding part 4"
subtitle:   "MarkDown"
categories: devlog
tags: rsudio
comments: true
# tags: tip
---

# R - Studio 기본 코딩

> %/% 몫, %% 나머지<br>
> 시퀀스 는 매개변수를 numeric형 만 써도됨.<br>

<br>

---
## Source code
```r
2+3*4
(2+3)*4

10000
1000000

# e표기법
1e+6 - 900000

0.3
0.00003

a <- 14
b <- 5

# 사칙연산
a+b
a-b
a*b
a/b # 형변환 몫(소숫점 까지 다 계산)

a %/% b # 몫
a %% b # 나머지

2 ** 3 # 2의 3승
2^3 # 2의 3승

su1 <- 17
su2 <- 5

# 몫
su1 %/% su2
# 나머지
su1 %% su2

# 응용1 : 몫 제곱 나머지
(su1 %/% su2) ^ (su1 %% su2)

# :은 연속형 데이터
aa <- c(1:100)
aa

# ,는 이산형 데이터
bb <- c(3:5, 8:9)
bb

# 시퀀스 로 수증가 시키기
cc <- seq(from=1, to=5)
cc

# 시퀀스 로 수증가 사용자 지정 증가값
dd <- seq(from=1, to=10, by=2)
dd

# 시퀀스 반대로 줄이기
ee <- seq(from=10, to=1, by=(-3))
ee

# rev(1:10) 뒤집기 reverse
rev(1:10)

# rep(1:10) 반복 replicates
rep(1, 10)

# 1, 2를 3번 반복
hh <- rep(1:2, 3)
hh

# 서로 3번씩 반복
ii <- rep(1:2, each=3)
ii

# 2, 3, 5 3번 반복
jj <- rep(c(2, 3, 5), 3)
jj

# vectorization
cc <- c(1, 2)
cc^2 # c벡터에 각 원소마다 제곱 해줌

# 1~100의 합에서 3~99 까지의 3의배수의 합의 차를 구함
a <- seq(1, 100, by=1)
b <- seq(3, 99, by=3)
result <- sum(a) - sum(b)
result

# seq를 이용하여 100, 91, 94 ... 1 만들기
a <- seq(100, 1, by=(-3))
a

# seq와 sum을 이용하여 1+9+25+...+9801의 합을 구하시오.
# 참조 : 9801은 99의 제곱이다.
# 정답 : 166650
b <- seq(1, 99, by=2)
sum(b^2)

# 1 - 1/2 + 1/3 - 1/4 + ...+1/99-1/100
# 힌트 : seq를 이용하여 홀수항과 짝수항을 별도로 구하여 연산하면된다.
# 정답 : 0.688172
ex3 <- sum(1 / seq(from=1, to=100, by=2)) - sum(1 / seq(from=2, to=100, by=2))
ex3

# 산수문제
kk <- seq(1, 100, by=3)
sum(kk)

kk2 <- seq(96, 1, by=-5)
sum(kk2)

# abc.csv 읽고
text <- read.csv("abc.txt")
text
summary(text) # 평균값

# 스트럭처 //$연산자는 직접참조 같은 개념
str(text)

# 클래스
culAdd <- factor(text$address, levels = c("seoul", "busan", "daegu"))
culAdd
qplot(culAdd)

# 클래스2
culGender2 <- factor(text$gender, levels = c(2, 1), labels = c("여", "남"))
culGender2
qplot(culGender2)

# 평균값 구하기
mean(text$price, na.rm = T)
```