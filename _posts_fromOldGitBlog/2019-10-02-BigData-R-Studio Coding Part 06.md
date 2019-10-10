---
layout: post
title:  "R-Studio Coding part 6 csv다루기"
subtitle:   "MarkDown"
categories: devlog
tags: rsudio
comments: true
# tags: tip
---

# R - Studio Coding CSV다루기

> CSV뿐만아닌 선형데이터 를 다룹니다.<br>

<br>

---
## Source code
```r
# csv 로드를 위해 워크스페이스 바꿈
getwd()
setwd('D:/Rwork/bigdata')

# header = 컬럼명 대로 정렬해줌.
abc.csv = read.csv('abc.csv', header = T)
abc.csv

# print 는 콘솔 출력
print(abc.csv)

# View(대소문자 조심) 는 Excel 처럼 테이블 View로 출력
View(abc.csv)

# 튜플 을 출력 해줌
# 튜플 앞에서 6개 출력
head(abc.csv)
# 튜플 앞에서 3개 출력
head(abc.csv, 3)

# 튜플 뒤에서 6개 출력
# 현재 DB자체는 6줄 이라 앞뒤 다 같음.
tail(abc.csv)

# 컬럼 명만 출력
names(abc.csv)

# 컬럼 출력
colnames(abc.csv, do.NULL = T)

# 튜플 갯수 출력
row.names(abc.csv)

# 속성 집합 출력, 행이 몇개 인지, 어떤 형태의 구조인지, 컬럼은 뭐가 있는지
# mode : 자료형
# class : 자료구조
attributes(abc.csv)

# 전체적인 구조 출력 6obs. of 9var : 6행 9열
str(abc.csv)

# abc.csv$[cul] $는 직접참조(불러온 객체의 컬럼 접근)
abc.csv$job
abc.csv$total

mytotal <- abc.csv$total
mytotal

# 컬럼 배열 접근 하는법
abc.csv[6]

# 여러 컬럼 접근하는 방법
abc.csv[c("job", "total")]
abc.csv[c("gender", "age", "position")]

# 숫자로 여러 컬럼 접근하는 방법
abc.csv[c(1, 3:4)]

abc.csv[ ,2] # 열 단독 접근
abc.csv[ ,3]
abc.csv[2] # 열 단독 접근(컬럼명 포함)
abc.csv[3]
 
abc.csv[2, ] # 행 단독 접근
abc.csv[3, ]

# 1, 3행만 보여주세요.
abc.csv[c(1, 3), ]

# 2, 4열을 보여주세요.
abc.csv[ ,c(2, 4)]

# 2, 5, 6열을 보여주세요.
abc.csv[ ,c(2, 5:6)]

# 마이너스가 붙으면 exclude(2, 5, 6열 제외하고)
abc.csv[ ,-c(2, 5:6)]
```
---