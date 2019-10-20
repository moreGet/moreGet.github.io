---
date: 2019-10-20 19:23:00
layout: post
title: R-Studio reshape2를 이용한 전처리 Part.2
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
# .csv 파일 불러오기
dataSet <- read.csv("dataset.csv", header = T)
str(dataSet)
# 'data.frame':	300 obs. of  7 variables:
# $ resident: int  1 2 NA 4 5 3 2 5 NA 2 ...
# $ gender  : int  1 1 1 2 1 1 2 1 1 1 ...
# $ job     : int  1 2 2 NA 3 2 1 2 1 2 ...
# $ age     : int  26 54 41 45 62 57 36 NA 56 37 ...
# $ position: int  2 5 4 4 5 NA 3 3 5 3 ...
# $ price   : num  5.1 4.2 4.7 3.5 5 5.4 4.1 675 4.4 4.9 ...
# $ survey  : int  1 2 4 2 1 2 4 4 3 3 ...

View(dataSet) # 테이블로 보여줌
attributes(head(dataSet, 10))
# 속성 구조 출력
# $names
# [1] "resident" "gender"   "job"      "age"      "position" "price"    "survey"  
# 
# $row.names
# [1]  1  2  3  4  5  6  7  8  9 10
# 
# $class
# [1] "data.frame"

# gender컬럼의 속성 원자값 갯수
length(dataSet$gender)
# [1] 300

# 산점도 그래프 출력을 위해 x축과 y축 생성
x <- dataSet$gender
head(x)
# [1] 1 1 1 2 1 1
y <- dataSet$price
head(y)
# [1] 5.1 4.2 4.7 3.5 5.0 5.4
# 산점도 그래프 출력
# (행, 열) 콤마 없으면 열 있으면 행 열 나뉨.
plot(x, y)
plot(y)

# resident의 속성원자값 들 중 중복되는 값 제외 시키고 유일한 값만 출력.
sort(unique(dataSet$resident))
# [1] 1 2 3 4 5
# 속성 원자값 을중 최소값, 최대값 만 출력해줌.
# 즉, 도메인 값 출력.

range(dataSet$age, na.rm = T)
# [1] 20 69

dim(dataSet)
# 300행 7열
# [1] 300   7

# 정보 요약(최대, 최소, 사분위수 등등...)
summary(dataSet$price)
# Min.  1st Qu.   Median     Mean  3rd Qu.     Max.     NA's 
# -457.200    4.425    5.400    8.752    6.300  675.000       30 

# 결측치 제외하고 price 속성 원자값 다 더함
sum(dataSet$price, na.rm = T)
# [1] 2362.9

# 결측치를 제외하고 price2에 price 속성자체를 저장함
price2 <- na.omit(dataSet$price)
sum(price2) # 더함
# [1] 2362.9
length(price2)
# [1] 270 # 결측치 제외한 행수

# 기본 값을 0이라고 가정하고, 0으로 치환하기
dataSet$price2 <- ifelse(!is.na(dataSet$price), dataSet$price, 0)

# 소숫점 2째자리에서 반올림 한 NA값을 제외한 price 컬럼 원자값들의 평균을 계산
mean_price <- round(mean(dataSet$price, na.rm = T), 2)
# price3 컬럼에 price컬럼 원자값이 NA값이 아니라면 원본데이터유지, NA값이라면 평균값으로 치환
dataSet$price3 <- ifelse(!is.na(dataSet$price), dataSet$price, mean_price)
# price, price2, price3 속성들을 출력
dataSet[c("price", "price2", "price3")]

###############################################################################

dataSet <- read.csv("dataset.csv", header = T)
dataSet

# gender컬럼을 table로 출력
gender <- dataSet$gender
table(gender)
# gender
# 0   1   2   5 
# 2 173 124   1 

# pie 그래프 그려보기
# 이상치가 존재 하여 의미 없는 그래프 결과.
pie(table(gender))

dataSet <- subset(dataSet, gender == 1 | gender == 2)
dim(dataSet)
# 전체 300 행에서 gender속성의 원자 값이 1혹은 2 인것만 뽑아오면 결측치 3행 제외
# 297행 이다.
# [1] 297   7

# 실습을 위해 데이터 다시 불러옴
dataSet <- read.csv("dataset.csv", header = T)
dataSet
# price 컬럼의 튜플의 갯수 출력
length(dataSet$price)
# 산점도 차트 출력
plot(dataSet$price)

