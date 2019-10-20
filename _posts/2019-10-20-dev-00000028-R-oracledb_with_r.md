---
date: 2019-10-20 20:50:00
layout: post
title: R-Studio OJDBC 연동하기(벌크로딩 페이지 미완성)
subtitle: R-Studio 데이터베이스를 제어하기 위한 OJDBC연동
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
# WordCloud2 를 완전히 사용하기 위한 선행 작업.
> install.packages(c("htmlwidgets", "htmltools", "yaml", "base64enc", "KoNLP", "jsonlite", "dplyr", "reshape2"))<br>
> install.packages("devtools")<br> -->

# 사용된 함수

<!-- | Use | Descriptions |
|:----------:|:-----------|
| `install.packages("KoNLP")` | library(KoNLP) 자연어 처리함수 |
| `install.packages("jsonlite")` | library(jsonlite) json 파일 다루기 | -->

<!-- ## 파일 소스
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
<br> -->

## 사용 예시 소스코드

```r
# 데이터베이스 실습(정형)
# 
# 파일 이름 : 09_03.데이터베이스 실습(정형).R
# 
# 첨부 이미지 : goods_info.png
# 
# 1. sql-developer을 이용하여 그림과 같은 테이블을 생성하세요.
# create table goods(
#   pcode char(10) primary key,
#   pname varchar2(50) not null,
#   price number(10) not null,
#   maker varchar2(25) not null
# );

driver <- "oracle.jdbc.driver.OracleDriver"
jarPath <- "C:\\oraclexe\\app\\oracle\\product\\11.2.0\\server\\jdbc\\lib\\ojdbc6.jar"
drv <- JDBC(driverClass = driver, classPath = jarPath)
chKType(drv)

# # 접속 객체 구하기
url <- "jdbc:oracle:thin:@127.0.0.1:1521/xe"
id <- "root"
password <- "root"
conn <- dbConnect(drv, url, id, password)

# 2. R에서 SQL문을 이용하여 데이터를 추가하시오.
# insert문
query <- "INSERT INTO goods"
query <- paste(query, " VALUES('1005', 'RTX2080', '510000', 'Nvidia')")
result <- dbSendUpdate(conn, query)

query <- "SELECT * FROM goods"
result <- dbGetQuery(conn, query) 
result <- result %>% arrange(PCODE) # 정렬
result
#   PCODE  PNAME  PRICE    MAKER
# 1 1001   냉장고 2000000  SM
# 2 1002   세탁기 700000   YG
# 3 1003   HDTV   2500000  GM

# 3. R에서 SQL문을 이용하여 데이터를 조회하시오.
query <- "SELECT * FROM goods"
result <- dbGetQuery(conn, query) 
result <- result %>% arrange(PCODE) # 정렬
result  

# 4. R에서 막대 그래프와 Pie 그래프를 그리시오.
barplot(main = "물품 가격", height = result$PRICE, names.arg = result$PNAME, col = rainbow(nrow(result)), ylim = c(0, max(result$PRICE)))
legend(x = "topright", legend = result$PNAME, fill = rainbow(nrow(result)), bty = 'o')

library(plotrix)
pie3d <- pie3D(main = "제품 가격", x = result$PRICE, radius = 2)
pie3D.labels(pie3d, labels = result$PNAME, height = 0, labelcex = 1.2, labelrad = 2.5)

#########################################################################33

# 벌크 로딩( csv --> table )
# 벌크(bulk)란 many, large등의 의미를 담고 있는 단어로써, 많은 량의 데이터를 의미한다.

driver <- "oracle.jdbc.driver.OracleDriver"
jarPath <- "C:\\oraclexe\\app\\oracle\\product\\11.2.0\\server\\jdbc\\lib\\ojdbc6.jar"
drv <- JDBC(driverClass = driver, classPath = jarPath)
# chKType(drv)

# # 접속 객체 구하기
url <- "jdbc:oracle:thin:@127.0.0.1:1521/xe"
id <- "root"
password <- "root"
conn <- dbConnect(drv, url, id, password)

# 데이터 조회
query <- "SELECT * FROM COUNTRY_SUMMARY"
result <- dbGetQuery(conn, query)
str(result)

# COUNTRY_SUMMARY : 히스토램 그려보기
x <- result$COUNTRY_TXT
val <- result$CNT
hist(val, breaks = 100, ylim = c(0, max(val)/130), col = rainbow(length(x)))
################################################################################################
# COUNTRY_SUMMARY : 테러 건수가 1000이상 2500이하의 데이터를 이용하여 막대 그래프
# 데이터 조회
query <- "SELECT * FROM COUNTRY_SUMMARY"
result <- dbGetQuery(conn, query) 
result <- result %>% filter(CNT>=1000 & CNT<=2500)
str(result)

x <- result$COUNTRY_TXT
val <- result$CNT
barplot(main = "나라별 테러 건수", height = val, col = rainbow(length(val)), names.arg = x, ylim = c(0, max(val)+500), axes = F)
axis(side=2, at=seq(0, max(val)+200, 100), las=1)
legend(x = "top", legend = x, fill = rainbow(length(val)), bty = 'o', horiz = T, cex = 0.8)

# SUMMARY_TOP_10 : 상위 10개국의 테러 현황을 막대그래프
query <- "SELECT * FROM COUNTRY_SUMMARY_TOP_10"
result <- dbGetQuery(conn, query) 
str(result)

x <- result$COUNTRY_TXT
val <- result$CNT
barplot(main = "Top 10 테러 건수", height = val, col = rainbow(length(val)), names.arg = x, ylim = c(0, max(val)+500), axes = F, cex.names = 0.8)
axis(side=2, at=seq(0, max(val)+1000, 500), las=1)
legend(x = "topright", legend = x, fill = rainbow(length(val)), bty = 'o', horiz = F, cex = 0.7)


# THREE_COUNTRY : 막대그래프
query <- "SELECT * FROM THREE_CONUNTY"
result <- dbGetQuery(conn, query) 
result <- dcast(result, COUNTRY_TXT ~ IYEAR, value.var = "CNT")

x <- result$COUNTRY_TXT
val <- result[-1]
leX <- colnames(result[-1])

barplot(main = "나라별 3년동안 테러 건수", height = t(as.matrix(val)), col = rainbow(length(val)), ylim = c(0, max(val)+500), axes = F, cex.names = 0.8, beside = T, names.arg = x)

axis(side=2, at=seq(0, max(val)+1000, 500), las=1)
legend(x = "topright", legend = leX, fill = rainbow(length(leX)), bty = 'o', horiz = T, cex = 0.9)
```