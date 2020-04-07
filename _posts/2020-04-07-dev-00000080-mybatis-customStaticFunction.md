---
date: 2020-04-07 22:00:00
layout: post
title: Mybatis 0 체크를 위한 Custom Static Function
subtitle: Mybatis 동적 쿼리를 위한 함수
description: "Mybatis"
image: ../assets/img/postsimg/R_main_00000003.png
optimized_image: ../assets/img/postsimg/R_op_00000003.png
category: coding
tags:
  - VisualStudioCode
  - VsCode
  - DataBase
  - DB
  - 동적 쿼리
  - PostgreSQL
  - PPAS
author: moreGet
# image:760*400
# optimized_image:380*200
---

<!-- # PPAS ( Postgres Plus Advanced Server )
PPAS는 PostgreSQL의 기반을 둔 RDMS(Raw data Management System : 기초데이터 관리 시스템) 이다. 오라클 호환을 위한 RDMS이며, 오라클의 함수와 기능을 대부분 지원하고 있다는 장점이 있다. -->

# MyBatis 환경 에서의 Integer(0 값) 체크 이슈 우회 방법

우리 개발자들은 쿼리작성시 자주 동적쿼리를 작성해야하는 문제에 직면 한다. 그리고 가끔은 유효한 값인지 체크해야할 때가 생기는데 String 값이 넘어온다면 상관이 없지만 Integer 값으로 0이 넘어올 경우 공백으로 인식할 때가 있다. 아래는 관련 소스 이다.

```xml
SELECT *
FROM ${tableName}
<WHERE>
    <if test = " num != null and num != '' ">
        ...
    </if>
</WHERE>
```

이러한 동적 쿼리를 작성할때 정수형태로 0 파라미터가 넘어오면 정상동작 하지 않는다....

이러한 이슈를 해결하기에 딱 좋은 코드가 아래 있다.

```java
public class VaildCheck(){

/**
  * Object type 변수가 비어있는지 체크
  * 
  * @param obj 
  * @return Boolean : true / false
  */
 public static Boolean empty(Object obj) {
  if (obj instanceof String) return obj == null || "".equals(obj.toString().trim());
  else if (obj instanceof List) return obj == null || ((List) obj).isEmpty();
  else if (obj instanceof Map) return obj == null || ((Map) obj).isEmpty();
  else if (obj instanceof Object[]) return obj == null || Array.getLength(obj) == 0;
  else return obj == null;
 }
 
 /**
  * Object type 변수가 비어있지 않은지 체크
  * 
  * @param obj
  * @return Boolean : true / false
  */
 public static Boolean notEmpty(Object obj) {
  return !empty(obj);
 }
}
```

위 코드를 맵퍼에서 사용한다면 정수형태의 0값이 넘어 오더라도 동적쿼리가 정상적으로 동작한다. 아래는 Mapper.xml 에서 호출방법 이다.

```xml
<if test="@패키지.클래스@메소드(파라미터)">
    ... 동작 구문
</if>
```

코드가 복잡해지고 쿼리가 많아질수록 에러의 발생원인 을 찾기 힘들어진다. 디버깅로그 와 주석의 중요성을 매번 느끼는것 같다...