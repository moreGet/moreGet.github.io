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
library(stringr)
library(KoNLP)

# Error in make.names(col.names, unique = TRUE) : 
#   invalid multibyte string 3
# 이에대한 에러가 발생한다면 인코딩을 확인하자.
myEncoding <- "UTF-8"
twitter <- read.csv("twitter.csv", header = T, fileEncoding = myEncoding)
head(twitter, 2)
colnames(twitter)

# 데이터 컬럼 변경 함.
colnames(twitter)<- c("x", "no", "id", "regdate", "contents")
colnames(twitter)

head(twitter$contents, 2)
reg <- "\\w{2,}"
# twitter$contents <- str_extract_all(twitter$contents, reg)
twitter$contents <- str_replace(twitter$contents, "\\W", " ")
# head(twitter$contents, 2)

nouns <- extractNoun(twitter$contents)
a <- MorphAnalyzer(twitter$contents, autoSpacing = T)
b <- SimplePos22(twitter$contents)

buildDictionary(ext_dic = c('sejong', 'woorimalsam'))
f <- extractNoun(twitter$contents)
f01 <- str_extract(string = f, pattern = "[가-힣]{2,}")
# f01 <- str_extract_all(string = f, pattern = "[가-힣]{2,}")
sf <- SimplePos22(f01)
sf

library(rJava)
library(KoNLP)
library(jsonlite)
library(stringr)
library(dplyr)
library(reshape2)
library(devtools)
library(wordcloud2)
library(htmltools)
library(yaml)
library(base64enc)
library(htmlwidgets)
library(ggplot2)
# table로 만들어서 요약 해봄.
ta <- table(f01)

# sort함수로 정렬 하기.
result <- sort(ta, decreasing = T)
result

wordcloud2(result, size = 1.5, fontFamily = "Segoe UI", shuffle = T, rotateRatio = 0)

# ggplot 사용하기

df <- as.data.frame(ta, stringsAsFactors = F)
top20 <- df %>% arrange(desc(df$Freq)) %>%  head(20)

# 가로막대형 의 가로축 빈도수대로 내림차순 하기.
ordering <- arrange(top20, Freq)$f01
ordering

# ggplot 구하기
# ggplot(data=top20, aes(x = f01, y = Freq, fill = f01, color = f01)) + geom_col()
# ggplot(data=top20, aes(x = f01, y = Freq, fill = f01, color = f01)) + geom_col() + coord_flip()
# ggplot(data=top20, aes(x = f01, y = Freq, fill = f01, color = f01)) + ylim(0, max(top20$Freq+100)) + geom_col() + coord_flip()
ggplot(data=top20, aes(x = f01, y = Freq, fill = f01, color = f01)) + ylim(0, max(top20$Freq+100)) + geom_col() + coord_flip() + scale_x_discrete(limit = ordering) + theme_minimal()

# 팔레트 참조해서 색 가져오기
install.packages("RColorBrewer")
library(RColorBrewer)

display.brewer.all()

# Dark2 의 8가지 범위 색 가져옴
pallette <- brewer.pal(8 ,"Set2")
# [1] "#1B9E77" "#D95F02" "#7570B3" "#E7298A" "#66A61E" "#E6AB02" "#A6761D" "#666666"
pallette

# 슬라이싱 하기.
pallette <- brewer.pal(9, "Blues")[5:9]
pallette

wordcloud2(result, size = 1.5, fontFamily = "Segoe UI", shuffle = T, rotateRatio = 0)
```

## 사용 예시 소스코드 02
```r
library(rJava)
library(KoNLP)
library(jsonlite)
library(stringr)
library(dplyr)
library(reshape2)
library(devtools)
library(wordcloud2)
library(htmltools)
library(yaml)
library(base64enc)
library(htmlwidgets)
library(ggplot2)

propose <- readLines("propose.txt")
head(propose, 4)

# 사전등록
buildDictionary(ext_dic = c('sejong', 'woorimalsam'))
newPropose <- sapply(propose , extractNoun)
str(newPropose)

vecPro <- unlist(newPropose)
length(vecPro)

# 이스케이프 시퀀스 거르기
repPropose <- str_replace(vecPro, "\\.", "")
repPropose <- str_replace(repPropose, "\\n", "")
repPropose <- str_replace(repPropose, "\\d+", "")
length(repPropose)

# 치환할 워드 텍스트 가져오기
stopWords <- readLines("propose_gsub.txt")
stopWords

for (idx in 1:length(stopWords)) {
  repPropose <- gsub(stopWords[idx], "", repPropose)
}

# 펑션으로 문자 거르기
repPropose <- Filter(function(x) { nchar(x)>=2 & nchar(x)<=6 }, repPropose)
repPropose
myFile <- "proPose_New.txt"
write(repPropose ,myFile)

# 불러오기
newData <- read.table(myFile)
newData

# 워드 클라우드 그리기
wordCount <- table(newData)
wordCount <- sort(wordCount, decreasing = T)
dataValue <- as.data.frame(head(wordCount, 15))
colnames(dataValue) <- c("내용", "빈도수")

# geom_col() : 그룹핑 한 데이터는 geom_col()를 이용한다.
# ggplot(data = dataValue, mapping = aes(x=내용, y=빈도수, fill=내용, color=내용)) + geom_col()

# 일반 데이터는 geom_bar(stat="identity") 으로 해야한다.
ggplot(data = dataValue, mapping = aes(x=내용, y=빈도수, fill=내용, color=내용)) + geom_bar(stat = "identity") + labs(title="Propose Present Top10") + theme(plot.title = element_text(hjust = 0.5))

# 워드 클라우드
wordCount <- table(newData)
head(sort(wordCount, decreasing = T), 30)

set.seed(1)

# 워드 클라우드 그리기
# The shape of the "cloud" to draw. Can be a keyword present. Available presents are 'circle' (default), 'cardioid' (apple or heart shape curve, the most known polar equation), 'diamond' (alias of square), 'triangle-forward', 'triangle', 'pentagon', and 'star'.
wordcloud2(wordCount, size = 2.5, ellipticity = 1, shuffle = T, shape = "circle") + WCtheme(1)
```

## 사용 예시 소스코드 03
```r
# install.packages('twitteR')
library(twitteR)

# API 인증을 위한 변수 목록 셋팅
apiKey <-  "AzzhJz4UldCm638r70wGSYKOa" 
apiSecret <- "GJCN2WbTjoGuPKbEoD1f5sE4Mo3EZsnuTQmCXT0PpwIbyF5MUb" 
accessToken="974830054092894208-lshTvtXvGxNpJ6EHM6m8Cx3KXlSOHUb"
accessTokenSecret="KZZNchYjasvAq4RrPAWH9CSRYso0p95hlC1n66cDmAfZg"

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