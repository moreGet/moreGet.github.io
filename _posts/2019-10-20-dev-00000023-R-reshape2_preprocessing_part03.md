---
date: 2019-10-20 19:24:00
layout: post
title: R-Studio reshape2를 이용한 전처리 Part.3
subtitle: R-Studio의 여러 함수들을 이용한 전처리
description: "Current RStudio : == Desktop 1.2.5001(64bit)"
image: https://res.cloudinary.com/dm7h7e8xj/image/upload/v1559820489/js-code_n83m7a.jpg
optimized_image: https://res.cloudinary.com/dm7h7e8xj/image/upload/c_scale,w_380/v1559820489/js-code_n83m7a.jpg
category: coding
tags:
  - R-Studio
author: thiagorossener
# image:760*400
# optimized_image:380*200
---
<!-- 
# R-Studio 도 함수를 만들 수 있다.
> plyr 를 이용하여 merge와 join을 사용하자!<br>
> 파이프라인 함수 를 사용하기 위해 dplyr 로 필요합니다.<br>
> 패키지 로드순서는 plyr -> dplyr 순서로 로드 해주시기 바랍니다. -->

## 파일 소스
우클릭 -> 다른이름으로 링크저장 이용해 주세요<br>
<a href="../assets/sources/dataset.csv" class="btn btn-lg btn-outline">
dataset.csv
</a><br>
<br>

## 사용 예시 소스코드

```r
# 가독성을 위한 코딩 변경

# 실습을 위해 데이터 다시 불러옴
dataSet <- read.csv("dataset.csv", header = T)
dataSet

# 앞에서 6개의 튜플 출력
head(dataSet)
#   resident gender job age position price survey
# 1        1      1   1  26        2   5.1      1
# 2        2      1   2  54        5   4.2      2
# 3       NA      1   2  41        4   4.7      4
# 4        4      2  NA  45        4   3.5      2
# 5        5      1   3  62        5   5.0      1
# 6        3      1   2  57       NA   5.4      2

# 데이터가 너무 방대해 거를꺼 걸러서 작게 하기위해 함.
dataSet <- subset(dataSet, dataSet$price >= 2 & dataSet$price <= 8)
nrow(dataSet)
# [1] 251

# 속성 원자값 들의 도메인 값 출력
unique(dataSet$resident)

# dataSet$새컬럼[조건식] <- "값"
# resident의 원자값이 1이면 서울시
# 가독성을 위해 코딩 변경(분류 해줌) 함.
dataSet$resident2[dataSet$resident == 1] <- "서울시"
dataSet$resident2[dataSet$resident == 2] <- "인천시"
dataSet$resident2[dataSet$resident == 3] <- "대전시"
dataSet$resident2[dataSet$resident == 4] <- "대구시"
dataSet$resident2[dataSet$resident == 5] <- "부산시"

# 가독성을 위해 코딩변경 되었음.
head(dataSet[c("resident", "resident2")], 10)
#    resident resident2
# 1         1    서울시
# 2         2    인천시
# 3        NA      <NA>
# 4         4    대구시
# 5         5    부산시
# 6         3    대전시
# 7         2    인천시
# 9        NA      <NA>
# 10        2    인천시
# 11        5    부산시

# job 공무원, 회사원, 개인사업
# 분석전 수치를 확인한다.
unique(dataSet$job)
# [1]  1  2 NA  3

# 조건을 작성한다.
dataSet$job2[dataSet$job == 1] <- "공무원"
dataSet$job2[dataSet$job == 2] <- "회사원"
dataSet$job2[dataSet$job == 3] <- "개인사업"
# job, job2 컬럼의 튜플을 앞에서 10개 출력한다.
head(dataSet[c("job", "job2")], 10)
# job     job2
# 1    1   공무원
# 2    2   회사원
# 3    2   회사원
# 4   NA     <NA>
#   5    3 개인사업
# 6    2   회사원
# 7    1   공무원
# 9    1   공무원
# 10   2   회사원
# 11  NA     <NA>

# 척도 변경
sort(unique(dataSet$age))
# 비율척도 : 나이 10세 ~ 70세
#  : 30이하 청년층 55이하 중년층 이라고 나누는게 

# 코딩 변경을 해 가독성을 높힌다.
dataSet$age2[dataSet$age <= 30] <- "청년층"
dataSet$age2[dataSet$age > 30 & dataSet$age <= 55] <- "중년층"
dataSet$age2[dataSet$age > 55] <- "장년층"
head(dataSet[c("age", "age2")], 10)
#    age   age2
# 1   26 청년층
# 2   54 중년층
# 3   41 중년층
# 4   45 중년층
# 5   62 장년층
# 6   57 장년층
# 7   36 중년층
# 8   NA   <NA>
# 9   56 장년층
# 10  37 중년층

# 척도변경을 위해 데이터 셋
survey <- dataSet$survey # survey를 추출
maxVal <- max(survey) # survey 속성 원자값중 MAX값만 추출
csurvey <- maxVal + 1 - survey # 값을 뒤집기 위해 알고리즘을 짬.
dataSet$survey2 <- csurvey # 파생컬럼을 만들어 뒤집은 값을 넣어줌
head(dataSet[c("survey", "survey2")], 10) # 앞에서 10개 출력해봄.
#    survey survey2
# 1       1       5
# 2       2       4
# 3       4       2
# 4       2       4
# 5       1       5
# 6       2       4
# 7       4       2
# 9       3       3
# 10      3       3
# 11      5       1
```