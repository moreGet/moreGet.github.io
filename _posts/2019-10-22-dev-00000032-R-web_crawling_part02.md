---
date: 2019-10-22 23:10:00
layout: post
title: R-Studio Web Crawling 하여 WordCloud2 그리기 Part.1
subtitle: R-Studio의 WebCrawling을 이용한 시각화.
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

## 사용 예시 소스코드 01
```r
# install.packages('twitteR')
library(twitteR)

# API 인증을 위한 변수 목록 셋팅
apiKey <-  "myUID" 
apiSecret <- "myUID" 
accessToken="myUID"
accessTokenSecret="myUID"

setup_twitter_oauth(consumer_key = apiKey, consumer_secret = apiSecret, access_token = accessToken, access_secret = accessTokenSecret)

# [1] "Using direct authentication"
# Use a local file ('.httr-oauth'), to cache OAuth access credentials between R sessions?
# 의 형식으로 물어 보는 데, 로컬 캐싱 내용을 사용할 것인가를 물어 보는 것이다.
# 2를 눌러서 새로운 인증을 시도로 하도록 한다.

searchWord <- "Seattle"
# searchWord <- '빅 데이터'
keyword <- enc2utf8( searchWord ) # base 패키지에 있음(인코딩 함수)
keyword

# Warning message:
#   In doRppAPICall("search/tweets", n, params = params, retryOnRateLimit = retryOnRateLimit,  :
#                     300 tweets were requested but the API can only return 42
searchResult <- searchTwitter(searchString = keyword, n = 300, lang = 'eng') #300개

class(searchResult) # "list"

tw_result <- twListToDF(searchResult)
head(tw_result, 2)

str(tw_result)

# 저장할 파일 이름
filename = paste('twitter(', searchWord ,').txt', sep='')
write.csv(tw_result, filename, quote = F, row.names = F)

# 텍스트, 생성횟수, 리트윗 횟수, 위치 정보 등등
colnames(tw_result)

bigdata_text <- tw_result$text
head(bigdata_text)

# install.packages('KoNLP')
library(KoNLP)
# buildDictionary(ext_dic = c('sejong', 'woorimalsam'))

# # 명사 추출 후 list를 vector으로 변환한다.
# bigdata_noun <- sapply(bigdata_text, extractNoun, USE.NAMES = F)
bigdata_noun <- unlist(bigdata_noun)
bigdata_noun
length(bigdata_noun)
# 
# 2글자 이상만 추출하기
bigdata_noun <- Filter(function(x){ nchar(x) >= 5}, bigdata_noun)

# 참고로 gsub 대신에 정규 표현식을 사용해도 된다.
# bigdata_noun <- gsub('[A-Za-z0-9]', '', bigdata_noun) # 영어와 숫자 제거
# bigdata_noun <- gsub('[~!@#$%^&*()_+|?/:;,]', '', bigdata_noun) # 특수 문자 제거
# bigdata_noun <- gsub('[ㄱ-ㅎ]', '', bigdata_noun) # 자음 제거
# bigdata_noun <- gsub('[ㅜ|ㅠ]', '', bigdata_noun) #1개 이상의 ㅜ와 ㅠ를 제거
bigdata_noun <- str_extract(bigdata_noun, "[A-z]{5,}")

word_table <- table(bigdata_noun)

# install.packages('wordcloud2')
library(wordcloud2)

word_table <- sort(word_table, decreasing = T)
wordcloud2(data = word_table, fontFamily = '맑은 고딕', size =2, color = 'random-light', backgroundColor = 'black') + WCtheme(2)

# letterCloud(data = word_table, word="설 리", fontFamily = '맑은 고딕',color = 'random-light', backgroundColor = 'black', size = 10)
```

## 사용 예시 소스코드 02
```r
library(rJava)
library(KoNLP)
library(stringr)
library(dplyr)
library(xlsx)
library(ggplot2)
# install.packages("extrafont")
library(extrafont)
library(RColorBrewer)
library(wordcloud2)

fileName <- 'ExcelData.xlsx'
data01 <- read_xlsx(fileName, sheet = 1)
data01

dustData <- data01[c(1:3)]
dustData

# 필터링
dustData_an <- dustData %>% filter(측정소명 %in% c('강동구', '송파구'))
dustData_an

# 날짜별 카운트
count(dustData_an, date) %>% arrange(desc(n))
# 측정소 별 카운트
count(dustData_an, 측정소명) %>% arrange(desc(n))

# 측정소 별로 분리하기
gangdong <- subset(dustData_an, 측정소명 == '강동구')
songpa <- subset(dustData_an, 측정소명 == '송파구')

summary(gangdong$미세먼지)
summary(songpa$미세먼지)

# theory : 두 지역의 평균은 차이가 없다.
# p-value > 0.05 이면 가설을 채택
# t 검증 : 두 집단의 평균이 같은지를 판단할 때 사용
t.test(data = dustData_an, 미세먼지 ~ 측정소명)

```