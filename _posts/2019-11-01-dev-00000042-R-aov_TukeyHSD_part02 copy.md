---
date: 2019-11-01 18:11:00
layout: post
title: R-Studio 세 집단 비율검정(prop.test)과 동질성 검사(bartlett.test)를 통한 (aov, kruskal)분산 분석 Part.2
subtitle: R-Studio의 3집단 이상 비율검정prop.test, bartlett.test(동질성검사)를 통한 평균 분산 분석(aov, kruskal)
description: "Current RStudio : == Desktop 1.2.5001(64bit)"
image: ../assets/img/postsimg/R_main_00000003.png
optimized_image: ../assets/img/postsimg/R_op_00000003.png
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
<a href="../assets/sources/S20191101.zip" class="btn btn-lg btn-outline">
S20191101.zip
</a><br>
<br>

## 사용 함수
> prop.test() 세집단 비율 검정<br>
> bartlett.test() 분산분석 전 선행 동질검사<br>
> aov() 동질한 경우<br>
> kruskal.test() 동질하지 않은경우<br>
> TukeyHSD() 마지막 사후 검정<br>

## 로직
![로직](../assets/sources/bartlettLogic.png "분산 검정 로직")

## 사용 예시 소스코드 1
```r
corrTest <- read.csv("cov_corr_test.csv", header = F)
str(corrTest)

myTable <- table(corrTest$V1, corrTest$V2)
myTable

propTable <- prop.table(myTable)
round(addmargins(propTable) , 2)

x <- corrTest$V1
y <- corrTest$V2

plot(x, y, type = 'o', cex = 1.5)

mean_x <- mean(x)
mean_y <- mean(y)

# 상관 계수
cor(x, y)
cov(x, y)

# install.packages("corrplot")
library(corrplot)
myCorr <- cor(corrTest)
myCorr

# 상관관계 차트 corrplot
par(mfrow = c(2, 2))
corrplot(myCorr)
corrplot(myCorr, addCoef.col = "red")
corrplot(myCorr, method = "ellipse", addCoef.col = "red")
corrplot(myCorr, method = "ellipse", addCoef.col = "yellow", type = "upper")

# 
df <- read.csv("시험 점수와 공부 시간1.csv")
result <- cor(df)
result
mode(result)
class(result)
round(result, 3)

library(corrplot)
par(mfrow = c(1, 1))
corrplot(result, method = "ellipse", addCoef.col = "red")

# 
crime <- read.csv("범죄와외국등록일반.csv")
crime

summary(crime)
sd(crime$범죄)
sd(crime$외국인)
sd(crime$내국인)

# cor : 상관계수 함수
corr1 <- cor(crime$범죄, crime$외국인)
corr2 <- cor(crime$범죄, crime$내국인)
corr1
corr2

# 속성 이어 붙히기
mat <- cbind(corr1, corr2)
mat

colnames(mat) <- c("외국인 범죄", "내국인 범죄")
mat

barplot(mat, col = rainbow(length(mat)), ylim = c(0, 1), main = "범죄와 내외국인의 상관 계수")
# cor : 상관계수 함수
cor(crime)

# install.packages("corrgram")
library(corrgram)
# 상관계수 다이어 그램
corrgram(crime)
corrgram(crime, upper.panel = panel.conf)
```

