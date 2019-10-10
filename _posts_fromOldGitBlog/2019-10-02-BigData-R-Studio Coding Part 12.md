---
layout: post
title:  "R-Studio Coding part 12 연습문제"
subtitle:   "MarkDown"
categories: devlog
tags: rsudio
comments: true
# tags: tip
---

# R - Studio Coding 연습문제

> 문법 자체는 간결 하고 직관적인 면이 좀 있지만 <br>하루에 막 때려넣으니 빡십니당.......

<br>

---
## Source code
```r
# 다음과 같은 벡터를 칼럼으로 갖는 데이터프레임을 생성하시오.
name <- c('김구', '유관순', '이순신', '김유신', '홍길동')
age <- c(55, 45, 45, 53, 15) #연령
gender <- c(1, 2, 1, 1, 1) #1:남자, 2: 여자
job <- c('연예인', '주부', '군인', '직장인', '학생')
sat <- c(3, 4, 2, 5, 5) # 만족도
grade <- c('C', 'C', 'A', 'D', 'A')
total <- c(44.4, 28.5, 43.5, NA, 27.1) #총구매금액(NA:결측치)

# 문제01) 위 7개의 벡터를 칼럼으로 갖는 members 데이터프레임을 생성하세요.
# 
# 문제02) gender 변수를 이용하여 막대 그래프를 그리시오.
# 
# 문제03) members에서 홀수 행만 선택해서 members2에 저장하세요.
# 
# 문제04) 전체 데이터에서 60%만 샘플링하여 mysampling에 저장하세요.
# 
# 문제05) 다음 물음에 답하시오.
# 나이가 50이상이 사람들을 조회하시오.
# grade가 'A'또는 'D'인 사람들의 name, age, grade만 조회하시오.
# 만족도의 평균 보다 높은 만족도를 보인 사람들을 조회하시오.

# 1번
members <- data.frame(name, age, gender, job, sat, grade, total)
members
str(members)

# 2번
gender <- factor(members$gender, levels = c(1, 2), labels = c("남", "여"))
qplot(gender)

# 3번
# 문제03) members에서 홀수 행만 선택해서 members2에 저장하세요.
members2 <- members[seq(1, nrow(members), by=2), ]
members2

# 4번
# 문제04) 전체 데이터에서 60%만 샘플링하여 mysampling에 저장하세요.
mysampling <- sample(1:nrow(members), 0.6*nrow(members))
members[mysampling, ]

# 5번
# 나이가 50이상이 사람들을 조회하시오.
age50 <- subset(members, (age >= 50))
age50

# grade가 'A'또는 'D'인 사람들의 name, age, grade만 조회하시오.
gradeAorD <- subset(members, ((grade=="A")|(grade=="B")), c("name", "age", "grade"))
gradeAorD

# 만족도의 평균 보다 높은 만족도를 보인 사람들을 조회하시오.
mean(members$sat,  na.rm = T) #만족도 평균
avg <- subset(members, sat>=mean(members$sat,  na.rm = T))
avg # 출력
```

---

# 콘솔 결과

```r
 # 다음과 같은 벡터를 칼럼으로 갖는 데이터프레임을 생성하시오.
> name <- c('김구', '유관순', '이순신', '김유신', '홍길동')
> age <- c(55, 45, 45, 53, 15) #연령
> gender <- c(1, 2, 1, 1, 1) #1:남자, 2: 여자
> job <- c('연예인', '주부', '군인', '직장인', '학생')
> sat <- c(3, 4, 2, 5, 5) # 만족도
> grade <- c('C', 'C', 'A', 'D', 'A')
> total <- c(44.4, 28.5, 43.5, NA, 27.1) #총구매금액(NA:결측치)
> 
> # 문제01) 위 7개의 벡터를 칼럼으로 갖는 members 데이터프레임을 생성하세요.
> # 
> # 문제02) gender 변수를 이용하여 막대 그래프를 그리시오.
> # 
> # 문제03) members에서 홀수 행만 선택해서 members2에 저장하세요.
> # 
> # 문제04) 전체 데이터에서 60%만 샘플링하여 mysampling에 저장하세요.
> # 
> # 문제05) 다음 물음에 답하시오.
> # 나이가 50이상이 사람들을 조회하시오.
> # grade가 'A'또는 'D'인 사람들의 name, age, grade만 조회하시오.
> # 만족도의 평균 보다 높은 만족도를 보인 사람들을 조회하시오.
> 
> # 1번
> members <- data.frame(name, age, gender, job, sat, grade, total)
> members
    name age gender    job sat grade total
1   김구  55      1 연예인   3     C  44.4
2 유관순  45      2   주부   4     C  28.5
3 이순신  45      1   군인   2     A  43.5
4 김유신  53      1 직장인   5     D    NA
5 홍길동  15      1   학생   5     A  27.1
> str(members)
'data.frame':	5 obs. of  7 variables:
 $ name  : Factor w/ 5 levels "김구","김유신",..: 1 3 4 2 5
 $ age   : num  55 45 45 53 15
 $ gender: num  1 2 1 1 1
 $ job   : Factor w/ 5 levels "군인","연예인",..: 2 3 1 4 5
 $ sat   : num  3 4 2 5 5
 $ grade : Factor w/ 3 levels "A","C","D": 2 2 1 3 1
 $ total : num  44.4 28.5 43.5 NA 27.1
> 
> # 2번
> gender <- factor(members$gender, levels = c(1, 2), labels = c("남", "여"))
> qplot(gender)
> 
> # 3번
> # 문제03) members에서 홀수 행만 선택해서 members2에 저장하세요.
> members2 <- members[seq(1, nrow(members), by=2), ]
> members2
    name age gender    job sat grade total
1   김구  55      1 연예인   3     C  44.4
3 이순신  45      1   군인   2     A  43.5
5 홍길동  15      1   학생   5     A  27.1
> 
> # 4번
> # 문제04) 전체 데이터에서 60%만 샘플링하여 mysampling에 저장하세요.
> mysampling <- sample(1:nrow(members), 0.6*nrow(members))
> members[mysampling, ]
    name age gender    job sat grade total
1   김구  55      1 연예인   3     C  44.4
3 이순신  45      1   군인   2     A  43.5
2 유관순  45      2   주부   4     C  28.5
> 
> # 5번
> # 나이가 50이상이 사람들을 조회하시오.
> age50 <- subset(members, (age >= 50))
> age50
    name age gender    job sat grade total
1   김구  55      1 연예인   3     C  44.4
4 김유신  53      1 직장인   5     D    NA
> 
> # grade가 'A'또는 'D'인 사람들의 name, age, grade만 조회하시오.
> gradeAorD <- subset(members, ((grade=="A")|(grade=="B")), c("name", "age", "grade"))
> gradeAorD
    name age grade
3 이순신  45     A
5 홍길동  15     A
> 
> # 만족도의 평균 보다 높은 만족도를 보인 사람들을 조회하시오.
> mean(members$sat,  na.rm = T) #만족도 평균
[1] 3.8
> avg <- subset(members, sat>=mean(members$sat,  na.rm = T))
> avg
    name age gender    job sat grade total
2 유관순  45      2   주부   4     C  28.5
4 김유신  53      1 직장인   5     D    NA
5 홍길동  15      1   학생   5     A  27.1
```