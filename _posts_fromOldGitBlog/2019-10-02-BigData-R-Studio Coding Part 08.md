---
layout: post
title:  "R-Studio Coding part 8 벡터 인덱싱, 슬라이싱"
subtitle:   "MarkDown"
categories: devlog
tags: rsudio
comments: true
# tags: tip
---

# R - Studio Coding 인덱싱, 슬라이싱

> 굉장히 중요한 개념 입니다.<br> 이게 안되면 R안하겠다는 겁니다.

<br>

---
## Source code
```r
somedata <- seq(10, 100, by=10)
somedata

# 9번째 요소 출력
somedata[9]

# 1~3번째 요소 출력
somedata[1:3]

# 요소만 따로 출력
somedata[c(1, 3, 7)]

# 첫번재 요소만 제외하고 출력
result = somedata[-1]
result

# 1부터 3까지를 제외하고 출력
# result0 = somedata[-c(1:3)]
result0 = somedata[-(1:3)]
result0

# 50 제외 전부 출력
result1 = somedata[ somedata != 50 ]
result1

# 70이상 요소만 출력
result2 = somedata[ somedata >= 70 ]
result2

myVector <- seq(1, 6, by=1)
myVector

# 행우선 맵핑
mat_a <- matrix(myVector, nrow = 3)
mat_a
# [,1] [,2]
# [1,]    1    4
# [2,]    2    5
# [3,]    3    6

mat_b <- matrix(myVector, nrow = 2)
mat_b
# [,1] [,2] [,3]
# [1,]    1    3    5
# [2,]    2    4    6

# 열우선 맵핑
mat_c <- matrix(myVector, nrow = 2, byrow = T)
mat_c
# [,1] [,2] [,3]
# [1,]    1    2    3
# [2,]    4    5    6

# 어트리뷰트 속성명 설정 해줌.
mydim <- list( c("김", "박"), c("국어", "영어", "수학") )
mat_d <- matrix(myVector, nrow=2, byrow = T, dimnames = mydim)
mat_d

x = seq(1, 5, by = 1)
y = seq(-1, -5, by = -1)

class(x)

result = x+y
result

# 각요소별 곱의 합
result = x %*% x
result

mode(result)
class(result)

# 벡터 를 벡터 생성하면 벡터
temp = c(x, y)
temp
class(temp)

x
y

# x하고 y를 컬럼 기준으로 바인딩 5행 2열
# 벡터의 길이를 컬럼 기준으로 세로 바인딩
mat_c <- cbind(x, y)
mat_c

# x하고 y를 행 기준으로 바인딩 2행 5열
# 벡터의 길이를 행 기준으로 가로 바인딩
mat_r <- rbind(x, y)
mat_r

# 행렬 갯수 리턴
dim(mat_r)

# 행과 열을 바꿈.
# transfrom metrix(행렬 전치)
mat_r_t = t(mat_r)
mat_r_t

# 행렬정보 조회
dim(mat_r_t)

# 행렬 곱
result = mat_r %*% mat_r_t
result
dim(result)

# 심심 조회
str(result)
summary(result)
```