# 이상치 를 보기위해 출력
mybox <- boxplot(dataSet$price)
# 최소, 최대, 사분위수 출력
mybox$stats
#      [,1]
# [1,]  2.1
# [2,]  4.4
# [3,]  5.4
# [4,]  6.3
# [5,]  7.9

# price 의 속성 원자값들중 2.1 초과 7.9미만 값들만 거름
dataSet2 <- subset(dataSet, dataSet$price > 2.1 & dataSet$price < 7.9)
length(dataSet2$price)
# [1] 248
# 결국 이상치 제외 한 정상범위 값으로 산점도 차트 그림.
boxplot(dataSet2$price)

# price 값을 정렬 하여 보여줌
sort(dataSet2$price)
# [1] 2.3 2.3 3.0 3.0 3.0 3.0 3.0 3.0 3.3 3.3 3.4 3.4 3.5 3.5 3.5 3.5 3.5 3.8 3.8 3.8 3.9 3.9 3.9 4.0 4.0
# [26] 4.0 4.0 4.0 4.0 4.0 4.0 4.0 4.0 4.0 4.0 4.0 4.0 4.0 4.1 4.1 4.1 4.1 4.1 4.1 4.1 4.1 4.1 4.2 4.2 4.2
# [51] 4.3 4.3 4.3 4.3 4.3 4.4 4.4 4.4 4.4 4.5 4.6 4.6 4.6 4.6 4.6 4.7 4.7 4.7 4.7 4.7 4.7 4.8 4.8 4.9 4.9
# [76] 4.9 4.9 5.0 5.0 5.0 5.0 5.0 5.0 5.0 5.0 5.0 5.0 5.0 5.0 5.0 5.0 5.0 5.0 5.0 5.0 5.1 5.1 5.1 5.1 5.1
# [101] 5.1 5.1 5.1 5.1 5.1 5.1 5.2 5.2 5.2 5.2 5.2 5.2 5.2 5.2 5.2 5.3 5.3 5.3 5.3 5.3 5.3 5.3 5.4 5.4 5.4
# [126] 5.4 5.4 5.5 5.5 5.5 5.5 5.5 5.5 5.5 5.5 5.5 5.6 5.6 5.6 5.6 5.7 5.7 5.7 5.7 5.7 5.7 5.8 5.8 5.8 5.8
# [151] 5.8 5.9 5.9 6.0 6.0 6.0 6.0 6.0 6.0 6.0 6.0 6.0 6.0 6.0 6.0 6.0 6.0 6.1 6.1 6.1 6.1 6.1 6.1 6.1 6.1
# [176] 6.2 6.2 6.2 6.2 6.2 6.2 6.2 6.2 6.2 6.2 6.2 6.2 6.2 6.3 6.3 6.3 6.3 6.3 6.3 6.3 6.3 6.3 6.3 6.3 6.3
# [201] 6.3 6.3 6.3 6.3 6.4 6.4 6.4 6.4 6.4 6.4 6.4 6.4 6.4 6.4 6.4 6.5 6.5 6.5 6.5 6.7 6.7 6.7 6.7 6.7 6.7
# [226] 6.7 6.7 6.8 6.8 6.8 6.8 6.9 6.9 6.9 6.9 7.0 7.0 7.0 7.1 7.1 7.1 7.1 7.2 7.2 7.7 7.7 7.7 7.7

stem(dataSet2$price)
# 2 | 33 (2.3 이 두개)
# 2 | 
# 3 | 0000003344
# 3 | 55555888999 (3.5가 5개, 3.8이 3개, 3.9가 3개)
# 4 | 000000000000000111111111222333334444
# 4 | 566666777777889999
# 5 | 00000000000000000011111111111222222222333333344444
# 5 | 55555555566667777778888899
# 6 | 00000000000000111111112222222222222333333333333333344444444444
# 6 | 55557777777788889999
# 7 | 000111122
# 7 | 7777

# age컬럼의 원자값 정보 출력
summary(dataSet$age)

# dataSet3 에 dataSet에서 age가 20이상 69.0이하인 튜플을 복사한다.
dataSet3 <- subset(dataSet, age >= 20.0 & age <= 69.0)
# 결과를 이상치 확인 그래프로 그려본다.
boxplot(dataSet3$age)
```