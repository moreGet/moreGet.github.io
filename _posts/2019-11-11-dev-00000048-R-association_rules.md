---
date: 2019-11-07 22:00:00
layout: post
title: R-Studio 연관규칙 의 시각화
subtitle: R-Studio의 arulesViz, inspect, transactions, itemFrequency, itemFrequencyPlot
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
<a href="../assets/sources/S20191111.zip" class="btn btn-lg btn-outline">
S20191111.zip
</a>
<br>
<br>
<br>

## 메인 사용 함수
> subset 조건부 필터 <br>
> inspect 트랜잭션 객체에 대한 연관 규칙의 내용을 확인하기 위한 함수 <br>
> itemFrequency 객체 빈도수 <br>
> itemFrequencyPlot 위 아이템의 시각화 함수 <br>
> image  바둑판처럼 격자 형식으로 데이터를 시각적으로 보여준다. 행은 트랜잭션, 열은 항목을 보여 준다. <br>
> dist 유클라디언 거리계산 함수 <br>
> hclust 클러스터를 만들기 위해서는 hclust() 함수를 사용하면 된다. 계층적 군집 분석을 위하여 클러스트링을 수행하는 함수이다. <br>
> read.transactions 트랜잭션 객체를 생성해주는 함수이다. (희소행렬 생성)<br>

## 소스 코드
```r
# install.packages("arules")
library(arules)

tran <- read.transactions("tran.txt", format = "basket", sep=",")
tran

inspect(tran)
image(tran)

# 지지도 = supp, 신뢰도 = conf
rule <- apriori(tran, parameter = list(supp=0.3, conf=0.1))

# 지지도 = supp, 신뢰도 = conf
rule <- apriori(tran, parameter = list(supp=0.1, conf=0.1))

sink("tran_result.txt")
inspect(rule)
sink()

# install.packages("arulesViz")
library(arulesViz)

plot(rule, method="grouped")
plot(rule, method="scatterplot")

# arules
library(arules)
df <- read.transactions("groceries.csv", sep=",")
dim(df)
summary(df)

inspect(df[1:5])

itemFrequency(df[, 1:3])
itemFrequency(df)

# plot 출력
class(df)
itemFrequencyPlot(df, support = 0.1)
itemFrequencyPlot(df, topN=20)

# 이미지
image(sample(df, 100))
dfRules <- apriori(df, parameter = list(support=0.006, confidence = 0.25, minlen = 2))
dfRules

summary(dfRules)
inspect(dfRules[1:3])

# 향상도 순으로 정렬
inspect(sort(dfRules, by="lift")[1:5])
# berry 아이템을 포함하는 ....
berryRules <- subset(dfRules, items %in% "berries")
inspect(berryRules)

# 파일로 저장
write(dfRules, file = "dfRules.csv", sep=",", quote=T, row.names=F)
# dfRules 데이터 프레임으로 만들기
dfRules_df <- as(dfRules, "data.frame")
str(dfRules_df)
```