## 사용 예시 소스코드 2
```r
# 로지스틱 회귀(피마 인디언)
# 비만이 유전과 환경 탓이라는 것을 증명해주는 사례가 미국 남서부에 살고 있는 피마 인디언의 사례이다.
# 1950년대까지 비만 인구가 없는 민족이었는데, 지금은 60%가 당뇨, 80%가 비만으로 고통 받고 있다.
# 미국의 기름진 fast 문화를 만나면서부터이다.
# 
# 첨부 파일 : pima-indians-diabetes.csv 
# 파일 이름 : A04.풀어 봅시다_피마 인디언(정답)
# 
# 첨부된 파일에는 컬럼이 없다.
# 다음 정보를 이용하세요.
# 
# 속성	설명	속성	설명
# pregnant	과거 임신 횟수	plasma	포도당 부하 검사 2시간 후 공복 혈당 농도
# pressure	확장기 혈압	thickness	삼두근 피부 주름 두께
# insulin	혈청 인슐린	BMI	체질량 지수
# pedigree	당뇨병 가족력	age	나이
# class	당뇨(1), 당뇨 아님(0)	-	-
#   
# 첨부된 데이터를 이용하여 당뇨에 가장 영향을 미치는 요인은 무엇인지 상관 관계 분석을 수행하세요.
df <- read.csv("pima-indians-diabetes.csv", header = F)
colnames(df) <- c("pregnant", "plasma", "pressure", "thickness", "insulin", "BMI", "pedigree", "age", "class")
df

# 상관관계 차트
corrgram(df, upper.panel = panel.conf)

# welfare.csv 파일을 이용하여
# 문제 01 : birth 컬럼
# 히스토그램을 작성하고, 비대칭도 통계량(왜도/첨도/밀도 분포/정규 분포 곡선)에 대하여 살펴 보세요.
wedf <- read.csv("welfare.csv")
str(wedf)
wedfBirth <- wedf$birth

library(moments)
skwe <- skewness(wedfBirth) #왜도
kurt <- kurtosis(wedfBirth) #첨도
hist(wedfBirth, freq = F)

wedf <- subset(wedf, !is.na(income))
# 밀도 분포 곡선
lines(density(wedfBirth), col="blue")
curve(dnorm(x, mean(wedfBirth), sd(wedfBirth)), col="red", add=T)

# 문제 02 : 카이 제곱 검정 : 성별(gender)과 종교 유무(religion)
dfGender <- wedf$gender
dfReli <- wedf$religion
ta <- table(dfGender, dfReli) # 카이제곱 검정을 위해 교차 테이블 작성
ct <- chisq.test(ta) # 교차 테이블로 검정
# 귀무가설 : 성별에 따른 종교선택 성향은 존재한다.
# 귀무가설을 기각한다 성별에 따른 종교선택 성향은 존재하지 않는다.
ct$p.value > 0.05

# 문제 03 : 두 집단 평균 검정 : 성별(gender)과 월급(income)
manin <- subset(wedf, gender==1, c(gender, income))
fenin <- subset(wedf, gender==2, c(gender, income))

mean(manin$income, na.rm = T)
mean(fenin$income, na.rm = T)
vt <- var.test(manin$income, fenin$income)
# 귀무가설 : 성별에 따른 월급의 금액은 차이가 없다.
# 귀무 가설을 기각한다. 차이가 존재 한다.
2.2e-16 > 0.05

# 문제 04 : 
# birth 컬럼을 이용하여 age 컬럼을 만드세요.
wedf$age <- 2019 - wedf$birth
wedf$age

# age 컬럼을 3그룹으로 나누세요.
range(wedf$age)
yb <- subset(wedf, age<=30, c(age, income))
mb <- subset(wedf, (age>30&age<=60), c(age, income))
ob <- subset(wedf, age>60, c(age, income))
yb$age <- "yb"
mb$age <- "mb"
ob$age <- "ob"

rbinddf <- rbind(yb, mb, ob)
range(rbinddf$age)
# rbinddf$group[rbinddf$age <= 30] = "yb"
# rbinddf$group[rbinddf$age > 30 & rbinddf$age <= 60] = "mb"
# rbinddf$group[rbinddf$age > 60] = "ob"
unique(rbinddf$group)
# 세 집단 평균 검정 : 나이(age)과 월급(income)
# aov(score ~ method2, data = df02)
# 종속 : 독립값에 관련되어 있는 것, 독립 : 뚜렷한 구분 원자값을 가진 컬럼
bartlett.test(income ~ age, data = rbinddf)
# 동질성 검정이란 두 집단의 분포가 동일한 분포를 갖는 모집단에서 추출된 것인지를 검정하는 방법이다.
2.2e-16 > 0.05 # 동질성

ao <- aov(income ~ age, data = rbinddf)
summary(ao)
# 귀무가설 : 나이에 따른 월급의 차이는 없다.
# 귀무 가설을 기각한다. 나이에 따른 월급의 차이는 생긴다.
2e-16 > 0.05

# 문제 05 : 
# stock_market.csv 파일을 이용하여 공분산 및 상관 계수를 구하여라.
# cor() 함수와 cov() 함수를 사용하지 않고 순수 R 코드를 이용하여 다시 풀어 보세요.
# 공분산 : 0.06384
# 상관 계수 : 0.9870298
stock_df <- read.csv("stock_market.csv", header = F)
stock_df

x <- stock_df$V1
y <- stock_df$V2

# 상관 계수
cor(x, y)
# 공분산
cov(x, y)
```