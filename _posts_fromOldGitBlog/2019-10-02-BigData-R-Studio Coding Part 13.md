---
layout: post
title:  "R-Studio Coding part 13 연습문제"
subtitle:   "MarkDown"
categories: devlog
tags: rsudio
comments: true
# tags: tip
---

# R - Studio Coding 연습문제

> JAVA보다 어려운거 같습니다...<br>

<br>

---
## Source code
```r
# jumsunew.csv 파일을 이용하여 다음 물음에 답하시오.
# 
# 각 과목들의 총점을 구하시오.
# 
# 각 학생별로 평균 컬럼을 추가하되, 소수점 2자리까지 표시하시오.
# 
# 학생별로 합격 여부를 판단하는 컬럼을 추가하시오.
# 3과목의 평균이 60이상이면 합격이다.
# 
# 각 과목별로 우수 또는 노력 컬럼을 판단하는 컬럼을 추가하시오.
# 점수가 50이상이면 우수, 미만이면 노력으로 추가한다.
# '우수'의 갯수가 2 이상인 행만 필터링하시오.
# 
# 국어 점수가 가장 높은 순으로 정렬하시오.
# 
# 평균 점수가 가장 낮은 순으로 정렬하시오.

# jumsunew.csv 파일을 이용하여 다음 물음에 답하시오.
excel <- read.csv("jumsunew.csv")
excel

# 각 과목들의 총점을 구하시오.
total_table <- excel[, c(3:5)]
total_table
sub_total <- apply(total_table, 2, sum)
sub_total

# 각 학생별로 평균 컬럼을 추가하되, 소수점 2자리까지 표시하시오.
avg_table <- total_table
avg_table
avg_result <- round(apply(avg_table, 1, mean), 2)
avg_result

str(excel) # 구조 확인
new_excel <- data.frame(excel, "avg"=avg_result) # 새로운 프레임 생성
new_excel # 확인

# 학생별로 합격 여부를 판단하는 컬럼을 추가하시오.
# 3과목의 평균이 60이상이면 합격이다.
col_avg = new_excel$avg
new_excel$"합격여부" <- ifelse(col_avg>=60, "합격", "불합격")
new_excel

# 각 과목별로 우수 또는 노력 컬럼을 판단하는 컬럼을 추가하시오.
# 점수가 50이상이면 우수, 미만이면 노력으로 추가한다.
# '우수'의 갯수가 2 이상인 행만 필터링하시오.


op <- new_excel[, c(3:5)]
op # 벡터 뽑기

opsw <- op[[1]][1:length(op)]
opsw

# 판단컬럼 만들기
new_excel$판단컬럼 <- 0

i <- 1
j <- 1
ch <- 0
while (i <= nrow(op)) {
 
  while (j <= length(opsw)) {
    if(op[[i , j]] >= 50) {
      ch <- ch + 1
    }
    j = j+1
  }
  
  new_excel[[i, length(new_excel)]] <- ch
  ch <- 0
  j <- 1
  i = i+1
}

new_excel

    # 우수 2이상 행만 필터링

result <- new_excel[new_excel$판단컬럼>=2, ]
result

# 국어 점수가 가장 높은 순으로 정렬하시오.
myIndex <- order(new_excel$kor, decreasing = T)
new_excel[myIndex, ]

# 평균 점수가 가장 낮은 순으로 정렬하시오.
myIndex <- order(new_excel$avg, decreasing = F)
new_excel[myIndex, ]
```

---

# 콘솔 결과

