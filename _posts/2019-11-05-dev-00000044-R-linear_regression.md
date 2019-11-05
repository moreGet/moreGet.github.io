---
date: 2019-11-05 22:00:00
layout: post
title: R-Studio 회귀 분석을 이용한 머신러닝
subtitle: R-Studio의 회귀, 선형회귀, 단순회귀, 다중회귀, 로지스틱 회귀, 다중 공선성 등...
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

## 파일 소스
우클릭 -> 다른이름으로 링크저장 이용해 주세요<br>
<a href="../assets/sources/S20191105.zip" class="btn btn-lg btn-outline">
S20191105.zip
</a>
<br>
<br>
<br>

## 사용 함수 & 키워드
> 회귀 분석, 선형 회귀 분석, 단순 회귀 분석, 다중 회귀 분석, 일반화 선형 모델, 로지스틱 회귀 분석, 다중 공선성 문제 해결 - R통계 분석&머신 러닝 PDF 33쪽부터.<br>

## 사용 예시 소스코드 1
```r
cars
str(cars) # 50행 2열
plot(cars$speed, cars$dist, main = "scatter plot")

model <- lm(dist~speed, cars) # 종속 변수 ~ 독립 변수
model
# Call:
#   lm(formula = dist ~ speed, data = cars)
# 
# Coefficients:
#   (Intercept)        speed  
# -17.579        3.932  
# dist : 3.932, speed : -17.579
abline(coef(model), col="red", lwd=2)
# 대각선 에서 먼 데이터가 많다면 적합하지 않다. 직선과 점 사이의 거리가 가까울수록 적합한 데이터이다.

coef(model)

# 각 독립 변수(x)에 대한 모델의 예측된 y 값을 적합된 값(fitted value)이라고 부른다. 예측치라고 이해하면 된다. 적합된 값은 fitted(mydata) 함수를 사용하면 구할 수 있다.
fitted.values(model)[1:4]

# 모델로부터의 구한 예측 값과 실제 값 사이의 차이를 말한다. (H - y)를 의미한다. 잔차는 residuals() 함수를 이용하면 구할 수 있다. 
residuals(model)[1:4] # 잔차 데이터

cars$dist[1:4]
# 선형 회귀에서는 오차의 제곱의 합이 최소가 되도록 회귀 계수를 정한다. stats 패키지에서 제공하는 deviance( ) 함수를 이용하면 잔차 제곱의 합을 구할 수 있다
deviance(model) # 잔차 제곱의 총합 

# 검정 데이터를 이용하여 회귀 모델의 예측치를 생성하려면 predict() 함수를 사용한다.( stats 패키지 )
# 학습된 모델을 이용하여 예측하기.
testData <- data.frame(speed=3)
predict(model, testData)

# dist = 3.932 * speed - 17.579
3.932 * 3 - 17.579

predict(model, testData, interval = "confidence")
# fit       lwr      upr
# 1 -5.781869 -17.02659 5.462853

predict(model, testData, interval = "prediction")
# fit       lwr      upr
# 1 -5.781869 -38.68565 27.12192

# speed의 최솟값과 최댓값을 확인하고,
summary(cars$speed)

# 이 범위를 이용하여 신뢰 구간 확인해보기.
new_data <- data.frame(speed=seq(4.0, 25.0, 0.2))
predict(model , newdata = new_data, interval = "confidence")
# Prediction -> 오차범위가 한 값을 중심으로 생긴다
# Confidence -> 오차범위가 평균값 (model에서의)을 중심으로 생긴다
# prediction interval이 confidence interval보다 더 넓다
# Prediction interval은 분포가 residual error가 정규분포를 따르고 변화(variance)가 일정하다는 가정하에만 사용한다
# 그러므로 데이터 모델이 분석 함수와 맞는 모델이 아니다. 설득력이 없음.

product <- read.csv("product.csv", header = T)
str(product)
head(product)

colnames(product) <- c("xdata1", "xdata2", "ydata")
head(product)

# 회귀 계수 = 절편 + 기울기  
# Call: 
# lm(formula = ydata ~ xdata, data = myframe) 
#  
# Coefficients: 
# (Intercept)   xdata   
# 0.7789 ← 절편  0.7393 ← 기울기 
model <- lm(formula = ydata ~ xdata1+xdata2, data = product)
model # y = b + a1x1 + a2x2
# install.packages("car")
library(car)

vif_result <- vif(model)
class(vif_result)
vif_result 
# 분산 팽창 요인 둘다 10보다 크진 않다.
# vif 의 값이 10 이상이면 다중 공선성 문제 의심
# xdata1   xdata2 
# 1.331929 1.331929 

# 10보다 큰지 안큰지 검증하기.
vif_result >= 10.0
# xdata1 xdata2 
# FALSE  FALSE
# TRUE 가 나오지 않았으므로 다중 공선성 문제가 의심되지 않는다.

# 다중 공선성 문제는 독립 변수 간의 강한 상관 관계로 인해서 회귀 분석의 결과를 신뢰할 수 없는 현상을 의미한다. 다중 공선성 문제가 의심이 되는 경우 상관 계수를 구하여 변수간의 강한 상관 관계를 명확히 분석하는 것이 좋다. 변수간의 강한 상관 관계를 갖는 독립 변수를 제거하여 해결할 수 있다.  

summary(model)

str(iris)
head(iris)
unique(iris$Species)

# 학습 데이터와 검증 데이터를 7:3으로 분리 한다.
set.seed(1234)
idx <- -sample(1:nrow(iris), 0.7*nrow(iris))
idx

training <- iris[idx, ]
testing <- iris[-idx, ]

dim(training)
dim(testing)

colnames(iris)
# [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width"  "Species"     
# 하나의 종속 변수에 3개의 독립 변수분석
myFormula <- Sepal.Length ~ Sepal.Width + Petal.Length + Petal.Width
model <- lm(formula = myFormula, data = training)
model

vif_result <- vif(model)
vif_result
# Sepal.Width Petal.Length  Petal.Width 
# 1.726987    17.247642    15.263802
table(vif_result >= 10.0)
# FALSE  TRUE 
# 1     2 

# 두개의 컬럼이 상관관계가 너무 높아 회귀분석 의미가 없다.
# TRUE가 두개니 독립변수 를 추리기 위해 상관관계를 분석해 본다.

colnames(iris) # Species 제외하고 상관관계 분석해봄
cor(iris[, -5]) # Petal.Length 와 Width 는 상관관계가 크므로 하나로 줄여준다.

# 다시 검사 해본다.
myFormula <- Sepal.Length ~ Sepal.Width + Petal.Length
model <- lm(formula = myFormula, data = training)
model

vif_result <- vif(model)
vif_result
# Sepal.Width Petal.Length 
# 1.644269     1.644269 
# 다중 공산성 문제 제거.
table(vif_result >= 10.0)
# FALSE 
# 2 
# 다중 공산성 문제 제거.

pred <- predict(model, testing)
pred[1:5]

# pred : 예측값 , testing$Sepal.Length : 본 데이터의 종속값
# 모델 평가하기 선형 회귀 모델은 회귀 방정식에 의해서 숫자로 예측을 하기 때문에 모델 평가는 일반적으로 상관 계수를 이용한다. 즉, cor(예측치, 실제_정답) 명령어를 사용하면 된다. 
cor(pred, testing$Sepal.Length) # 상관계수 검사.
# [1] 0.9187418 예측치와 본데이터 와 91% 의 상관 관계를 가지므로 예측을 성공적으로 했다.
```
## 사용 예시 소스코드 2
```r
df <- read.csv("weather.csv")
str(df)
colnames(df)
unique(df$RainTomorrow)

# 로지스틱 회귀는 확률로 따지는 것이므로 숫자화 해야한다.
df$RainTomorrow <- as.character(df$RainTomorrow)

# 비온다(1) / 안온다(0)
df$RainTomorrow[df$RainTomorrow == "Yes"] <- 1
df$RainTomorrow[df$RainTomorrow == "No"] <- 0

df$RainTomorrow <- as.numeric(df$RainTomorrow)
str(df)
dim(df)

# 학습용과 점검용으로 데이터 분리
train_row <- round(0.7 * nrow(df))
train_row

training <- df[1:train_row, ]
testing <- df[train_row+1:nrow(df), ]
dim(training)
dim(testing)

###################################중요
# 로지스틱 회귀 검사
colnames(df)
model <- glm(RainTomorrow ~ ., data=training, family = "binomial") 
# glm 함수에 family = "binomial"필수
model
summary(model)

# 로지스틱 회귀 모델이면 'response'을 값을 사용하면 된다.
# response 를 넣어줘야 확률 값으로 나옴.
pred <- predict(model, newdata = testing, type = "response")
head(pred)
head(testing$RainTomorrow)
###################################중요

# 예측치 --> 이항형으로 변환
# 시그모이드(Sigmoid) Function  0.5 기준으로 1로 올리거나 0으로 내려버림
result_pred <- ifelse(pred >= 0.5, 1, 0)
myTable <- table(result_pred, testing$RainTomorrow)

# 컨퓨전 메트릭스 만듬.
accuracy <- (90+7)/(90+9+1+7)
accuracy

# predict, prediction : 예측값
# lables : 정답
# install.packages("ROCR")
library(ROCR)
# pr <- prediction( 예측_값, 정답_label )
pr <- prediction(pred, testing$RainTomorrow)
prf <- performance(pr, measure = "tpr", x.measure = "fpr")
plot(prf, main = "ROC Curve")
```
## 사용 예시 소스코드 3
```r
df <- read.csv("pima-indians-diabetes.csv", header = F) 
# View(df)
str(df)
colnames(df)
# [1] "V1" "V2" "V3" "V4" "V5" "V6" "V7" "V8" "V9"

# V9이 종속 변수
dim(df) # [1] 768   9
set.seed(1234) # test 결과 같게 뽑아 내기 위한 seed 설정
idx <- sample(1:nrow(df), 0.7*nrow(df))
train <- df[idx, ]
test <- df[-idx, ]

dim(train)
dim(test)

# 로지스틱 회귀 함수
# glm 함수에 family = "binomial"필수
model <- glm(V9 ~ ., data=train, family = "binomial")
model

# 로지스틱 회귀 모델이면 'response'을 값을 사용하면 된다.
# response 를 넣어줘야 확률 값으로 나옴.
pred <- predict(model, newdata = test, type = "response")
head(pred)
head(test$V9)

result_pred <- ifelse(pred >= 0.5, 1, 0)
mytable <- table(result_pred, test$V9)
mytable[1, 1]
mytable[2, 2]

accuracy <- (mytable[1, 1] + mytable[2, 2]) / sum(mytable)
paste(round(100*accuracy, 2), "%", sep="")

# k겹 교차 검증
library(ROCR)
pr <- prediction(pred, test$V9)
prf <- performance(pr, measure = "tpr", x.measure = "fpr")
plot(prf, main = "ROC Curve")
```
## 사용 예시 소스코드 4
```r
df <- read.table("housing.csv")
df

# 컬럼 변경
colnames(df) <- c('CRIM', 'ZIN', 'INDUS', 'CHAS', 'NOX', 'RM', 'AGE', 'DIS', 'RAD', 'TAX', 'PTRAITO', 'B', 'LSTAT', 'PRICE')

str(df) # 506행 14열 맨 끝열이 집가격
plot(df$CRIM, df$PRICE, main = "scatter plot") # 표 출력 해보기기

# 단계 01 : 학습용과 훈련용 데이터 분리하기
idx <- 10 # 행 뽑기
train_row <- nrow(df) - idx
train <- df[1:train_row, ] # idx만큼 뺀 1부터 행전체

test_row <- nrow(train) 
test <- df[test_row:nrow(df), ] # train 행 빼고 전부

# 전체 데이터 506행 중
dim(train) # 354행
dim(test) # 152행 누적 506행

# 단계 02 : 모델 생성하기
fo <- PRICE ~ . # 종속 변수 ~ 독립 변수 
model <- lm(formula = fo, data = train)
# 단계 03 : 예측과 신뢰 구간
# Prediction -> 오차범위가 한 값을 중심으로 생긴다
# Confidence -> 오차범위가 평균값 (model에서의)을 중심으로 생긴다
# prediction interval이 confidence interval보다 더 넓다
# Prediction interval은 분포가 residual error가 정규분포를 따르고 변화(variance)가 일정하다는 가정하에만 사용한다

model # 회귀 계수 구하기 
# Coefficients: 종속변수에 대해 가장 많이 영향을 주는 컬럼 을 통계함.
#   (Intercept)         CRIM          ZIN        INDUS         CHAS          NOX           RM          AGE          DIS          RAD  
# 35.405559    -0.105122     0.048787     0.021118     2.609734   -17.035131     3.801268     0.002384    -1.509027     0.292501  
# TAX      PTRAITO            B        LSTAT  
# -0.012648    -0.886459     0.009244    -0.542078  


# fitted(model)[1:7] # 종속값 기준 으로 적합 값 예측
head(model$fitted.values ,7)
#        1        2        3        4        5        6        7 
# 30.09921 25.12278 30.71690 28.75579 28.06377 25.39246 22.84080 

# residuals(model)[1:7] # 종속값 - 적합값 한게 잔차
head(model$residuals ,7)
#           1           2           3           4           5           6           7 
# -6.09920644 -3.52277665  3.98309629  4.64421165  8.13623338  3.30754181  0.05919869

fitted(model)[1:7] + residuals(model)[1:7] # 잔차 + 적합값 하면 당연하게 원래 값이랑 같아야 모델링 잘된거임.
#    1    2    3    4    5    6    7 
# 24.0 21.6 34.7 33.4 36.2 28.7 22.9 
head(df$PRICE, 7)
# 적합된 값과 잔차의 합은 실제 데이터의 종속 데이터 값과 같다.

# 단계 03 : 예측과 신뢰 구간
pred <- predict(model, newdata = test, interval = "confidence")
head(pred, 5)
#          fit      lwr      upr
# 496 16.90929 15.24622 18.57235
# 497 14.05287 12.91404 15.19170
# 498 19.25956 18.36332 20.15580
# 499 21.47470 20.48618 22.46323
# 500 18.61183 17.58406 19.63960
# fit : 예상값, lwr ~ upr 사이의 값이 이상적인 값.
# model 을 공부 했으니 test문제를 한번 풀어봐 라는 의미
# 그리고 나온게 위 값.

predict(model, newdata=test, interval='confidence') # interval 변수별로 한번 구해봄
predict(model, newdata=test, interval='prediction')

# 단계 04 : 모델에 대한 평가하기
summary(model) # p-value: < 2.2e-16
2.2e-16 > 0.05 # FALSE
# P value 값이 FALSE 이므로 종속변수와 독립변수 간의 상관관계는 부적절하다.
# 좀 더 적절한 데이터 셋이 필요하다.

# 단계 05 : 예측 값과 정답을 한 번에 보여 주기
# 컴퓨터가 예측한 값과 원래 값을 비교 하여 잘 학습 했는지 알아본다.
test$PRICE
dfr <- data.frame(prediction=pred, real_Price=test$PRICE)
dfr

################################## 문제 2
library(ROCR)

data <- read.csv('wine.csv', header = F)
str(data)
summary(data)

idx <- sample(1:nrow(data), 0.7 * nrow(data))
training <- data[idx, ]
testing <- data[-idx, ]

# 로지스틱 회귀 모델
model <- glm(V13 ~ ., data = training, family = 'binomial')

coef(model)

# 로지스틱 회귀모델 예측치 생성
## model을 이용하여 testing 데이터 검증
pred <- predict(model, newdata = testing, type = 'response')
head(pred)

# recode
pred_res <- ifelse(pred >= 0.5, 1, 0)
head(pred_res)

table(pred_res)
# pred_res
# 0    1 
# 1476  474 

conf <- table(pred_res, testing$V13)
conf
# pred_res    0    1
#       0 1468    8
#       1    6  468

acc <- (conf[1, 1] + conf[2, 2]) / sum(conf)
acc
accPer <- paste(round(100 * acc, 2), '%', sep = '')
# [1] 0.9928205

# ROC Curve를 이용한 모델 평가
pr <- prediction(pred, testing$V13)
prf <- performance(pr, measure = 'tpr', x.measure = 'fpr')
mainText <- paste('ROC Curve', accPer)
plot(prf, main = mainText)
```
