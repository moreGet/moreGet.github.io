---
date: 2019-11-07 22:00:00
layout: post
title: R-Studio 인공신경망 을 이용해 데이터분석 하기.
subtitle: R-Studio의 이항차트(ctree), 인공신경망(neuralnet)
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
<a href="../assets/sources/S20191108.zip" class="btn btn-lg btn-outline">
S20191108.zip
</a>
<br>
<br>
<br>

## 소스 코드
```r
# install.packages('party')
library(party)
str(airquality)
# 연속형은 boxplot으로 보여주는게 좋다.
colnames(airquality)

myformula <- Temp ~ Ozone + Solar.R + Wind
model <- ctree(formula = myformula, data = airquality)
model
# -----------------output----------------- 
# Conditional inference tree with 5 terminal nodes
# 
# Response:  Temp 
# Inputs:  Ozone, Solar.R, Wind 
# Number of observations:  153 
# 
# 1) Ozone <= 37; criterion = 1, statistic = 56.086  내려갈수록 숫자가 커지는게 작을수록 중요도 큼
#  2) Wind <= 15.5; criterion = 0.993, statistic = 9.387
#   3) Ozone <= 19; criterion = 0.964, statistic = 6.299
#     4)*  weights = 29 
#   3) Ozone > 19
#     5)*  weights = 69 
#  2) Wind > 15.5
#     6)*  weights = 7 
# 1) Ozone > 37
#  7) Ozone <= 65; criterion = 0.971, statistic = 6.691
#   8)*  weights = 22 
#  7) Ozone > 65
#   9)*  weights = 26 

plot(model)
```

## 소스 코드 2
```r
mush <- read.csv('통계_분석_src_02/27.의사 결정 트리01/mushrooms.csv', header = T)
head(mush)
dim(mush)
colnames(mush)

# sampling 7:3
idx <- sample(1:nrow(mush), 0.7 * nrow(mush))
idx

training <- mush[idx, ] # 학습용
testing <- mush[-idx, ] # 테스트용

dim(mush)
dim(training)
dim(testing)

library(party)
myformula <- type ~ .
model <- ctree(formula = myformula, data = training)
model

plot(model)
plot(model, type = 'simple')

# prediction
pred <- predict(model, testing)
pred # 예측값

testing$type # 정답(labels)

my_table <- table(pred, testing$type)
my_table

enu <- my_table[1, 1] + my_table[2, 2]
deno <- sum(my_table)

accuracy <- enu / deno
round(100 * accuracy, 3)

# caret 패키지 개인적으로 써보기

#################################################

library(devtools)
library(nnet)

source_url('https://gist.githubusercontent.com/fawda123/7471137/raw/466c1474d0a505ff044412703516c34f1a4684a5/nnet_plot_update.r')

data <- data.frame(x1 = c(1:6), x2 = c(6:1))
data

some <- read.csv('통계_분석_src_02/28.인공 신경망/somedata.csv')
str(some)

set.seed(1234)
model <- nnet(y ~ ., some, size = 1)
model
# a 2-1-1 network with 5 weights
# inputs: x1 x2 
# output(s): y 
# options were - entropy fitting

plot.nnet(model)

attributes(model)

model$n

model$fitted.values
model$fitted.values >= 0.5

pred <- predict(model, some, type = 'class')
pred
# input data를 attributes라고 한다 머신러닝에서
# 정답값을 가지고 있는 컬럼(데이터)를 class라고 한다.
```

