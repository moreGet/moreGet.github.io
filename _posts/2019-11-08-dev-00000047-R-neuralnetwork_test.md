---
date: 2019-11-07 22:00:00
layout: post
title: R-Studio 인공신경망 을 이용해 데이터분석 하기.
subtitle: R-Studio의 로지스틱회귀(glm), 샘플링(samlple), 예측(predict), 이항분류
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

## 메인 사용 함수
> nnet 신경망 구축 <br>
> predict 예측 하기 <br>
> sample 샘플링 <br>
> glm 로지스틱 회귀 <br>
> performance ROC그리기 <br>

## 소스 코드
```r
# 패키지 설치 : nnet
# 분류 모델 : 이항 분류
# 훈련용 데이터 : department_store.csv
# 테스트용 데이터 : department_testing.csv
# 
# 강남 백화점 적립 카드를 만들기 위한 고객들의 정보 파일이 있다.
# 이 파일을 이용하여 백화점 카드를 사용하는 고객 들의 예측 모델을 nnet 패키지를 이용하여 신경망으로 구현해 보세요.
# 이 모델에 의하여 구매 지수가 다음과 같은 고객(파일 : department_testing.csv)이 있다고 할 때, 이 사람은 카드를 만들 확률이 얼마인지 점검해 보세요.
library(devtools)
library(nnet)
source_url('https://gist.githubusercontent.com/fawda123/7471137/raw/466c1474d0a505ff044412703516c34f1a4684a5/nnet_plot_update.r')

df_tr <- read.csv("department_store.csv")
unique(df_tr$card)

# softmax 를 하기 위해 one hot encoding 함
type.ind <- class.ind(df_tr$card)
df_tr <- cbind(df_tr, type.ind)

x <- df_tr[1:6]
y <- df_tr[8:9]
model01 <- nnet(x = x, y = y, size = 10)

# 신경망 구현
plot.nnet(model01)

df_te <- read.csv("department_testing.csv")

# 카드 만들 확률 인 사람
prediction <- predict(model01, df_te, type = "raw")

#######################################################################
# 패키지
# library(devtools)
# library(nnet)
# 
# 데이터 : rent_car.csv
# 렌트여부, 성별, 직업, 지역, 재산
# 
# 첨부된 데이터를 이용하여 고객이 렌트 카 서비스를 잘 이용하는 지 살펴 보도록 한다.
# 이용 고객에 대한 예측 모델을 로지 스틱 회귀로 분석을 수행하세요.
# 
# 해당 데이터를 이용하여 정확도가 어느 정도 되는지 확인해보세요.
library(devtools)
library(nnet)
df02 <- read.csv("rent_car.csv")
tr <- df02
te <- df02

# 로지스틱 회귀 검사
fo <- 렌트여부 ~ .
model <- glm(formula = fo, data=tr, family = "binomial") 
# glm 함수에 family = "binomial"필수
model
summary(model)

# 로지스틱 회귀 모델이면 'response'을 값을 사용하면 된다.
# response 를 넣어줘야 확률 값으로 나옴.
pred <- predict(model, newdata = te, type = "response")
head(pred)

######################################################################
# 데이터 셋 : ThoraricSurgery.csv
# 마지막 컬럼이 폐암 여부를 저장하고 있는 열이다.
# 데이터 셋을 읽어 들이시오.
# 훈련:테스트 = 7:3으로 데이터 셋을 나누세요.
# 로지스틱 회귀 모델을 생성하세요.
# 로지 스틱 회귀 모델에 대하여 예측 값을 구해 보세요.
# confusion matrix를 이용하여 정확도를 구해 보세요.
# ROC 곡선을 그려 보세요.
df03 <- read.csv("ThoraricSurgery.csv", header = F)
dim(df03)

# 마지막 컬럼이 폐암 여부를 저장하고 있는 열이다.
lastCol <- ncol(df03)
idx <- sample(1:nrow(df03), 0.7 * nrow(df03))

# 7:3 데이터 샘플링
tr <- df03[idx, ]
te <- df03[-idx, ]

# 로지스틱 회귀 검사
colnames(df03)
fo <- V18 ~ .
model <- glm(formula = fo, data=tr, family = "binomial") 
# glm 함수에 family = "binomial"필수
model
summary(model)

# 예측 확률값 구하기
pred <- predict(model, newdata = te, type = "response")
head(pred)

# confusion matrix 정확도 구하기
pr_result <- ifelse(pred>=0.5, 1, 0)
con_ta <- table(pr_result, te$V18)
dim(con_ta)

acc <- round((con_ta[1,1] + con_ta[2,2]) / sum(con_ta) * 100, 2)
acc

# predict, prediction : 예측값
# lables : 정답
# install.packages("ROCR")
library(ROCR)
# pr <- prediction( 예측_값, 정답_label )
pr <- prediction(pred, te$V18)
prf <- performance(pr, measure = "tpr", x.measure = "fpr")
plot(prf, main = "ROC Curve")
```