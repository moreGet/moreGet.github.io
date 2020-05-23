---
date: 2020-05-21 22:00:00
layout: post
title: Spring Boot - AOP방식의 Logging
subtitle: AOP - Logging
description: "Current JDK, Spring Boot : == JDK 1.8.0_231(64bit) / 2.1.4"
image: ../assets/img/postsimg/R_main_00000003.png
optimized_image: ../assets/img/postsimg/R_op_00000003.png
category: coding
tags:
  - IntelliJ
  - Java
  - Gradle
  - SpringBoot
  - Logging
  - AOP
author: moreGet
# image:760*400
# optimized_image:380*200
---

## Logging

요즘 애자일 방법론 으로 프로젝트를 진행 하면서 생각보다 좀더 타이트 하게 개발 업무를 수행 하는 것 같다...<br>
블로그에 소홀해 지는 시기 인것 같다...<br>
아무튼 DEV서버 에서 API가 윤곽이 들어나면서 점점 PRD쪽 이야기가 나오고 있는데... 환경셋팅도 그렇고.. 할일이 산더미 이다.<br>
그 와중에 API의 트랜잭션을 모니터링 할 일이 생겼는데 모니터링 서비스 에서 요구하는 포맷에 맞춰 AOP방식으로 Log를 Worker NODE에 특정 디렉토리에<br>
쌓아 주어야 한다. 그래서 간단하게 AOP방식을 적용하는 방법을 알아보아야 겠다.<br>

## AOP

- Aspect-Oriented Programming
- Aspect들을 모아 모듈화 하는 기법.
- <a href = "https://velog.io/@max9106/Spring-AOP%EB%9E%80-93k5zjsm95">AOP에 대한 자세한 설명 참고 블로그</a>

간단하게 나마 글로 정리해 보자면, 각 클래스 마다 비슷한 일을 하는 구현체 부분들에 대해 변경 사항이 생기면 클래스 마다 수정을 해 주어야 하는데,<br>
이는 매우 비효율 적이다. 하여 대책이 바로 AOP 방식이다. Advice(할일) 에 대해 Pointcut(적용해야 하는 구현체 부분)으로 각기 비슷한 기능의 구현체들을<br>
끌어모아 한 트랜잭션에서 이를 수행하게 한다.<br>
<br>

## MAIN

```java
// AOP방식의 Logging 구현
package Logic;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

// 생성한 Logging 클래스가 aspect로써의 동작을 수행할 수 있도록 해줍니다.
@EnableAspectJAutoProxy
@SpringBootApplication
public class MainApplication {

    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class, args);
    }
}
```

--- 

## Controller

```java
package Logic;

import lombok.RequiredArgsConstructor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RequiredArgsConstructor
@RestController
public class UserController {

    private final UserService userService;

    @GetMapping("/main")
    public void hello() {
        userService.sayHello();
    }
}
```

---

## Service

```java
package Logic;

import org.springframework.stereotype.Service;

@Service
public class UserService {

    public void sayHello() {
        System.out.println("Hello!!");
    }
}
```

---

## LoggingAOP

```java
package Logic;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;

@Aspect // 필수 선언(할 일)
@Component
public class LoggingAOP {

    // 로깅을 위해 선언
    Logger logger = LoggerFactory.getLogger(LoggingAOP.class);

    // Around를 통해 UserService클래스의 모든 메소드 들에 대해 Logging처리
    @Around("execution(* Logic.UserService.*(..))") // 이 부분은 규칙 처럼 외워야 할 것 같다.
    public Object wow(ProceedingJoinPoint pjp) throws Throwable {
        logger.info("Wow!!"); // wow로깅을 먼저 찍고
        Object result = pjp.proceed(); // proceed()를 호출 해주어야 Service로직을 수행한다(Hello 메세지 콘솔 출력)
        return result;
    }

    @After("execution(* Logic.UserService.*(..))") // 서비스 로직이 실행 후에 byebye를 콘솔에 찍는다.
    public void byebye() {
        logger.info("byebye");
    }
}
```

---

## 설명

사실 다른 부분들은 굉장히 기본적인 부분이고, LoggingAOP Class 부분만 보면 될 것 같다.<br>

<a href="https://mvnrepository.com/artifact/org.springframework">MavenReposiory 검색</a>