## 소스 코드 3
```r
library(devtools)
library(nnet)

source_url('https://gist.githubusercontent.com/fawda123/7471137/raw/466c1474d0a505ff044412703516c34f1a4684a5/nnet_plot_update.r')

str(iris)
head(iris$Species)
species.ind <- class.ind(iris$Species)
iris <- cbind(iris, species.ind)
head(iris)

# one hot encoding: 분류 항목 중에서 1개만 on 시킨다
species.ind <- class.ind(iris$Species)
iris <- cbind(iris, species.ind)
head(iris)

# 훈련: 테스트 2:1
idx <- sample(1:nrow(iris), 2/3 * nrow(iris))
training <- iris[idx, ]
testing <- iris[-idx, ]

dim(training)
dim(testing)
colnames(training)

##### training data #####
x_data <- training[, c(1:4)]
y_data <- training[, c(6:8)]

# 다항분류를 하려면 softmax에 T를 줘
model <- nnet(x = x_data, y = y_data, size = 10, softmax = T)
model

plot.nnet(model)

prediction <- predict(model, x_data, type = 'class')
head(prediction)

table(prediction, training$Species)

##### actual data #####
x_test <- testing[, c(1:4)]
prediction <- predict(model, x_test, type = 'class')
head(prediction)

table(prediction, testing$Species)

#############################################################

# product_sales.csv 파일을 읽어서 visit_count, buy_count, avg_price 칼럼을
# tot_price 칼럼을 종속 변수로 하여 다음과 같이 분류 모델를 생성하시오.
#  
# 변수 설명  
# tot_price : 총구매액
# visit_count : 방문 횟수, buy_count : 판매횟수, avg_price : 1회평균구매액
# 
# 학습 데이터와 검정 데이터를 7:3 비율로 셈플링하시오.
# tot_price를 종속 변수로 하는 포뮬러를 작성하고 
# ctree() 함수를 이용하여 분류 모델을 생성해 보세요.
# 
# 의사 결정 트리를 시각화하고, 중요한 변수에 대한 해석을 수행해 보세요.

library(party)
prd <- read.csv('통계_분석_src_02/27.의사 결정 트리01/product_sales.csv')
colnames(prd)

idx <- sample(1:nrow(prd), 0.7 * nrow(prd))
idx

training <- prd[idx, ]
testing <- prd[-idx, ]

my_formula <- tot_price ~ .

model <- ctree(formula = my_formula, data = prd)
model

plot(model)

# 총 구매액(tot_price)에 가장 큰 영향을 주는 변수는 1회 평균 구매액(avg_price)이다.
# 1회 평균 구매액이 6.0이상인 경우, 총 구매액(tot_price)는 평균 7.5 정도의 값을 보이고 있다.
# 방문 횟수(visit_count)는 총 구매액에 큰 영향을 주지 않는 것으로 보인다.

sample_data <- subset(prd, (avg_price > 3.3 & avg_price <= 4.2))
sort(sample_data$tot_price)

# summary(sample_data$tot_price)
mybox <- boxplot(sample_data$tot_price)
mybox

# 회기문제는 모델이 잘됐다 못됐다를 상관계수로 나타낸다. 두개의 데이터를 가지고.

pred <- predict(model, testing)
pred

testing$tot_price # 정답(labels)


# --------------- 이건 안해도 됨-------------------
# accuracy (연속형 데이터라서 회기로 접근해야한다. 상관계수로)
# my_table <- table(pred, testing$tot_price)
# my_table
# 
# enu <- my_table[1, 1] + my_table[2, 2]
# deno <- sum(my_table)
# 
# accuracy <- enu / deno
# round(100 * accuracy, 3)
```

## 소스 코드 4
```r

# install.packages('neuralnet')
library(neuralnet)


data <- read.csv('통계_분석_src_02/28.인공 신경망/some.csv', header = T)

str(data)
head(data)
dim(data)

# scale() : 표준화 변환
# min-max 정규화 : 제일 큰 값을 1, 작은값을 0으로 만드는 정규화
# 10 20 30

normalize <- function(x) {
  return((x - min(x)) / (max(x) - min(x)))
}

a <- data.frame(data = c(123,41,23,45,245,3,4))
lapply(a, normalize)

# norm : normalization
dataNorm <- as.data.frame(lapply(data, normalize))
dataNorm$y <- dataNorm$y1

dataNorm <- dataNorm[c('x', 'y')]
head(dataNorm)
summary(dataNorm)

# 3:1로 데이터 분리

trainRow <- round(0.75 * nrow(data))
training <- dataNorm[1:trainRow,]
testing <- dataNorm[(trainRow + 1):nrow(data),]

model <- neuralnet(formula = y ~ ., training, hidden = 500)
model

plot(model)

head(testing)

model_result <- compute(model, testing[1:1])
attributes(model_result)

prediction <- model_result$net.result
prediction # 예측값

df <- data.frame(predict = prediction, real = testing$y)
df
# 오차를 최소화하는 적합치를 찾는게 머신러닝이다.

x <- seq(1:length(prediction))
plot(x = x, y = prediction, type = 'n', xlab = '', ylab = 'value', ylim = c(0, 1))
points(x, prediction, pch = 4, col = 'red') # 예측값
points(x, testing$y, pch = 4, col = 'blue') # 정답

cor(prediction, testing$y) #  이만큼의 상관성이 있다.

########################################################################################

library(neuralnet)

concrete <- read.csv('통계_분석_src_02/28.인공 신경망/concrete.csv', header = T)
dim(concrete)
head(concrete)
colnames(concrete)
str(concrete)


# 정규화
normalize <- function(x){
  return((x - min(x)) / (max(x) - min(x)))
}
concrete_norm <- as.data.frame(lapply(concrete, normalize))
head(concrete_norm)
summary(concrete_norm)

# 샘플링
idx <- sample(1:nrow(concrete), 0.75 * nrow(concrete))
training <- concrete_norm[idx, ]
testing <- concrete_norm[-idx, ]

myformula <- strength ~ .
model <- neuralnet(formula = myformula, data = training)
plot(model)

testing_x <- testing[1:8]
model_result <- compute(model, testing_x)

prediction <- model_result$net.result

# 회귀는 수치 값 예상이므로 상관 계수로 평가해야 한다.(model01)
cor(prediction, testing$strength)

model02 <- neuralnet(formula = myformula, data = training, hidden = rep(5, 3)) # 은닉층이다 히든은
plot(model02)

model_result02 <- compute(model02, testing_x)

prediction02 <- model_result02$net.result


# 회귀는 수치값 예상이므로 상관계쑤로 평가해야한다.(model02)
cor(prediction02, testing$strength)


# 데이터를보고 분류문제, 회기문제, 카이검정, 등등으로 어떻게 분석할것인지 결정을 해야한다.
```