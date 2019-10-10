---
layout: post
title:  "R-Studio Coding part 9 벡터 연습문제"
subtitle:   "MarkDown"
categories: devlog
tags: rsudio
comments: true
# tags: tip
---

# R - Studio Coding 벡터 연습문제

> 중요한 개념은 계속 보면서 익숙해 져야 합니다.<br>

<br>

---
## Source code
```r
# 다음과 같은 벡터 객체를 생성하시오.
# 문제01) vector01 벡터 변수를 만들고, 'K' 문자가 5회 반복되도록 하시오.   
# 문제02) vector02 벡터 변수에 1~10까지 숫자 중에서 홀수만 포함시키시오.
# 문제03) vector03에는 1~10까지 3간격으로 연속된 정수가 3회 반복되도록 만드시오.    
# 문제04) vector04에는 vector02~vector03가 모두 포함되는 벡터를 만드시오.    
# 문제05) seq() 함수를 이용하여 25에서 -15까지 -5간격으로 벡터를 생성하시오.
# 문제06) seq() 함수를 이용하여 vector04에서 홀수번째 값들만 선택하여 vector05에 할당하시오.

# 문1
vector01 <- rep("k", 5)
vector01

# 문2
vector02 <- seq(1, 10, by=2)
vector02

# 문3
vector03 <- rep(seq(1, 10, by=3), each = 3)
vector03

# 문4
vector04 <- c(vector02, vector03)
vector04

# 문5
print(seq(25, -15, by=(-5)))

# 문6
# vector05 <- vector04[seq(1, length(vector04), by=2)]
vector05 <- vector04[seq(1, sum(vector04/vector04), by=2)]
vector05

```
---

# 콘솔 결과

```r
> # 다음과 같은 벡터 객체를 생성하시오.
> # 문제01) vector01 벡터 변수를 만들고, 'K' 문자가 5회 반복되도록 하시오.   
> # 문제02) vector02 벡터 변수에 1~10까지 숫자 중에서 홀수만 포함시키시오.
> # 문제03) vector03에는 1~10까지 3간격으로 연속된 정수가 3회 반복되도록 만드시오.    
> # 문제04) vector04에는 vector02~vector03가 모두 포함되는 벡터를 만드시오.    
> # 문제05) seq() 함수를 이용하여 25에서 -15까지 -5간격으로 벡터를 생성하시오.
> # 문제06) seq() 함수를 이용하여 vector04에서 홀수번째 값들만 선택하여 vector05에 할당하시오.
> 
> # 문1
> vector01 <- rep("k", 5)
> vector01
[1] "k" "k" "k" "k" "k"
> 
> # 문2
> vector02 <- seq(1, 10, by=2)
> vector02
[1] 1 3 5 7 9
> 
> # 문3
> vector03 <- rep(seq(1, 10, by=3), each = 3)
> vector03
 [1]  1  1  1  4  4  4  7  7  7 10 10 10
> 
> # 문4
> vector04 <- c(vector02, vector03)
> vector04
 [1]  1  3  5  7  9  1  1  1  4  4  4  7  7  7 10 10 10
> 
> # 문5
> print(seq(25, -15, by=(-5)))
[1]  25  20  15  10   5   0  -5 -10 -15
> 
> # 문6
> # vector05 <- vector04[seq(1, length(vector04), by=2)]
> vector05 <- vector04[seq(1, sum(vector04/vector04), by=2)]
> vector05
[1]  1  5  9  1  4  4  7 10 10
```