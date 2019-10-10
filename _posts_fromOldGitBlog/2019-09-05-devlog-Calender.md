---
layout: post
title:  "Java Calender"
subtitle:   "MarkDown"
categories: devlog
tags: java
comments: true
# tags: tip
---

# JAVA Calender를 이용한 달력 출력
>import java.util.Calendar;<br>
>Calender를 이용해 달력 출력

<br>

## 생성자 사용방법

```java
String today = "20170512"; // 특정 일자를 년 월 일 로 받고
int year = Integer.parseInt(today.substring(0, 4)); //int 형으로 파싱
int month = Integer.parseInt(today.substring(4, 6)); //같음
//int day = Integer.parseInt(today.substring(6));
int day = 1; // 1일부터
```
***
## 구현 소스

```java
String today = "20170512";
int year = Integer.parseInt(today.substring(0, 4));
int month = Integer.parseInt(today.substring(4, 6));
//int day = Integer.parseInt(today.substring(6));
int day = 1;

Calendar cal = Calendar.getInstance();
cal.set(year, month-1, day);

int lastday = cal.getActualMaximum(Calendar.DATE); 
// month 변수에 대한 마지막 일
int weekconst = cal.get(Calendar.DAY_OF_WEEK); 
// String today 에 입력한 17년 5월 1일이 어느 요일부터 시작 하는지
// 1 = 일요일 부터 시작함

System.out.println(lastday); // 한번 출력 해보기
System.out.println(weekconst); // 이것두

System.out.println(year + "년 " + month + "월 달력");
System.out.println();
System.out.println("일\t월\t화\t수\t목\t금\t토");
//수평탭 정렬로 달력 컨테이너 짜고

for(int i=1; i<weekconst; i++) {
    System.out.print("\t"); 
    //17년 5월 1일은 월요일부터 시작하니깐 weekconst 가 2임
    //그래서 월요일 까지 탭으로 위치 맞추고.
}

for(int i=1; i<=lastday; i++) {

    System.out.print(i+"\t");
    //월요일 자리부터 일자 출력 하고 수평탭 함
    if((i+weekconst-1)%7 == 0) {
        //그날 + weekconst 자릿수 - 1 한게 7로 나누었을때 0이면
        //토일일임 그러므로 한줄 개행 해줘야함
        System.out.println(); //줄 개행
    }
}

```