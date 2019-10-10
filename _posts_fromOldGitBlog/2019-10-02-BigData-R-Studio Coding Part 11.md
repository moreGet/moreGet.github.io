---
layout: post
title:  "R-Studio Coding part 11 행렬 곱"
subtitle:   "MarkDown"
categories: devlog
tags: rsudio
comments: true
# tags: tip
---

# R - Studio Coding 행렬 계산

>수학이 약하다보니 행렬 하는것도 빡시네용... 열심히 해야겠습니다. <br>

<br>

---
## Source code
```r
mat_a <- matrix(c(1, 2), nrow=1)
mat_a

mat_b <- matrix(c(-2, 3), nrow=2)
mat_b

mat_c <- matrix(c(3, 2, -1, 0), nrow=2, byrow = T)
mat_c

# 행렬 곱셈 하기
ab <- mat_a %*% mat_b
ab

ac <- mat_a %*% mat_c
ac

ba <- mat_b %*% mat_a
ba

cb <- mat_c %*% mat_b
cb

ba <- mat_b %*% mat_a
ba

abbccsv <- read.csv("abc.csv", header = T)
abbccsv

str(abbccsv)
summary(abbccsv)

# 행열 다보여줌
dim(abbccsv)

# 행 따로 열따로 정보 출력
nrow(abbccsv)
ncol(abbccsv)

# 프로토타입 만드는 함수 sample()
# 1 부터 abbccsv 의 행사이즈 까지의 행중 3개만 랜덤으로 행을 뽑아와라.
# 왜? 1:nrow(abbccsv) 이니까!
choicel <- sample(1:nrow(abbccsv), 3)
choicel
abbccsv[choicel, ]

# 이건 3부터 nrow의 매개변수 객체 사이즈 만큼의 행중 1개만 랜덤으로 추출!
choicel2 <- sample(3:nrow(abbccsv), 1)
choicel2

# 2부터 nrow함수 매개변수 객체 사이즈 만큼의 행중 2개를 랜덤 추출
choicel3 <- sample(2:nrow(abbccsv), 2)
choicel3

# 그리고 abbccsv 에 행 기준에 choicel3 의 변수에 매칭되는 행 을 출력
# 배열로 따지자면 arr[변수][변수] 인데 변수를 랜덤으로 뽑아와서 맞는 요소를 뽑아옴
abbccsv[choicel3, ]

# 전체 행수에서 30%만 가져와라
# 데이터프레임[]에서 []안에 콤마주의
# 데이터프레임[1, ] 1행 / 데이터프레임[1] 1열 
choice4 <- sample(1:nrow(abbccsv), 0.3*nrow(abbccsv))
choice4
abbccsv[choice4, ]
str(abbccsv)

# 3개의 벡터로 데이터 프레임 만들기
no <- c(1, 2, 3)
name <- c("hong", "lee", "kim")
pay <- c(100, 200, 300)

emp01 <- data.frame(No=no, Name=name, Pay=pay)
emp01

# 조회
str(emp01)
mode(emp01)
class(emp01)

# 벡터를 가지고 메트릭스(행렬)을 만듬 그리고 데이터 프레임으로 넣어버림.
somedata <- c(1, "hong", 100, 2, "lee", 200, 3, "kim", 300)
mat <- matrix(somedata, 3, byrow = T)
mat
mode(mat)
class(mat)

# 데이터 프레임 생성
emp02 <- data.frame(mat)
emp02

# 속성 이름 바꿔주기
colnames(emp02) <- c("사번", "이름", "급여")
colnames(emp02)
emp02

# 작업공간 변경
getwd()
setwd('D:/Rwork/bigdata/S20191001/empfile')

# header = T : 기준 컬럼이 있는지, sep = "" 는 구분자
# 만약 구분자가 공백이나 탭인경우 알아서 해줌 즉, sep 생력 가능.
emp03 <- read.table("emp.txt", header = T, sep = "")
emp03

emp04 <- read.csv("emp.csv", header = T)
emp04

emp05 <- read.csv("emp2.csv", header = T)
emp05

mycolname <- c("사번", "이름", "급여")
emp06 <- read.csv("emp2.csv", header = T, col.names = mycolname)
emp06

# 
korean <- seq(50, 90, 10)
english <- seq(10, 50, 10)
name <- c("김유신", "강감찬", "이순신", "신사임당", "홍길동")
df <- data.frame(name=name, korean=korean, english=english)
df

# 타입 조회
mode(df)
class(df)

# 행 열 속성 이름 리스트 조회
nrow(df)
ncol(df)
names(df)

str(df)
df

korean1 <-subset(df, korean >= 60)
korean1

english1 <- subset(df, english <=30)
english1

# r에서 엔드는 & or는 |
# 국어는 70이상이고 영어는 40 이하인 행 추출
result <- subset(df, (korean >= 70 & english <= 40))
result                 

# 국어는 50이상이거나 영어가 40이상인 행 추출
result1 <- subset(df, (korean>=50 | english>=40))
result1

# 1열과 3열만 가져오기
result2 <- subset(df, korean>=7, c(1, 3))
result2

# 문자로 인덱싱 하기.
result3 <- subset(df, korean>=7, c("name", "english"))
result3

################################################################## 문제
# student 데이터 프레임 만들기
sid = c('a', 'b', 'c', 'd')
score = c(90, 80, 70, 60)
subject = c('컴퓨터', '국어국문', '소프트웨어', '유아교육')

# 데이터 프레임 생성
sid = c('a', 'b', 'c', 'd')
score = c(90, 80, 70, 60)
subject = c('컴퓨터', '전자 공학', '수학', '심리학')

student <- data.frame(sid, score, subject)
student 

mode(student)
class(student)

str(sid) ; str(score) ; str(subject) 
summary( student )

# 키(신장) dataframe
height <- data.frame(name=c('김유신', '이순신', '강감찬'), height=c(180, 165, 175))
height 

# 몸무게 dataframe
weight <- data.frame(name=c('김유신', '이순신'), weight=c(80, 75))
weight

###################################################################
result <- merge(height, weight, by.x='name', by.y='name')
result

result <- merge(height, weight, all=T)
result

help(merge)
```