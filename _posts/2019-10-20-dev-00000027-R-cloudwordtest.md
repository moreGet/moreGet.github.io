---
date: 2019-10-20 20:30:00
layout: post
title: R-Studio json과 cloudword2를 이용한 예제
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

# WordCloud2 를 완전히 사용하기 위한 선행 작업.
> install.packages(c("htmlwidgets", "htmltools", "yaml", "base64enc", "KoNLP", "jsonlite", "dplyr", "reshape2"))<br>
> install.packages("devtools")<br>
> library(devtools)<br>
> devtools::install_github("lchiffon/wordcloud2")<br>
>
> library(rJava)<br>
> library(KoNLP)<br>
> library(jsonlite)<br>
> library(stringr)<br>
> library(dplyr)<br>
> library(reshape2)<br>
> library(devtools)<br>
> library(wordcloud2)<br>
> library(htmltools)<br>
> library(yaml)<br>
> library(base64enc)<br>
> library(htmlwidgets)<br>

# 사용된 함수

<!-- | Use | Descriptions |
|:----------:|:-----------|
| `install.packages("KoNLP")` | library(KoNLP) 자연어 처리함수 |
| `install.packages("jsonlite")` | library(jsonlite) json 파일 다루기 | -->
<!-- | `install.packages("jsonlite")` | library(jsonlite) json 파일 다루기 | -->

## 파일 소스
우클릭 -> 다른이름으로 링크저장 이용해 주세요<br>
<a href="../assets/sources/jumsu.json" class="btn btn-lg btn-outline">
jumsu.json
</a><br>
<a href="../assets/sources/미국(275)_해외방문객정보(2016 ~ 2017).json" class="btn btn-lg btn-outline">
미국(275)_해외방문객정보(2016 ~ 2017).json
</a><br> 
<a href="../assets/sources/pokedex-korean.json" class="btn btn-lg btn-outline">
pokedex-korean.json
</a><br> 
<br>

## 사용 예시 소스코드

