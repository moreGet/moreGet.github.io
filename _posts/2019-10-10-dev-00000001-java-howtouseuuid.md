---
date: 2019-10-10 10:56:00
layout: post
title: JAVA_UUID 사용하기
subtitle: 자바 UUID 사용 튜토리얼
description: "Current JRE : >= JDK 1.5"
image: https://res.cloudinary.com/dm7h7e8xj/image/upload/v1559820489/js-code_n83m7a.jpg
optimized_image: https://res.cloudinary.com/dm7h7e8xj/image/upload/c_scale,w_380/v1559820489/js-code_n83m7a.jpg
category: coding
tags:
  - java
  - uuid
  - unique
author: thiagorossener
# image:760*400
# optimized_image:380*200
---

# JAVA UUID (중복되지 않는 Unique Key 생성)
> Package import java.util.UUID<br>
> Current JRE : >= JDK 1.5<br>
> 유니버셜 유니크 아이디 (UUID / 128bit / unique Key)

<br>

## 기본적인 사용방법

```java
// 기본 생성자 생성 방법
UUID uid = new UUID(1, 2); // UUID(long mostSigBits, long leastSigBits)
System.out.println(uid.toString()); // UUID 결과값 toString
```

***
```
결과 : 00000000-0000-0001-0000-000000000002 //고유한 키 값
```

<br>

## 비추천 방법

```java
// 문자열 키입력 수동 생성
UUID uid = UUOD.fromString("2051a8d7-aea7-1801-e0bf-bc539dd60cf3");
System.out.println(uid.toString()); // UUID 결과값 toString
```

***
```
결과 : 2051a8d7-aea7-1801-e0bf-bc539dd60cf3 //고유한 키 값
```

<br>

## 추천 방법

```java
// 추천 방법
UUID uid = UUID.randomUUID();
System.out.println(uid.toString()); // UUID 결과값 toString
```

***
```
결과 : 2051a8d7-aea7-1801-e0bf-bc539dd60cf7 //고유한 키 값
```
