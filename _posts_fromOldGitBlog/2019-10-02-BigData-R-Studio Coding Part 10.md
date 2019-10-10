---
layout: post
title:  "R-Studio Coding part 10 행렬 바인드"
subtitle:   "MarkDown"
categories: devlog
tags: rsudio
comments: true
# tags: tip
---

# R - Studio Coding 행렬 바인드

> 매개변수 잘 넣어야 합니다.<br>

<br>

---
## Source code
```r
# R은 행 열 순이다. (기본)행 우선 적으로 모든것이 정의된다.
# 열기준 bind 행렬 아래로 쭉 배열
somedata = cbind( x1 = seq(1, 5), x2 = seq(6, 10))
somedata

# 소문자 a b c d e
letters[1:5]
# 대문자
LETTERS[1:5]

# 행 이름 넣어주기 dimnames()로
dimnames(somedata)[[1]] = letters[1:5]
class(somedata)

# 1=행, 2=열 
# apply() 함수의 인자는 최소3개 이상
apply(somedata, 1, mean) # somedata 가지고 행단위로 평균 구하기
apply(somedata, 2, mean) # somedata 가지고 열단위로 평균 구하기

# 마지막 인자가 덧셈
apply(somedata, 1, sum) # 행단위 합계
apply(somedata, 2, sum) # 열단위 합계

# apply 함수랑 같은 기능.
colMeans(somedata) # 열단위 평균
rowMeans(somedata) # 행단위 평균

colSums(somedata) # 열단위 합계
rowSums(somedata) # 행단위 합계

# 텍스트 불러오기
textFile = read.table("D:/Rwork/bigdata/S20191001/jumsu.txt", header = T)
textFile

# 1열 빼고 출력
textFile <- textFile[-1]
textFile

# 열 기준 덧셈
b = sapply(textFile, sum)
plot(b) # 

# 열 기준 평균
sapply(textFile, mean)

# 리스트 형태로 바꾸기 컬럼 별로 리스트 요소 만들어주기
sapply(textFile, sum, simplify = F)

```