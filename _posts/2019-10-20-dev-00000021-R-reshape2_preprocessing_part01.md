---
date: 2019-10-20 19:22:00
layout: post
title: R-Studio reshape2를 이용한 전처리 Part.1
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

<!-- ## 파일 소스
우클릭 -> 다른이름으로 링크저장 이용해 주세요<br>
<a href="../assets/sources/member.csv" class="btn btn-lg btn-outline">
member.csv
</a><br>
<a href="../assets/sources/board.csv" class="btn btn-lg btn-outline">
board.csv
</a><br>
<a href="../assets/sources/2013년_프로야구선수_성적 (1).csv" class="btn btn-lg btn-outline">
2013년_프로야구선수_성적 (1).csv
</a><br>
<br> -->

## 사용 예시 소스코드

```r
name <- c("김", "유", "강", "이", "신")
gender<- c("남자", "여자", NA, "남자", "여자")
score <- c(10, 15, 20, 25, NA)

df <- data.frame(name=name, gender=gender, score=score)
str(df)
# 'data.frame':	5 obs. of  3 variables:
# $ name  : Factor w/ 5 levels "강","김","신",..: 2 4 1 5 3
# $ gender: Factor w/ 2 levels "남자","여자": 1 2 NA 1 2
# $ score : num  10 15 20 25 NA

# NA값이 있는 속성값 출력
is.na(df)
#       name gender score
# [1,] FALSE  FALSE FALSE
# [2,] FALSE  FALSE FALSE
# [3,] FALSE   TRUE FALSE
# [4,] FALSE  FALSE FALSE
# [5,] FALSE  FALSE  TRUE

# TRUE = 1 이라서 sum으로 더하면 2개 가 나옴
sum(is.na(df))
# [1] 2

# 테이블에 NA 값을 TRUE 로 치고 몇개 있는지.
# FALSE (정상값) 은 몇개 있는지.
table(is.na(df))
# FALSE  TRUE 
# 13     2 

library(dplyr)
# df에서 score속성 의 원자값들 중 NA인것
df %>% filter(is.na(score))
#   name gender score
# 1   신   여자    NA
# df에서 gender속성 의 원자값들 중 NA인것
df %>% filter(is.na(gender))
#   name gender score
# 1   강   <NA>    20

# 결측치 제거
# df에서 score속성 원자값들 중 NA인것을 제외한 나머지 릴레이션 인스턴스(튜플) 출력.
df_nomiss <- df %>% filter(!is.na(score))
df_nomiss
# name gender score
# 1   김   남자    10
# 2   유   여자    15
# 3   강   <NA>    20
# 4   이   남자    25


# 모든 속성에서 NA 다뺌
df_nomiss <- df %>% filter(!is.na(score) & !is.na(gender))
df_nomiss
#   name gender score
# 1   김   남자    10
# 2   유   여자    15
# 3   이   남자    25

# omit : 생력하다.
# 결측치 생략 함수.
df_nomiss2 <- na.omit(df)
df_nomiss2
#   name gender score
# 1   김   남자    10
# 2   유   여자    15
# 4   이   남자    25

students <- read.csv("jumsu2.csv")
str(students)
# 'data.frame':	20 obs. of  5 variables:
# $ id  : int  1 2 3 4 5 6 7 8 9 10 ...
# $ ban : int  1 1 1 1 2 2 2 2 3 3 ...
# $ kor : int  45 58 39 44 35 65 90 25 87 84 ...
# $ eng : int  71 73 91 28 66 79 89 82 98 21 ...
# $ math: int  32 54 37 88 49 96 20 98 59 56 ...

# 결측치 만들어 주기.
students[c(3, 8, 15), "kor"] <- NA
# 결측치 때문에 평균 값이 안나옴
students %>%summarise(mean_kor=mean(kor))
#   mean_kor
# 1       NA

# kor속성 값들중 NA값 빼고 나머지 원자값 들의 평균
students %>% summarise(mean_kor=mean(kor, na.rm = T))
#   mean_kor
# 1 58.23529

# kor컬럼 중 속성 원자값의 결측치가 몇개 인지.(TRUE = NA)
table(is.na(students$kor))
# students
mean(students$kor, na.rm = T)

# 평균을 구하여, NA 값에 대하여 평균으로 대체.
mean_kor <- mean(students$kor, na.rm = T) # 평균 값을 먼저 계산하고.
# [1] 58.23529

# ifelse로 값을 대입 해준다. 
# 만약 조회하는 속성 원자값이 NA 갑이면 mean_kor 값을 대입하고 아니면 원래 속성 원자값을 대입한다.
students$kor <- ifelse(is.na(students$kor), mean_kor, students$kor) 
table(is.na(students$kor))
# FALSE 
# 20 

# 실습을 위해 다시 NA값 원복
students[c(3, 8, 15), "kor"] <- NA
table(is.na(students$kor))
# 결측치 3개 확인.
# FALSE  TRUE 
# 17     3 

# NA 값은 디폴트 값으로 주던가 아니면 위처럼 평균값을 주던가.
# 데이터 분석을 위해 전처리 할때 고민해봐야 한다.
myDefault <- 40
students$kor <- ifelse(is.na(students$kor), default, students$kor)
table(is.na(students$kor))
# 결측치가 사라짐.
# FALSE 
# 20

# 이상치(outlier) : 존재할 수 없는 이상한 값
# 새로운 실습을 위해 데이터 프레임 만들기
gender <- c(1, 2, 3, 1, 2, 1) # 성별값 3이 이상치
jumsu <- c(2, 5, 3, 4, 1, 20)  # 점수값 기준 1~5 이므로 6이 이상치

# 이상치 그래프 그려주기.
boxplot(jumsu)

outlier <- data.frame(gender=gender, jumsu=jumsu)
str(outlier)
# 'data.frame':	6 obs. of  2 variables:
# $ gender: num  1 2 3 1 2 1
# $ jumsu : num  2 5 3 4 1 6

# 정렬 해보기(오름 차순 정렬)
sort(unique(outlier$gender))
# [1] 1 2 3 # 3은 이상치
sort(unique(outlier$jumsu))
# [1] 1 2 3 4 5 6 # 6은 이상치
table(outlier$gender)
# 남자(1) 가 3명 여자(2) 가 2명 인데 코드3 은 누군지 모름.
# 1 2 3 
# 3 2 1

# gender에서 1, 2가 아니면 NA로 치환
outlier$gender <- ifelse(outlier$gender == 3, NA, outlier$gender)
table(outlier$gender, useNA = "ifany")
# table함수의 useNA는 NA 값까지 출력 해준다.
# 남자(1) 3, 여자(2) 2, NA 1
# 1    2    <NA> 
# 3    2    1 

# 5보다 크면 NA으로 치환하세요.
# 1 ~ 5 사이의 점수가 아니면 NA로 치환 한다.
outlier$jumsu <- ifelse(outlier$jumsu > 5 | outlier$jumsu < 1, NA, outlier$jumsu)
table(outlier$jumsu, useNA = "ifany")
# 1    2    3    4    5 <NA> 
# 1    1    1    1    1    1 


outlier %>% filter(!is.na(gender) & !is.na(jumsu)) %>%  group_by(gender) %>% summarise(mean_jumsu = mean(jumsu))
# 1은 남자, 2는 여자
# gender, jumsu 속성 원자값 중 NA값은 제외 시키고. 
# gender로 그룹핑하여 jumsu컬럼의 평균치를 출력
# A tibble: 2 x 2
# gender mean_jumsu
# <dbl>      <dbl>
# 1      1          3
# 2      2          3

# 유저 그래픽 패키지
install.packages("ggplot2")
library(ggplot2)

str(mpg) # https://cafe.naver.com/ugcadman/1113
range(mpg$hwy)
sort(unique(mpg$hwy)) # gallon 당 고속도로 주행 마일수

# 그래프 그리기
# boxplot는 이상치 가시화에 특화된 차트
mybox <- boxplot(mpg$hwy)

# 차수(Dgree) 정보 출력
attributes(mybox)
# $names
# [1] "stats" "n"     "conf"  "out"   "group" "names"

# 4분위수(통계:stats(준말)) 출력 1=0%, 2=4/1, ... 5는 최고 이상치
mybox$stats
# [,1]
# [1,]   12
# [2,]   18
# [3,]   24
# [4,]   27
# [5,]   37
# attr(,"class")

# 실습을 위해 조건에 맞는 부분을 결측치로 바꿔 버림.
mpg$hwy <- ifelse(mpg$hwy<12 | mpg$hwy>37, NA, mpg$hwy)
table(is.na(mpg$hwy))
# FALSE  TRUE 
# 231     3 

# drv : 자동차 구동방식
result <- mpg %>% group_by(drv) %>%  summarise(mean_hwy = mean(hwy, na.rm = T))
result
# A tibble: 3 x 2
# drv   mean_hwy
# <chr>    <dbl>
# 1 4         19.2
# 2 f         27.7
# 3 r         21
```