## 소스 코드 2
```r
# install.packages('arules')
library(arules)

# install.packages('arulesViz')
library(arulesViz)

# 단계 : 실습 데이터 읽기
tran <- read.transactions('mybasket.csv', format='basket', sep=',')

tran # 행과 컬럼 구조를 알려 준다.

summary(tran) # 데이터들의 기술 분석 결과를 보여 준다.
# clothes가 386건, snack이 373건으로 가장 많이 거래가 되었다.

# transactions as itemMatrix in sparse format with
#  786 rows (elements/itemsets/transactions) and
#  10 columns (items) and a density of 0.3049618 
# 
# most frequent items:
# clothes   snack    deco  bakery  frozen (Other) 
#     386     373     358     337     318     625 
# 
# element (itemset/transaction) length distribution:
# sizes
#   1   2   3   4   5   6   7   8   9 
# 209 166 147 101  57  48  33  18   7 
# 
#    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
#    1.00    1.00    3.00    3.05    4.00    9.00 
# 
# includes extended item information - examples:
#    labels
# 1 alcohol
# 2  bakery
# 3 clothes

# 개괄적인 현황을 플로팅을 통하여 그림으로 보여 준다.
image( head(tran) )
image(tran)
# savePlot('개괄적인 현황.png', type='png')

# 단계 : 알고리즘 적용
myframe <- as(tran, 'data.frame')
myframe

# apriori 알고리즘을 이용하여 rules 객체에 저장
rules <- apriori(tran, parameter=list(supp=0.2, conf=0.1))
sink('mybasket.csv.txt')
inspect( rules )
sink()

# 제품은 희소 행렬에 알파벳 순으로 정렬이 되어 있다.
# alcohol은 39.57%, bakery는 42.87% 정도 거래 되었다.
itemFrequency(tran)

itemFrequencyPlot(tran, support = 0.1)

itemFrequencyPlot(tran, topN = 10, ylim=c(0, 0.5), col=rainbow(10))

# 정렬
inspect(sort(rules, by = "lift", decreasing = FALSE))

inspect(sort(rules, by = "lift", decreasing = TRUE))

inspect(sort(rules, by = c("lift", "support"), decreasing = c(TRUE, FALSE)))

# 안되는 듯..
inspect(sort(rules, by = c("lift", "support"), decreasing = c(TRUE, TRUE)))

# 슬라이싱
inspect(sort(rules, by = "lift")[1:5])

# 필터링
subset_rules <- subset(rules, items %in% "frozen")

inspect(rules) # 원본 갯수 확인
inspect(subset_rules) # 필터링된 것 갯수 확인

filter01 <- subset(rules, rhs %in% 'frozen') 
filter01 
inspect(filter01) # 규칙 확인
plot(filter01, method="graph")
plot(filter01, method="scatterplot") 

# %pin% : ~ 가 포함된 규칙만....
filter02 <- subset(rules, rhs %pin% 'o') # 알파벳 o가 포함되어 있는 ....
filter02 # set of 10 rules
inspect(filter02) # 규칙 확인

filter03 <- subset(rules, lhs %in% c('snack','frozen')) 
filter03 # set of 4  rules
inspect(filter03) # 규칙 확인
plot(filter03, method="graph") 

# 지지도와 신뢰도, 향상도에 대한 산포도 그래프
plot(rules, method='scatterplot')
# savePlot('패키지 추천 산포도 그래프.png', type='png')

# 가로축(조건 : X 아이템)과 세로축(결과 : Y 아이템)
plot(rules, method='grouped')
# savePlot('패키지 추천 매트릭스 그래프.png', type='png')

# 각 규칙별로 어떠한 아이템들이 연관되어 있는가를 보여 주는 네트워크 그래프
plot(rules, method='graph', control=list(types='items'))
# savePlot('패키지 추천 네트워크 그래프.png', type='png')

# 팝업창을 띄워 준다.
plot(rules, method='graph', interactive= TRUE, control = list(type='items'))
# savePlot('패키지 추천 네트워크 팝업 그래프.png', type='png')

# CSV 파일에 규칙 쓰기
write(rules, file = "myrules.basket.csv",
      sep = ",", quote = TRUE, row.names = FALSE)

# 규칙들을 데이터 프레임으로 변환
rules_df <- as(rules, "data.frame")
str(rules_df)
rules_df


# 단계 : 연관 규칙 해석
# 단일 item에서 발생 확률이 가장 높은 아이템은 ?
# 단일 item에서 발생 확률이 가장 낮은 아이템은 ?
# 가장 발생 가능성이 높은 <2개 상품 간>의 연관 규칙?
# 가장 발생 가능성이 높은 <2개 상품 이상에서> <3개의 상품으로>의 연관 규칙?

vec <- c(2, 5, 2, 4, 4, 5, 5, 6, 4, 1)
mat <- matrix(vec, nrow=5, byrow=T)
rownames(mat) <- c("짜장면", "짬뽕", "라면", "우동", "돈까스")
colnames(mat) <- c("속성1", "속성2")
mat

library(cluster)

distance <- dist(mat)
distance

distance <- dist(mat, diag=T)
distance

hc <- hclust(distance, method = "single")
plot(hc)

mds <- cmdscale(distance)
plot(mds, type='n')
text(mds, rownames(mat))

# 30.군집 분석
cmdscale <- read.csv("cmdscale.csv", header = T, stringsAsFactors = F)
cmdscale

cmd_matrix <- t(as.matrix(cmdscale)) %*% as.matrix(cmdscale)
cmd_matrix

distance <- dist(cmd_matrix)
distance

cluster <- hclust(distance)
cluster

plot(cluster)

# classical mult dimensional scaling
mds <- cmdscale(distance)
plot(mds, type='n')
text(mds, rownames(cmd_matrix))
```