```r
> # jumsunew.csv 파일을 이용하여 다음 물음에 답하시오.
> # 
> # 각 과목들의 총점을 구하시오.
> # 
> # 각 학생별로 평균 컬럼을 추가하되, 소수점 2자리까지 표시하시오.
> # 
> # 학생별로 합격 여부를 판단하는 컬럼을 추가하시오.
> # 3과목의 평균이 60이상이면 합격이다.
> # 
> # 각 과목별로 우수 또는 노력 컬럼을 판단하는 컬럼을 추가하시오.
> # 점수가 50이상이면 우수, 미만이면 노력으로 추가한다.
> # '우수'의 갯수가 2 이상인 행만 필터링하시오.
> # 
> # 국어 점수가 가장 높은 순으로 정렬하시오.
> # 
> # 평균 점수가 가장 낮은 순으로 정렬하시오.
> 
> # jumsunew.csv 파일을 이용하여 다음 물음에 답하시오.
> excel <- read.csv("jumsunew.csv")
> excel
  id ban kor eng math
1  1   1  45  71   32
2  2   1  58  73   54
3  3   1  39  91   37
4  4   1  44  28   32
5  5   2  35  66   54
> 
> # 각 과목들의 총점을 구하시오.
> total_table <- excel[, c(3:5)]
> total_table
  kor eng math
1  45  71   32
2  58  73   54
3  39  91   37
4  44  28   32
5  35  66   54
> sub_total <- apply(total_table, 2, sum)
> sub_total
 kor  eng math 
 221  329  209 
> 
> # 각 학생별로 평균 컬럼을 추가하되, 소수점 2자리까지 표시하시오.
> avg_table <- total_table
> avg_table
  kor eng math
1  45  71   32
2  58  73   54
3  39  91   37
4  44  28   32
5  35  66   54
> avg_result <- round(apply(avg_table, 1, mean), 2)
> avg_result
[1] 49.33 61.67 55.67 34.67 51.67
> 
> str(excel) # 구조 확인
'data.frame':	5 obs. of  5 variables:
 $ id  : int  1 2 3 4 5
 $ ban : int  1 1 1 1 2
 $ kor : int  45 58 39 44 35
 $ eng : int  71 73 91 28 66
 $ math: int  32 54 37 32 54
> new_excel <- data.frame(excel, "avg"=avg_result) # 새로운 프레임 생성
> new_excel # 확인
  id ban kor eng math   avg
1  1   1  45  71   32 49.33
2  2   1  58  73   54 61.67
3  3   1  39  91   37 55.67
4  4   1  44  28   32 34.67
5  5   2  35  66   54 51.67
> 
> # 학생별로 합격 여부를 판단하는 컬럼을 추가하시오.
> # 3과목의 평균이 60이상이면 합격이다.
> col_avg = new_excel$avg
> new_excel$"합격여부" <- ifelse(col_avg>=60, "합격", "불합격")
> new_excel
  id ban kor eng math   avg 합격여부
1  1   1  45  71   32 49.33   불합격
2  2   1  58  73   54 61.67     합격
3  3   1  39  91   37 55.67   불합격
4  4   1  44  28   32 34.67   불합격
5  5   2  35  66   54 51.67   불합격
> 
> # 각 과목별로 우수 또는 노력 컬럼을 판단하는 컬럼을 추가하시오.
> # 점수가 50이상이면 우수, 미만이면 노력으로 추가한다.
> # '우수'의 갯수가 2 이상인 행만 필터링하시오.
> 
> 
> op <- new_excel[, c(3:5)]
> op # 벡터 뽑기
  kor eng math
1  45  71   32
2  58  73   54
3  39  91   37
4  44  28   32
5  35  66   54
> 
> opsw <- op[[1]][1:length(op)]
> opsw
[1] 45 58 39
> 
> # 판단컬럼 만들기
> new_excel$판단컬럼 <- 0
> 
> i <- 1
> j <- 1
> ch <- 0
> while (i <= nrow(op)) {
+  
+   while (j <= length(opsw)) {
+     if(op[[i , j]] >= 50) {
+       ch <- ch + 1
+     }
+     j = j+1
+   }
+   
+   new_excel[[i, length(new_excel)]] <- ch
+   ch <- 0
+   j <- 1
+   i = i+1
+ }
> 
> new_excel
  id ban kor eng math   avg 합격여부 판단컬럼
1  1   1  45  71   32 49.33   불합격        1
2  2   1  58  73   54 61.67     합격        3
3  3   1  39  91   37 55.67   불합격        1
4  4   1  44  28   32 34.67   불합격        0
5  5   2  35  66   54 51.67   불합격        2
> 
>     # 우수 2이상 행만 필터링
> 
> result <- new_excel[new_excel$판단컬럼>=2, ]
> result
  id ban kor eng math   avg 합격여부 판단컬럼
2  2   1  58  73   54 61.67     합격        3
5  5   2  35  66   54 51.67   불합격        2
> 
> # 국어 점수가 가장 높은 순으로 정렬하시오.
> myIndex <- order(new_excel$kor, decreasing = T)
> new_excel[myIndex, ]
  id ban kor eng math   avg 합격여부 판단컬럼
2  2   1  58  73   54 61.67     합격        3
1  1   1  45  71   32 49.33   불합격        1
4  4   1  44  28   32 34.67   불합격        0
3  3   1  39  91   37 55.67   불합격        1
5  5   2  35  66   54 51.67   불합격        2
> 
> # 평균 점수가 가장 낮은 순으로 정렬하시오.
> myIndex <- order(new_excel$avg, decreasing = F)
> new_excel[myIndex, ]
  id ban kor eng math   avg 합격여부 판단컬럼
4  4   1  44  28   32 34.67   불합격        0
1  1   1  45  71   32 49.33   불합격        1
5  5   2  35  66   54 51.67   불합격        2
3  3   1  39  91   37 55.67   불합격        1
2  2   1  58  73   54 61.67     합격        3
```