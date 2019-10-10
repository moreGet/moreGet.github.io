---
layout: post
title:  "R-Studio Coding part 1"
subtitle:   "MarkDown"
categories: devlog
tags: rsudio
comments: true
# tags: tip
---

# R - Studio 기본 코딩

> a = 10 / a <- 10 은 같은 의미 이다.<br>
> <br>

<br>

---
## Source code
```r
# available.packages() = 현재 활성화 되어있는 패키지
# dim = 행렬 정보 출력 해주는 메소드 dim([var])
dim(available.packages())

# head([method], [row])/디폴트 매개변수6 = 앞행 6줄 보기
head(available.packages(), 1)

# 현재 사용자가 이용하고있는 시스템의 환경설정 보기
sessionInfo()

# installed.packages() 현재 R라이브러리 패키지 보여줌
head(installed.packages(), 50)

# search() 이미 로딩되어있는 패키지출력
search()

# remove.packages([package name]) 인스톨된 패키지 지움
remove.packages("aaa")

# ggplot으로 막대 그래프 그리기
# install.packages([package name]) 패키지 설치
install.packages("ggplot2")

# 패키지를 메모리에 로딩
library(ggplot2)

x <- c('a', 'a', 'b', 'c') # 벡터 생성후 X에 대입
x # X 출력
qplot(x) # X를 차트화

# data 변수에 c백터 생성
data <- c(1, 2, 3)

# mean() = 평균
mean(data)

# sum() = 합계
sum(data)

# ?은 help
?mean
help(mean)
```