```r
# jumsu.json 파일을 이용하여 학생들의 국영수 각각 평균 점수를 구하시오.
# 3 과목의 평균을 이용하여 Pie 그래프를 그리시오.
filePath <- "jumsu.json"
jumsuData <- fromJSON(filePath)
str(jumsuData)

avgScore <- data.frame(avgKor=mean(as.numeric(jumsuData$kor)), avgEng=mean(as.numeric(jumsuData$eng)), avgMath=mean(as.numeric(jumsuData$math)))
str(avgScore)

pieLables <- colnames(jumsuData[2:4])
pie_3d <- pie3D(x=as.matrix(avgScore), radius = 1, main = "시카,호동 의 과목별 평균 점수")
pie3D.labels(pie_3d, labels = pieLables, labelcex = 1.7, height = 0.1)

# 미국(275)_해외방문객정보(2016 ~ 2017).json 파일을 이용하여 
# 매년(2016년과 2017년) 총 방문객 수를 이용하여 막대 그래프 그리기
filePath01 <- "미국(275)_해외방문객정보(2016 ~ 2017).json"
visit_USA <- fromJSON(filePath01)
str(visit_USA)

yearOfVisiter <- visit_USA %>% filter(str_sub(yyyymm, 1, 4)=="2016"|str_sub(yyyymm, 1, 4)=="2017") %>% select(yyyymm, visit_cnt)

# 컬럼 빼기
c2016 <- yearOfVisiter[str_sub(yearOfVisiter$yyyymm, 1, 4)=="2016", ]
c2017 <- yearOfVisiter[str_sub(yearOfVisiter$yyyymm, 1, 4)=="2017", ]
cMonth <- paste(str_sub(c2017$yyyymm, 5, 6), "월")
groupYear <- data.frame('2016'=c2016["visit_cnt"], '2017'=c2017["visit_cnt"])

# 컬럼 이름 바꾸기
colnames(groupYear) <- c("2016", "2017")
groupYear # 확인
# 레이블 색 뽑기
myColor01 <- rainbow(nrow(groupYear))
# 지수표기법 바꾸기
options(scipen = 25)
# 차트 그리기
barplot(as.matrix(groupYear), beside = T, col = myColor01, main = "2016 ~ 2017 총 방문객 수", ylim = c(0, 100000))
legend(x="top", legend = cMonth, bty='o', fill = myColor01, cex = 0.7, horiz = T)

# 년도에 상관 없이 4분기의 총방문객을 이용하여 Pie 그래프를 그리시오.
yearOfVisiter <- visit_USA %>% filter(str_sub(yyyymm, 5, 6)==str_sub(cMonth[10:12], 1, 2)) %>% select(yyyymm ,visit_cnt)
yearOfVisiter$yyyymm <- c(str_sub(yearOfVisiter$yyyymm, 5, 6))
yearOfVisiter <- yearOfVisiter %>% group_by(yyyymm) %>% summarise(total = sum(visit_cnt))

pieLables <- str_c(yearOfVisiter$yyyymm, "월")
pie_3d <- pie3D(x=yearOfVisiter$total, radius = 1, main = "16,17 4분기 총반문객")
pie3D.labels(pie_3d, labels = pieLables, labelcex = 1.7, height = 0.1)
# pokedex-korean.json 파일을 이용하여 
# name, img, type, height, weight 항목들만 추출하여 csv 파일로 저장하시오.
filePath <- "pokedex-korean.json"
pokeData <- fromJSON(filePath)
str(pokeData)

total_df <- data.frame()
for(idx in 1:nrow(pokeData)) {
  name <- pokeData$name[idx]
  img <- pokeData$img[idx]
  type <- pokeData$type[idx]
  height <- pokeData$height[idx]
  weight <- pokeData$weight[idx]
  
  df <- data.frame(name=name, img=img, type=type, height=height, weight=weight)
  total_df <- rbind(total_df, df)
}
total_df

# 파일쓰기
write.csv(total_df, file = "pokedex.csv", fileEncoding = "UTF-8")

# 다음 url에 대하여 json을 읽어 들여서, node_id, name, full_name 3개의 컬럼만
# result.csv 파일에 저장해 보세요.
url = 'https://api.github.com/users/hadley/repos'
urlFile <- fromJSON(url)
str(urlFile)

total_df <- data.frame()
for(idx in 1:nrow(urlFile)) {
  node_id <- urlFile$node_id[idx]
  name <- urlFile$name[idx]
  full_name <- urlFile$full_name[idx]
  
  df <- data.frame(node_id=node_id, name=name, full_name=full_name)
  total_df <- rbind(total_df, df)
}
total_df

# 파일쓰기
write.csv(total_df, file = "result.csv", fileEncoding = "UTF-8")

# jtbcnews_facebook_2016-10-01_2017-03-12.json 파일에서 
# message 항목만 추출하여 워드 클라우드를 만들어 보세요.
filePath <- "jtbcnews_facebook_2016-10-01_2017-03-12.json"
faceFile <- fromJSON(filePath)
str(faceFile)

# 새로운 벡터 하나 만들어주고.
newData <- c()
# for문을 돌려야 하니 for문을 몇번 돌릴지. 추출해야함
# myFrame의 description 벡터를 뽑을것이니.
# 벡터의 크기만큼 for문을 돌려야 하므로 msgLen에 length()함수로 행수 뽑음.
msgLen <- length(faceFile$message)

# for문을 돌림.
for (idx in 1:msgLen) {
  newData <- c(newData, faceFile$message[idx])
}
newData

# 명사를 다빼옴.
someData <- sapply(newData, extractNoun, USE.NAMES = F)
someData

# list를 풀어버림
myCheckData <- unlist(someData)
# 벡터 총 갯수
length(myCheckData)

for (i in 1:length(myCheckData)) {
  
  if(nchar(myCheckData[i]) > 6) {
    cat(myCheckData[i], "\n\n")
  }
}

# 함수 를 필터에 지정하여 단어가 2이상 6이하 단어 추출 해줌.
# nchar(x) = myCheckData 의 값을 넣고 단어의 길이를 추출함. 그리고 오른쪽 조건에의하여
# true이면 뽑고 false이면 넘김
myCheckData <- unlist(someData)
length(myCheckData)
# myCheckData
reg <- "[가-힣]{2,}|[A-z]{2,}|[0-9]{2,}"
myCheckData <- str_extract(myCheckData, reg)

# NA값 거르기
filData <- Filter(function(x) { !is.na(x) }, myCheckData)
# myCheckData <- ifelse(is.na(myCheckData), "무효", myCheckData)
# myCheckData <- Filter(function(x) { nchar(x)>=2 }, myCheckData)
# myCheckData <- Filter(function(x) { nchar(x) <= 6 }, myCheckData)
# 얼마나 추출 했는지.
# 데이터 확인하기
filData

# table로 만들어서 요약 해봄.
ta <- table(filData)

# sort함수로 정렬 하기.
result <- sort(ta, decreasing = T)
result
# dataLen <- 20 # 워드 클라우드할 갯수
# result <- head(result, length(result))

# 워드 클라우드2
# wordcloud2(result, size = 0.7, fontFamily = "Segoe UI", color = "random-dark", shuffle = T, rotateRatio = 0)
# 엘리스로 그리기
wordcloud2(result, figPath = "a.png", size = 1.0, fontFamily = "Segoe UI", shuffle = T, rotateRatio = 0)
# inchon_20160229.xml 파일을 읽어 와서 csv 파일로 저장해 보세요.
```