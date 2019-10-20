---
date: 2019-10-20 20:30:00
layout: post
title: R-Studio json과 cloudword2
subtitle: R-Studio로 json을 전처리하여 cloudword 그리기
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

# R-Studio 도 함수를 만들 수 있다.
> 차트를 그리기에 앞서 차트를 그릴 인자 값의 자료형이 행렬 혹은 벡터 여야 한다.<br>
> 본래 차트 함수에 legend 인자가 있는경우 legend() 함수의 단독 함수 사용이 대부분 가능하다.<br>
<!-- > 패키지 로드순서는 plyr -> dplyr 순서로 로드 해주시기 바랍니다. -->

# 사용된 함수

| Use | Descriptions |
|:----------:|:-----------|
| `install.packages("KoNLP")` | library(KoNLP) 자연어 처리함수 |
| `install.packages("jsonlite")` | library(jsonlite) json 파일 다루기 |
<!-- | `install.packages("jsonlite")` | library(jsonlite) json 파일 다루기 | -->

## 파일 소스
우클릭 -> 다른이름으로 링크저장 이용해 주세요<br>
<a href="../assets/sources/member.json" class="btn btn-lg btn-outline">
member.json
</a><br>
<a href="../assets/sources/김주혁_naver_news.json" class="btn btn-lg btn-outline">
김주혁_naver_news.json
</a><br> 
<br>

## 사용 예시 소스코드

```r
# json 제어하기
# install.packages("jsonlite")
# library(jsonlite)

myFile <- 'member.json'
myFrame <- fromJSON(myFile)
mode(myFrame)
class(myFrame)
myFrame

total_df <- data.frame()
for(idx in 1:nrow(myFrame)) {
  name <- myFrame$name[idx]
  occupation <- myFrame$occupation[idx]
  address <- myFrame$information$address[idx]
  height <- myFrame$information$info$height[idx]
  weight <- myFrame$information$info$weight[idx]
  blood <- myFrame$information$info$blood[idx]
  
  # cat("이름 : ", name)
  # cat("\n")
  # 
  # cat("키 : ", height)
  # cat("\n")
  # 
  # cat("혈액형 : ", blood)
  # cat("\n")
  df <- data.frame(name=name, occupation=occupation, address=address, height=height, weight=weight, blood=blood)
  total_df <- rbind(total_df, df)
}
total_df

write.csv(myFrame, "affter.csv")
  
chart <- unlist(total_df$height)
barplot(chart, ylim = c(0, 200))

###############################################################################################################
# json 파싱하기.
china_file <- "중국_방문객_정보.json"
china_visit <- fromJSON(china_file)
china_visit

# 컬럼 뽑아오기
newdata <- china_visit[c("yyyymm", "visit_cnt")]
newdata

# 정규표현식 사용을 위한 메모리 로딩
library(stringr)
newdata$mm <- str_sub(newdata$yyyymm, 5, 6)
# 컬럼 이름 바꾸기
colnames(newdata) <- c("년월", "방문자수", "월")
# 컬럼 원자값에 월 추가하기
newdata$월 <- str_c(newdata$월, "월")
str(newdata)

# 차트그리기
mycolor <- rainbow(nrow(newdata))
xlabel <- newdata$월
# 차트그리기
# e 패턴을 정수형을 풀어서 써줌 1*e5 = 100000 이런식
options(scipen = 25)

# barplot 그리기
barplot(height = newdata$방문자수, names.arg=xlabel ,col=mycolor, main = "2017년도 중국인 방문수", ylim=c(0, 600000), las=1, cex.names=0.9)

# 다음 json 파일 파싱하고 write 하기.
myFile <- "somedata.json"
myFrame <- fromJSON(myFile)
str(myFrame)
write.csv(myFrame, "humans.csv", quote = F, row.names = F)

jsonData <- toJSON(myFrame)
jsonData
str(jsonData)

# rm(list = ls()) # plots 비워줌
rm(list = ls())
# dev.off() # val 비워줌
dev.off()

#####################################################################################

# KoNLP : 자연어 처리 패키지
# install.packages("KoNLP")
library(KoNLP)
library(jsonlite)

myFile <- "김주혁_naver_news.json"
myFrame <- fromJSON(myFile)
str(myFrame)
# nrow(myFrame)
# myFrame$description[1]

# 새로운 벡터 하나 만들어주고.
newData <- c()
# for문을 돌려야 하니 for문을 몇번 돌릴지. 추출해야함
# myFrame의 description 벡터를 뽑을것이니.
# 벡터의 크기만큼 for문을 돌려야 하므로 msgLen에 length()함수로 행수 뽑음.
msgLen <- length(myFrame$description)

# for문을 돌림.
for (idx in 1:msgLen) {
    newData <- c(newData, myFrame$description[idx])
}
newData

# 명사를 다빼옴.
someData <- sapply(newData, extractNoun, USE.NAMES = F)
someData

# list를 풀어버림
myCheckData <- unlist(someData)
# 벡터 총 갯수
length(myCheckData)
myCheckData

# 함수 를 필터에 지정하여 단어가 2이상 6이하 단어 추출 해줌.
# nchar(x) = myCheckData 의 값을 넣고 단어의 길이를 추출함. 그리고 오른쪽 조건에의하여
# true이면 뽑고 false이면 넘김
myCheckData <- Filter(function(x) { nchar(x) >= 2 }, myCheckData)
myCheckData <- Filter(function(x) { nchar(x) <= 6 }, myCheckData)
# 얼마나 추출 했는지.
length(myCheckData)
# 데이터 확인하기
myCheckData

# table로 만들어서 요약 해봄.
ta <- table(myCheckData)

# sort함수로 정렬 하기.
result <- sort(ta, decreasing = T)
result
# dataLen <- 20 # 워드 클라우드할 갯수
result <- head(result, length(result))

# 워드 클라우드 하기
# install.packages("wordcloud2")
# library(wordcloud2)
# remove.packages("wordcloud2")

# library(devtools)
# devtools::install_github("lchiffon/wordcloud2")

# 워드 클라우드2
wordcloud2(result, figPath = "a.png",size = 0.1, fontFamily = "Segoe UI", color = "random-dark", shuffle = T, rotateRatio = 0)
```