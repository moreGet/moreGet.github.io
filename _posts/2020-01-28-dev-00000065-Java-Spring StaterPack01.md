---
date: 2020-01-28 22:00:00
layout: post
title: Java SpringBoot 입문 Part.01
subtitle: SpringBoot FrameWork 사용을 위한 기초 
description: "Current JDK : == JDK 1.8.0_231(64bit)"
image: ../assets/img/postsimg/R_main_00000003.png
optimized_image: ../assets/img/postsimg/R_op_00000003.png
category: coding
tags:
  - Eclipse
  - Java
  - Lambda
  - vsCode
  - SpringBoot
  - Web
  - REST
author: thiagorossener
# image:760*400
# optimized_image:380*200
---
# Spring은 엔터프라이즈 급의 웹 애플리케이션 개발을 위한 라이브러리를 제공하는 프레임워크 이다.

<br>

- 특징, 장점
  - Lightweight Container (가벼운 컨테이너)
  - Dependency Injection Pattern 지원
  - AOP(Aspect Oriented Programming) 지원
  - POJO(Plain Old Java Object) 지원
  - 일관된 Transaction 처리 방법 제공
  - 다양한 Persistance 관련 API 제공 (JDBC, iBATIS, JPA, JDO 등...)
  - Restlet과 얀동 가능

- 관련 WebSite
  - http://springframework.org/

- 맺음말
  - Spring 프레임워크는 최근 가장 많이 사용되고 있는 웹 프레임 워크
  - 최근 주목받고 있는 REST 아키텍쳐의 자바 구현체인 Restlet과 같이 사용할 수 있다.

- 공식 웹사이트 튜토리얼
  - https://www.tutorialspoint.com/spring/spring_environment_setup.htm
  - https://www.popit.kr/%EC%8B%A0%EC%9E%85-%EA%B0%9C%EB%B0%9C%EC%9E%90-%ED%95%99%EC%83%9D%EC%9D%84-%EC%9C%84%ED%95%9C-spring-mvc-setting-1%ED%8E%B8/

- 설치 순서
  - https://maven.apache.org/download.cgi 접속 -> Link 에서 OS에 맞게 메이븐 다운로드 -> C드라이브 root에 압축해체 -> maven폴더 경로로 환경변수 설정 -> cmd 에서 mvn -v 확인해봄
  - STS 다운로드 -> https://spring.io/tools3/sts/all 맞는 버전 다운로드 -> C드라이브 root폴더에 압축해체 -> STS실행
  - Lombok설치 -> https://projectlombok.org/download -> jar파일 다운로드후 더블클릭 -> 인스톨 되어있는 이클립스 및 STS툴 자동탐색됨 -> 이후 [install/update] 버튼 클릭
  - curl설치 -> https://curl.haxx.se/download.html 에서 win64 - generic 항목 찾아봄 -> 200129기준 curl-7.68.0-win64-mingw.zip 다운로드 후 C드라이브 root 폴더에 압축해제 -> curl bin경로 환경변수 셋팅 -> curl -V으로 정상설치 확인

<hr>

## Chapter.01 기본 프로젝트 시작

- 기본 프로젝트 만들기
  - cmd창 -> 기본 템플릿 생성 명령 :: mvn -B archetype:generate -DgroupId=com.example -DartifactId=hajiboot -Dversion=1.0.0-SNAPSHOT -DarchetypeArtifactId=maven-archetype-quickstart -> 이 작업은 스프링 부트와는 관계없는 작업 일반적인 절차임. -> cmd상 현재 디렉토리에 생성됨.
  - cd hajiboot의 디렉토리로 이동
- pom.xml 설정하기 
  - 스프링 부트를 사용하는 데 필요한 설정 내용을 메이븐의 의존 관계 정의 파일에 입력합니다.

## pom.xml 설정 파일 에 새로 입력한 소스 목록이다.

> 새로 입력한 부분 주석 부분이 원래 존재하던 소스 말고 새로 입력한 부분이다.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>hajiboot</artifactId>
  <packaging>jar</packaging>
  <version>1.0.0-SNAPSHOT</version>
  <name>hajiboot</name>
  <url>http://maven.apache.org</url>
  
  <!-- 새로 입력한 부분5 -->
  <properties>
    <java.version>1.8</java.version>
  </properties>
  <!-- end -->

  <!-- 새로 입력한 부분1 -->
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.2.4.RELEASE</version>
  </parent>
  <!-- end -->

  <dependencies>
    <!-- 새로 입력한 부분2 --> 
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- end -->

    <!-- 새로 입력한 부분3 -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>
    <!-- end -->

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

    <!-- 새로 입력한 부분4 -->
    <build>
      <plugins>
        <plugin>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
      </plugins>
    </build>
    <!-- end -->
</project>
```

| 번호     | 설명                                                                                                                                                                                      |
|----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1번 부분 | 스프링 부트의 설정 정보를 상속합니다.<br><br>여기서 지정한 버전이 스프링 부트의 버전이 됩니다.<br><br>스프링 부트의 버전을 올리려면 <version>태그 안에 있는 설정 값을 변경합니다.         |
| 2번 부분 | 스프링 부트로 웹 어플리케이션을 만들 떄 참조할 기본 라이브러리 정보를 설정합니다.<br><br>웹 어플 제작에 필요한 스프링 프레임워크 관련 라이브러리와 서드파티 라이브러리를 이용할 수 있게됨 |
| 3번 부분 | 스프링 부트로 제작하는 과정에서 유닛 테스트에 필요한<br>라이브러리 참조 정보를 설정합니다(일반적인 라이브러리 이므로 추가)                                                                |
| 4번 부분 | 스프링 부트로 제작한 애플리케이션을 간단하게 빌드하고 실행하기 위해 메이븐 플로그인을 설정합니다.                                                                                         |
| 5번 부분 | 자바 8을 사용할 수 있도록 설정합니다.  

- mvn dependency:tree 명령어로 위 설정한 내용만으로도 얼마나 많은 라이브러리를 사용할 수 있게 되는지 확인해봅시다.

<hr>

## Chapter.02
- STS로 Spring 개발하기
  - 전에 만들어둔 소스 가져오기 -> STS -> file -> import -> maven -> Existing Maven Projects -> 소스폴더 클릭후 xml추가 -> STS 왼쪽에 프로젝트 생성 완료됨. -> src/main/java -> com.example.App.java 오른쪽 클릭후 Spring Boot App 실행 하면 됨.

> HelloWorld 띄워보기

```java
// App.java

package com.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * Hello world!
 *
 */

@RestController // 이 클래스가 웹 에서 요청을 받아들이는 컨트롤러 클래스임을 나타냄
@EnableAutoConfiguration // 스프링 부트에서 대단히 중요한 요소임 다양한 설정이 자동으로 수행되고 기존의 스프링 어플에 필요했던 설정 파일들이 필요 없게 됨
// @SpringBootApplication // Configuration, enableautoconfig, componentScan 등 프로그래머가 자주 사용하는 어노테이션을 기본값 으로 결합한 어노테이션 SpringBoot 버전 1.2.0 부터 사용 가능.
public class App 
{
	
	@RequestMapping // 이 메서드가 HTTP요청을 받아들이는 메소드임을 나타냄
	String home() {
		return "Hello World!"; // http응답 반환 컨트롤러 어노테이션 클래스에 속한 메서드에서 문자열을 반환하면 해당 문자열이 그대로 HTTP 응답이 되어 출력됨.
	}
	
    public static void main( String[] args )
    {
       // 스프링부트 어플을 실행하는 데 필요한 처리를 main() 메소드 안에 작성
       // @EnableAutoConfiguration 어노테이션이 붙은 클래스를 아래 클래스 메서드의 첫번째 인자로 지정합니다.
       SpringApplication.run(App.class, args);
    }
}
```
- 스프링 부트 템플릿 프로젝트를 생성해 주는 SPRING INITIALIZR 라는 서비스도 있습니다. 웹 브라우저에서 필요한 요소들을 선택하면 템플릿 프로젝트를 zip 파일로 다운로드할 수 있게 해주는 서비스 입니다.
- 코드수정 내용을 바로 반영하기 위해서 Spring loaded라는 편리한 유틸리티를 사용한다. 아래는 로디드를 사용하기 위한 pom.xml의 양식이다.
  - https://collinsd.tistory.com/284 사이트 참조
  - https://github.com/spring-projects/spring-loaded 다운로드
  - C로컬 최상위 루트에 압축 해제
  - SpringBoot App arguments(적용 프로젝트 선택후) 에 다운받은 loaded.jar을 설정
    - -javaagent:{jar디렉토리}/springloaded-1.2.8.RELEASE.jar -noverify
    - 프로젝트 오른쪽 클릭 -> maven build... 클릭후 Goals에 spring-boot:run적용\
    - 그 후 아래 Pom 파일에 요소 추가

```xml
<!-- 빌드 안에다가 입력 해야 한다-->
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				
				<!-- 추가내용 -->
				<dependencies>
					<dependency>
						<groupId>org.springframework</groupId>
						<artifactId>springloaded</artifactId>
						<version>1.2.8.RELEASE</version>
					</dependency>
				</dependencies>
				<!-- 추가 끝 -->
			</plugin>
		</plugins>
	</build>
```

<hr>

## Chapter.03 스프링 프레임워크에서 구현하는 DI
- DI(Dependency Injection)란 의존성 주입의 줄임말이며, 스프링 프레임워크의 핵심 기술입니다.
- DI를 사용하면 클래스 사이의 의존 관계를 자동으로 구성
- DI 컨테이너는 인스턴스를 관리합니다. DI 컨테이너가 인스턴스를 생성하고 그 인스턴스에 필요한 인스턴스를 설정하여 애플리케이션에 반환 합니다.
- 장점
  - 인스턴스의 스코프를 제어할 수 있다(ex. 싱글톤 객체로 생성할지 매번 새로 생성할지 등...)
  - 인스턴스의 Life Cycle을 이벤트로 제어할 수 있다.(생성과 소멸 이벤트 처리)
  - 공통 처리를 포함할 수 있다.(트랜잭션 관리나 로깅처리)
  - 객체 사이의 의존 관계가 느슨해지므로 유닛 테스트를 하기 쉬워집니다.

<hr>

## Chapter.03-1 맛보기 만들기

```java
// 인터페이스 생성
public interface Calculator {
	public int calc(int a, int b);
}

/****************************************/

// Calculator 구현 클래스
public class AddCalculator implements Calculator {

	@Override
	public int calc(int a, int b) {
		return a + b;
	}
}
```

- 위순서대로 설정후 인터페이스 선언후 AddCalculator 클래스 로 인터페이스를 구현해준다.
- Calculator 인터페이스에 어떤 실제 기능(Bean)을 제공할지는 DI 컨테이너가 관리하도록 구현합니다.
- DI 컨테이너가 Bean을 관리할 수 있도록 Bean 정의 파일을 만듭니다. 스프링 프레임워크에는 크게 다음 두가지 Bean 정의 파일 형식이 있습니다.
  - XML 정의
  - 자바 클래스(Java Config) 정의
- 두번째 자바 클래스 정의는 스프링 프레임워크 버전 3부터 도입된 형식이며, 이 책에서는 이 빙법을 사용하려 설명 합니다.

## AppConfig 클래스

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration // 이 클래스가 JavaConfig용 클래스임을 컴파일러에게 알립니다.
public class AppConfig {
	@Bean // DI컨테이너가 관리할 Bean을 생성하는 메서드에는 이 어노테이션을 붙힙니다.
	public Calculator calculator() {
		// @Bean 어노테이션으로 인해 기본적으로 메서드 이름이 Bean이름이 됩니다.
		// 또한 기본적으로 이 메소드가 생성한 이름은 싱글톤 형태로 DI컨테이너 별로 한 개만 생성 됩니다.
		return new AddCalculator(); // Calculator 타입의 싱글톤 객체
	}
}
```

- 완성된 Bean정의 파일인 AppConfig를 읽어들여 어플리케이션에서 구현객체를 실행할 Entry(출입 할 수 있는 기회, 가입 할 수 있는 기회) point를 만들겠습니다.

## App 클래스

```java
import java.util.Scanner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.Import;

@SpringBootApplication
@Import(AppConfig.class) // javaConfig흫 읽어들이기 위해 @Cunfiguration 어노테이션이 붙은 클래스를 지정
public class App {
	public static void main(String[] args) {
		
		// try() 안에 정의하는 방법을 try-with-resources이다. 처리가 끝나면 
		// 자동으로 close() 메소드가 호출되어  DI컨테이너가 소멸하고 어플리케이션을 종료합니다.
		try(ConfigurableApplicationContext context = 
				SpringApplication.run(App.class, args)) {
			
			// SpringApplication.run으로 @EnableAutoConfiguration을 붙힌 클래스를 지정
			// 이 메소드의 반환값은 DI컨테이너의 본체인 타입으로 반환 받습니다.
			Scanner sc = new Scanner(System.in); // 데이터 입력
			System.out.println("Enter 2 number like 'a b' : ");
			int a = sc.nextInt();
			int b = sc.nextInt();
			
			// getBean() 메소드를 이용하여 DI 컨테이너에서 Calculator 인스턴스를 반환 받는다
			// 실제 인스턴스는 DI가 알아서 찾아주므로 어플리케이션 쪽에서는 신경쓰지 않아도 됩니다.
			Calculator calculator = context.getBean(Calculator.class);
			int result = calculator.calc(a, b);
			System.out.println("result = " + result);
			
			sc.close(); // Scanner 닫기
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

- 위 처럼 DI컨테이너를 Import로 지정해주고 DI컨테이너를 관리한다면 어플리케이션 안에 있는 모듈 사이의 의존성이 느슨해지고 독립성이 커집니다.

<hr>

## Chapter.03-2 어플리케이션 추상화하기

- 이제 어플리케이션을 조금 더 추상화해보자 Calculator의 인자를 만들기 위해서 ArgumentResolver 인터페이스를 만듭니다.

```java
// 인터페이스 부분
import java.io.InputStream;

public interface ArgumentResolver {
	
	Argument resolve(InputStream is);
}

// Argument 클래스 구현 부분
import lombok.Data;
import lombok.RequiredArgsConstructor;

@Data // class파일을 생성할때 각 필드의 setter/getter, toString, equals, hashCode 메소드가
// 생성되므로 소스코드가 간결해진다. 아래 필드는 final형태라 setter/getter가 생성되지 않는다.
@RequiredArgsConstructor
public class Argument {
	private final int a;
	private final int b;
}
```

- 자바 클래스를 쉽게 작성하기 위한 Lombok을 사용합니다. 다음과 같이 pom에 추가 합니다.
```java
  <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.10</version>
    <scope>provided</scope>
  </dependency>
```

- 앞에서 한 것처럼 Scanner로 인자를 생성해서 반환하는 클래스(ArgumentResolver의 본체)를 만듭니다.

```java
// ArgumentResolver의 본체
import java.io.InputStream;
import java.util.Scanner;

// Resolver : 결의, 결심, 결단
public class ScannerArgumentResolver implements ArgumentResolver{
	
	@Override
	public Argument resolve(InputStream is) {
		Scanner scanner = new Scanner(is);
		
		int a = scanner.nextInt();
		int b = scanner.nextInt();
		return new Argument(a, b);
	}
}
```

- 위 본체를 DI컨테이너가 관리하게 끔 Bean을 AppConfig에 생성해줌

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration // 이 클래스가 JavaConfig용 클래스임을 컴파일러에게 알립니다.
public class AppConfig {
	
	@Bean // DI컨테이너가 관리할 Bean을 생성하는 메서드에는 이 어노테이션을 붙힙니다.
	public Calculator calculator() {
		...위 AppConfig와 동일
	}
	
  // 이 부분을 추가해줌 의존성을 낮추고 독립성을 높힘
	@Bean // DI컨테이너가 관리할 Bean을 생성하는 메서드
	ArgumentResolver argumentResolver() {
		return new ScannerArgumentResolver();
	}
}
```

- 이제 DI컨테이너 에서 수정을 거친다

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.Import;

@SpringBootApplication
@Import(AppConfig.class) // javaConfig흫 읽어들이기 위해 @Cunfiguration 어노테이션이 붙은 클래스를 지정
public class App {
	public static void main(String[] args) {
		
		// try() 안에 정의하는 방법을 try-with-resources이다. 처리가 끝나면 
		// 자동으로 close() 메소드가 호출되어  DI컨테이너가 소멸하고 어플리케이션을 종료합니다.
		try(ConfigurableApplicationContext context = 
				SpringApplication.run(App.class, args)) {
			
			// SpringApplication.run으로 @EnableAutoConfiguration을 붙힌 클래스를 지정
			// 이 메소드의 반환값은 DI컨테이너의 본체인 타입으로 반환 받습니다.
			System.out.println("Enter 2 numbers like 'a b' : ");
			ArgumentResolver ar = context.getBean(ArgumentResolver.class);
			
			Argument argument = ar.resolve(System.in);
			Calculator calculator = context.getBean(Calculator.class);
			
      // @Data 어노테이션
			int result = calculator.calc(argument.getA(), argument.getB());
			System.out.println("result = " + result);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

- 지금까지 DI 컨테이너에서 Bean을 가져오는 방법을 설명 했습니다. 아직 의존성 주입(DI)은 하지 않았습니다. 이런 방식으로 프로그램이 커진다면 App클래스 안에서 context.getBean()을 호출할 일이 많아지면서 관련 모듈이 늘어나고 코드가 지저분해질 것입니다. 
- 이 문제를 해결하는 데 필요한 것이 DI입니다. DI를 사용하도록 어플리케이션을 수정 하겠습니다.

<hr>

## Chapter.03-3 오토 와이어링을 이용한 DI

- 지금까지는 DI 컨테이너(App.java) 에서 명시적으로 Bean을 가져오도록 App 클래스를 구현했습니다. 이번에는 DI 컨테이너가 클래스에 Bean을 주입(Injection) 하도록 구현 하겠습니다. 먼저 App 클래스에 구현한 처리를 정리할 FrontEnd 클래스를 만듭니다. 이미 작성한 Calculator와 ArgumentResolver를 이 Frontend 클래스에 주입합니다.

```java
import org.springframework.beans.factory.annotation.Autowired;

public class Frontend {

	@Autowired // 어노테이션을 붙인 필드는 DI 컨테이너가 주입해야 할 필드를 의미합니다.
	ArgumentResolver argumentResolver; // 자동으로 객체를 넣어줌
	@Autowired
	Calculator calculator; // 자동으로 객체를 넣어줌
	// DI컨테이너는 관리하고 있는 객체 중에서 @Autowired 어노테이션이 붙은 필드에 맞는 객체를 자동으로 찾아내어 주입.
	// 이를 오토 와이어링 이라고 부릅니다.
	
	public void run() {
		System.out.print("Enter 2 number like 'a b' : ");
    // 오토 와이어링 으로 인해 자동으로 객체를 찾아줌.
		Argument argument = argumentResolver.resolve(System.in);
		int result = calculator.calc(argument.getA(), argument.getB());
		System.out.println("result = " + result);
	}
}
```

- 이어서 Bean정의 파일에(AppConfig) Frontend클래스도 추가 정의 합니다.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration // 이 클래스가 JavaConfig용 클래스임을 컴파일러에게 알립니다.
public class AppConfig {
	
  ... 다른 Bean 메소드
	
  // 아래는 추가 부분
	@Bean // DI컨테이너가 관리할 Bean을 생성하는 메서드
	Frontend frontend() {
		return new Frontend();
	}
}
```

- 지금까지 살펴본 예쩨에서는 인터페이스의 본체를 제공하는 방식으로 Bean을 정의 했습니다. 그러나 Bean 정의 파일에서 정의하는 방식에 이런 제약은 없습니다. 인터페이스의 본체에 국한되지 않고 임의의 클래스를 정의할 수 있습니다.

## 오토와이어링 을 적용한 후 의 DI 컨테이너

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.Import;

@SpringBootApplication
@Import(AppConfig.class) // javaConfig흫 읽어들이기 위해 @Cunfiguration 어노테이션이 붙은 클래스를 지정
//@Import(AppConfig.class) // ComponentScan으로 인해 필요 없습니다.
public class App {
	public static void main(String[] args) {
		
		// try() 안에 정의하는 방법을 try-with-resources이다. 처리가 끝나면 
		// 자동으로 close() 메소드가 호출되어  DI컨테이너가 소멸하고 어플리케이션을 종료합니다.
		// SpringApplication.run으로 @EnableAutoConfiguration을 붙힌 클래스를 지정
		// 이 메소드의 반환값은 DI컨테이너의 본체인 타입으로 반환 받습니다.
		try(ConfigurableApplicationContext context = 
				SpringApplication.run(App.class, args)) {
			
			Frontend frontend = context.getBean(Frontend.class);
			frontend.run();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

- 이전 DI컨테이너(App.java) 소스코드 보다 간결해 졌다.
- @Import() 에 config(Bean정의 파일)클래스 를 임포트 시켜 getBean으로 받아 클래스를 구현 했다. 오토와이어링을 적용한 DI컨테이너의 코드는 이렇게 간결하다.

<hr>

## Chapter.03-5 컴포넌트 스캔을 사용하여 자동으로 Bean 등록하기

- 그 다음 해결해야 할 문제는 DI 컨테이너에 등록할 Bean을 하나하나 정의하기가 번거롭다는 것 입니다. 스프링 프레임워크는 Bean을 DI 컨테이너에 자동으로 드옥하는 컴포넌트 스캔 이라는 기능을 통해 이 문제를 해결합니다.
- @ComponentScan은 @SpringBootApplication 에 내포 되어 있습니다. 
- @ComponentScan의 Scope에 반영되기 위해선 반영을 원하는(자동 Bean 등록) 클래스 에 @Component 어노테이션을 명시 합니다.
- 여기선 AddCalculator, ScannerArgumentResolver, Frontend 클래스에 붙혀 줍니다.
- 자! 이제 위 ComponentScan으로 인해 AppConfig 클래스는 필요가 없습니다! 이 클래스를 삭제후 소스를 동작 시켜 봅니다!

<hr>

## Chapter.03-6 CommandLineRunner 사용하기

- CommandLineRunner의 사용으로 FrontEnd 클래스의 역할을 App 클래스가 대신 할 수 있게끔 할 수 있습니다.
- 아래는 CommandLineRunner 인터페이스 구현으로 App클래스가 FrontEnd 클래스의 역할 까지 하게 된 바뀐 App클래스 입니다.
- 이는 FrontEnd 클래스가 더는 필요가 없다는 것을 의미 합니다. 지우세요!

## CommandLineRunner 인터페이스 구현으로 바뀐 App 클래스

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class App implements CommandLineRunner{ // 앞서 구현한 FrontEnd 클래스의 Run메소드와 같은 역할
	
	@Autowired // 자동 오토 와이어링 으로 인해 자동으로 객체를 찾아주는 역할을 하라 하는 어노테이션
	Calculator calculator;
	@Autowired
	ArgumentResolver argResolver;
	
	@Override
	public void run(String... args) throws Exception {
		System.out.println("Enter 2 number like 'a b' : ");
		
		// 오토 와이어링 으로 인한 자동객체 대입
		Argument argument = argResolver.resolve(System.in);
		
		// 오토 와이어링...
		// Argument 에서 구현한 InputStream 으로 받은 키보드 인자값을 계산;
		int result = calculator.calc(argument.getA(), argument.getB());
		System.out.println("result : " + result);
	}
	
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}
}
```

## Chapter 정리
- 위 이론은 어플리케이션 규모가 커질수록 효과가 매우 증대 됩니다.
- 따라서 매우 중요한 이론이므로 기본기가 탄탄 해야 합니다.
- 아래는 최종 프로젝트 구성 입니다.

![commandLineRunner](/assets/sources/commandLineRunner.jpg "SpringBoot")

<hr>

## Chapter.03-7 레이어로 구성한 컴포넌트 주입하기

- 스프링 프레임워크에는 @Component 외에도 컴포넌트 스캔의 대상이 되는 어노테이션이 있습니다.

| Annotation  | description                                                                                                                                                                   |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @Controller | 웹 MVC 프레임워크인 스프링 MVC의 컨트롤러를 나타내는 어노테이션 입니다. 스프링4<br>에서는 REST웹 서비스용으로 @RestController를 추가했습니다.                                 |
| @Service    | 서비스 클래스를 나타내는 어노테이션 입니다. 기능 면에서는 @Component와 다르지 않습니다.                                                                                       |
| @Repository | 리포지토리 클래스를 나타내는 어노테이션 입니다. 이 어노테이션이 붙은 클래스<br>안에서 발생한 예외는 특수한 규칙에 따라 스프링이 제공하는 DataAccessException으로 교체 됩니다. |

- 한 가지 더 Bean 정의 파일인 JavaConfig클래스에 붙인 @Configuration도 컴포넌트 스캔의 대상이 됩니다.
- 리포지토리는 도메인 객체를 보존하거나 얻어오고 검색하는 조작을 캡슐화하며 컬렉션 객체와 동일한 역할을 담당합니다. 리포지토리에는 로직을 포함하면 안 됩니다.

- 아래 는 스프링 프레임워크를 사용한 어플리케이션 구성이다.<br>

(엔트리 포인트/컨트롤러) -> (서비스) -> (리포지토리)<br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;↑----------------↓  ↑-----------↓<br>
<---------------&emsp;&emsp;(DI 컨테이너)&emsp;&emsp;-----------------><br>

- 보통 각 레이어에 클래스를 만들고 DI 컨테이너가 이 클래스들을 주입하도록 구성 합니다.
- 어플리케이션에서는 서비스 클래스가 엔티티 같은 도메인 객체의 컬렉션인 리포지토리 클래스를 사용하여 로직을 조합하며, 엔트리 포인트(웹 에서는 컨트롤러)가 사용자의 요청에 맞춰 서비스 클래스를 호출합니다.
- 의존성 해결은 DI 컨테이너에 맡깁니다.

<hr>

## Chapter.04-1 리포지토리와 서비스를 이용한 간단한 고객 과닐 시스템 어플 만들기

- 새 프로젝트를 생성하여 만듭니다.
- pom.xml에 설정할 의존 관계는 앞에서 한 것과 동일합니다.
- 현재 까지의 프로젝트 구성은 아래와 같다.

![SimpleServiceApp](/assets/sources/SimpleServiceApp.jpg "SpringBoot")

- 4개의 클래스 소스는 아래와 같다.

```java
//------------------------ Domain Class
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

// @Data의 역할
// class파일을 생성할때 각 필드의 setter/getter, toString, equals, hashCode 메소드가
// 생성되므로 소스코드가 간결해진다. 아래 필드는 final형태라 setter/getter가 생성되지 않는다.
@Data
@AllArgsConstructor // 롬복이 제공하는 이 어노테이션들을 붙혀서 모든 필드를 인자로 받는 생성자를 만듭니다.
@NoArgsConstructor
public class Customer {
	private Integer id;
	private String firstName;
	private String lastName;
}

//------------------------ Repository Class
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.ConcurrentMap;

import org.springframework.stereotype.Repository;

import com.example.demo.domain.Customer;

@Repository
public class CustomerRepository {
	private final ConcurrentMap<Integer, Customer> customerMap = 
			new ConcurrentHashMap<Integer, Customer>();
	
	// 전부 출력
	public List<Customer> findAll() {
		return new ArrayList<Customer>(customerMap.values());
	}
	
	// HashMap 요소 Key값 으로 찾기
	public Customer findOne(Integer customerId) {
		return customerMap.get(customerId);			
	}
	
	// HashMap 요소 추가
	public Customer save(Customer customer) {
		return customerMap.put(customer.getId(), customer);
	}
	
	// HashMap 요소 삭제
	public void delete(Integer customerId) {
		customerMap.remove(customerId);
	}
}

//------------------------ Service Class
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.example.demo.domain.Customer;
import com.example.demo.repository.CustomerRepository;

@Service
public class CustomerService {

	@Autowired
	CustomerRepository customerRepository;

	public Customer save(Customer customer) {
		return customerRepository.save(customer);
	}

	public List<Customer> findAll() {
		return customerRepository.findAll();
	}
}

//------------------------ App Class
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import com.example.demo.domain.Customer;
import com.example.demo.service.CustomerService;

@SpringBootApplication
public class App implements CommandLineRunner{
	
	@Autowired
	CustomerService customerService;
	
	@Override
	public void run(String... args) throws Exception {
		// 데이터 추가
		customerService.save(new Customer(1,  "Nobita", "Nobi"));
		customerService.save(new Customer(2, "Shin", "SungHyeun"));
		customerService.save(new Customer(3, "Kim", "ShinHan"));
		
		// 데이터 표시
		customerService.findAll().forEach(System.out::println);
	}
	
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}
}
```

- 실행 하게 되면 선언한 동시성 성질을가진 HashMap에 누적시킨 고객 요소가 출력 된다.

<hr>

## Chapter.04-2 스프링 JDBC를 사용한 DB접속

- 스프링 프레임워크에는 기본 사양으로 스프링 JDBC라는 모듈이 포함되어 있다.
- JDBC API를 쉽게 사용하기 위한 JdbcTemplate Class가 내포되어 있다.
- 복잡한 JDBC를 간결하게 사용할수 있어 소규모 DB핸들링이 용이하다.

## 새로운 프로젝트 생성후 pom.xml 의존 관계를 추가하자.

- 아래는 새로 추가할 dependency 입니다.
- jdbcTemplate을 사용하기 위한 추가 요소 입니다.

```xml
<!-- 밑의 의존관계를 추가하면 스프링 JDBC로 DB에 접속하는 기능에 필요한 lib가 추가 됩니다. -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>

<!-- 이번 예제 에서는 h2 DB를 사용 합니다. -->
<dependency>
  <groupId>com.h2database</groupId>
  <artifactId>h2</artifactId\>
</dependency>
```

## Chpater.04-3 JdbcTemplate으로 DB 접속하기

- 이번에는 위 Template을 래핑하여 더욱 편리하게 사용할 수 있는 
  - NamedParameterJdbcTemplate 을 사용 한다.
  - 위 템플릿은 SQL구문 안에 파라미터를 삽입할 수 있도록 ':param' 형식의 플레이스 홀더를 이용
  - 일반적인 JDBC API는 ?를 플레이스 홀더로 사용하므로 불편
  - NamedPatameterJdbcTemplate의 플레이스홀더는 다루기 쉬움 오류를 방지
  - 아래는 이를 위한 소스구현 부분 입니다.

## App.java 구현 부분

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.jdbc.core.namedparam.MapSqlParameterSource;
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate;
import org.springframework.jdbc.core.namedparam.SqlParameterSource;

@SpringBootApplication
public class App implements CommandLineRunner{
	
	/*
	 *	DI 컨테이너에 등록된 NamedParameter객체를 얻어옵니다.
	 *	SpringBoot는 자동 설정 이라는 기능을 통해 DataSource나 JdbcTemplate, NamedParameter... 을 자동으로 생성하여
	 *	DI 컨테이너에 등록 합니다.
	 *	따라서 SpringBoot는 의존 lib에 spring-boot-starter-jdbc와 JDBC 드라이버만 추가하면 다른 설정을 하지 않아도
	 *	위 템플릿을 사용할 수 있습니다. 이번 예제는 pom.xml에 H2의존 관계를 JDBC드라이버로 정의 했으므로 H2 데이터베이스용 DataSource가 
	 *	생성됩니다. H2 DB를 사용하여 DB의 URL을 지정하는 일이 없다면 기본값으로 인 메모리 DB가 만들어집니다. DB설정이 번거롭지 않으므로
	 *	동작을 확인하기에는 편리 합니다. 
	 */
	@Autowired
	NamedParameterJdbcTemplate jdbcTemplate;
	
	@Override
	public void run(String... args) throws Exception {
		String sql = "SELECT 1"; // 간단한 SQL입니다. 문법상 맞지않는 DBMS도 있지만 H2는 그대로 1을 반환합니다.
		
		/*
		 *  밑의 클래스로 SQL에 삽입할 param을 만듭니다 이번예제 에서는 param을 사용하지 않으므로
		 *	디폴트 생성자를 이용 합니다. 이것 대신 Map<String, Objecr>를 사용 가능 하지만
		 *	더 편리한 밑의 클래스를 사용합니다.
		 */	
		SqlParameterSource param = new MapSqlParameterSource();
		
		/*
		 * queryForObject() 메소드를 사용하여 쿼리 실행 결과를 오브젝트로 변환한 상태로 얻어옵니다.
		 * 첫번째 인자로 SQL, 두번째 인자로 param, 세번쨰 인자로 반환 값이 될 객체 클래스를 지정합니다.
		 * 이 메소드는 쿼리 반환 값이 단일 레코드가 아니면 IncorrectResult... 예외를 발생 시킵니다.
		 */
		Integer result = jdbcTemplate.queryForObject(sql, param, Integer.class);
		
		System.out.println("result = " + result);
	}
	
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}
}
```

- 이제 param을 지정해보겠습니다. 아래는 수정된 App Class 입니다.
- 위 부분에서 수정된 부분만 기재 합니다.

```java
	@Override
	public void run(String... args) throws Exception {
		String sql = "SELECT :a + :b"; // :a와 :b 라는 플레이스 홀더 사용

		// 아래 param addValue() 메서드로 플레이스 홀더로 명시한 param값에  값을 넣어줌.
		SqlParameterSource param = new MapSqlParameterSource()
				.addValue("a", 100)
				.addValue("b", 200);

		Integer result = jdbcTemplate.queryForObject(sql, param, Integer.class);
		
		System.out.println("result = " + result);
	}
	
	// 콘솔창에 결과값 300 출력 됨.
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}
```

<hr>

- 다음은 JdbcTemplate을 사용한 객체 매핑 입니다.
- SQL의 실행결과를 Java객체에 매핑 하는 방법이다.
- Customer클래스를 이용하여 SQL의 결과를 Customer객체에 매핑
- 쿼리 결과를 위해 코드를 수정하는데 매핑을 위해 RowMapper를 익명 클래스로 구현
- 아래는 수정된 App 이다.
- 고객관리 어플리케이션(바로 위 프로젝트 구성)구성 과 프로젝트 구성이 같아야 합니다. (Customer 필요)

```java
import java.sql.ResultSet;
import java.sql.SQLException;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.core.namedparam.MapSqlParameterSource;
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate;
import org.springframework.jdbc.core.namedparam.SqlParameterSource;

import com.example.app.domain.Customer;

@SpringBootApplication
public class App implements CommandLineRunner{

	@Autowired
	NamedParameterJdbcTemplate jdbcTemplate;
	
	@Override
	public void run(String... args) throws Exception {
		// customers 테이블에서 키를 지정하여 정보를 얻는 SQL를 작성
		String sql = "SELECT id, first_name, last_name FROM customers WHERE id = :id";
		
		// 파라미터로 기본 키의 값을 설정합니다.
		SqlParameterSource param = new MapSqlParameterSource()
				.addValue("id", 1);
		
		// ResultSer에서 Customer 객체를 생성하는 RowMapper<Customer>를 구현합니다.
		Customer result = jdbcTemplate.queryForObject(sql, param, new RowMapper<Customer>() {
			@Override
			public Customer mapRow(ResultSet rs, int rowNum) throws SQLException {
				return new Customer(rs.getInt("id"), 
									rs.getString("first_name"), 
									rs.getString("last_name"));
			}
		});
		
		System.out.println("result = " + result);
	}
	
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}
}
```

- 현재는 DB에 테이블이나 데이터를 등록하지 않은 상태라 실행시 예외가 발생 합니다.
- SpringBoot에서는 클래스 패스 아래에 다음과 같은 SQL파일이 존재하면 그것을 읽어들여 실행 합니다.
  - schema-플랫폼.sql
  - schema.sql
  - data-플랫폼.sql
  - data.sql
- 이번 예제에서는 src/main/resources/schema.sql에 이번에 사용할 DDL을 기술하고 src/main/resources/data.sql에 초기 데이터를 기술합니다.

```sql
-- src/main/resources/schema.sql 에 사용할 DDL
CREATE TABLE customers (id INT PRIMARY KEY AUTO_INCREMENT, first_name VARCHAR(30), last_name VARCHAR(30));
```

```sql
-- src/main/resources/data.sql 초기 데이터
INSERT INTO customers(first_name, last_name) VALUES('Nobita', 'Nobi');
INSERT INTO customers(first_name, last_name) VALUES('Nobita2', 'Nobi2');
INSERT INTO customers(first_name, last_name) VALUES('Nobita3', 'Nobi3');
INSERT INTO customers(first_name, last_name) VALUES('Nobita4', 'Nobi4');
```

- 헌데 App.class에 RowMapper<>를 익명 클래스로 구현한 게 조금 번잡스러워 보입니다.
- 이는 Lambda식 으로 해결볼 수 있습니다.

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.jdbc.core.namedparam.MapSqlParameterSource;
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate;
import org.springframework.jdbc.core.namedparam.SqlParameterSource;

import com.example.app.domain.Customer;

@SpringBootApplication
public class App implements CommandLineRunner{

	@Autowired
	NamedParameterJdbcTemplate jdbcTemplate;
	
	@Override
	public void run(String... args) throws Exception {
		// customers 테이블에서 키를 지정하여 정보를 얻는 SQL를 작성
		String sql = "SELECT id, first_name, last_name FROM customers WHERE id = :id";
		
		// 파라미터로 기본 키의 값을 설정합니다.
		SqlParameterSource param = new MapSqlParameterSource()
				.addValue("id", 1);
		
		// ResultSer에서 Customer 객체를 생성하는 RowMapper<Customer>를 구현합니다.
		// Lambda식 으로 코드를 간소화 합니다.
		Customer result = jdbcTemplate.queryForObject(sql, param, (rs, num) -> new Customer(
				rs.getInt("id"), rs.getString("first_name"), rs.getString("last_name")));
		
		System.out.println("result = " + result);
	}
	
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}
}
```

- 위 람다식으로 래퍼 클래스를 간소화 하니 코드가 간결.
- 스프링 프레임워크에는 익명 클래스를 인자로 받는 XXXTemplate 형태의 클래스가 많으므로 람다 표현식을 사용하면쉽게 기술할 수 있습니다.
- SQL파일에 한국어를 사용하려면 src/main/resources/application.yml 파일에 아래 코드 지정 필요
  - spring.datasource.sqlScriptEncoding: UTF-8

<hr>

## Chapter.04-4 데이터 소스 설정을 명시적으로 변경하기

- 지금까지는 JDBC설정이 자동으로 일어나는 것을 사용 했습니다.
- H2 DB용 JDBC드라이버를 의존관계로 추가하면 기본값으로 내장되어 있는 인 메모리 DB를 사용 합니다 따라서 JDBC설정을 변경 하려면 설정 파일에 명시적 설정이 필요
- 스프링 부트에서 속성 설정은 클래스 패스 바로 아래에 있는 application.properties 또는 application.yml에 기술합니다.
- 일단은 지금까지 인 메모리 데이터베이스를 사용해온 상태로 명시적으로 설정 하겠습니다.
- 위 두파일 중 가지고 있는 설정파일안에 설정합니다.

```properties
spring:
	datasource:
		driverClassName: org.h2.Driver
		url: jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
		username: sa
		password:
```

- 위는 DBMS 속성정보를 가진 설정 파일 입니다.
- 위 처럼 설정하면 App이 실행을 끝낼 때마다 데이터가 사라집니다.
- 데이터가 지속되도록 하기 위해 H2 DB를 File DB방식으로 사용하도록 설정을 변경 하겠습니다.
- 아래와 같이 url 속성을 바꿔 줍니다.

```properties
spring:
	datasource:
		driverClassName: org.h2.Driver
		url: jdbc:h2:file:/tmp/testdb
		username: sa
		password:
```

- 위와 같이 설정후 App.java를 실행하면 C:\tmp\testdb.h2.db라는 파일이 생성되며, 이 파일에 DB정보가 입력되어 지속 됩니다.
- 전에 설정해 놓았던 schema.sql이 App이 실행될떄 마다 반복 실행되어 예외가 발생하는데 이를 방지하기 위해 아래처럼 스키마를 수정 합니다.

```sql
CREATE TABLE IF NOT EXISTS customers (id INT PRIMARY KEY AUTO_INCREMENT, first_name VARCHAR(30), last_name VARCHAR(30));
```

- 다만 data.sql은 여러번 실행 됩니다.
- 위 프로퍼티 파일의 다른 공식은 레퍼런스 페이지를 참조 해야 합니다.
- https://docs.spring.io/spring-boot/docs
- 현재 사용하는 버전 https://docs.spring.io/spring-boot/docs/2.2.4.RELEASE/reference/html/appendix-application-properties.html#common-application-properties
- 다음 챕터의 Flyway를 이용한 DB 마이그레이션 이후에는 파일에 보존하는 DB를 사용 합니다.

<hr>

## Chapter.04-5 Log4JDBC2

- 개발 과 유지보수 시 좀더 작업향상을 위해 Log4jdbc 를 사용합니다.
  - 참고 사이트 http://log4jdbc.brunorozendo.com/how-to-use.html
- 해야할 작업은 다음과 같습니다
  - pom.xml 의존성 추가
  - application.properties 수정
  - /src/main/resources 폴더에 logback-sptring.xml 생성 및 설정
  - 입니다.

### pom.xml 의존성 추가

```xml
<dependency>
	<groupId>org.bgee.log4jdbc-log4j2</groupId>
	<artifactId>log4jdbc-log4j2-jdbc4.1</artifactId>
	<version>1.16</version>
</dependency>
```

### application.properties 설정

```properties
# spring.datasource.url=jdbc:h2:file:C:/tmp/testdb
spring.datasource.url=jdbc:log4jdbc:h2:file:C:/tmp/testdb;AUTO_SERVER=TRUE
spring.datasource.driver-class-name=net.sf.log4jdbc.sql.jdbcapi.DriverSpy

# spring.datasource.url=jdbc:h2:mem:testdb

spring.datasource.data-username=sa
spring.datasource.data-password=

# base Properties ON/OFF
spring.h2.console.enabled=false
```

### logback-spring.xml 설정

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
	<include resource="org/springframework/boot/logging/logback/base.xml"/>
	
	<!-- 좀 더 보기 좋은 MyBatis 쿼리 Log : log4jdbc -->
	<logger name="jdbc.sqlonly" level="debug" />
	<logger name="jdbc.sqltiming" level="off" />
	<logger name="jdbc.audit" level="off" />
	<logger name="jdbc.resultset" level="off" />
	<logger name="jdbc.resultsettable" level="debug" />
	<logger name="jdbc.connection" level="off" />
</configuration>
```

- jdbc.sqlonly : SQL만 log. prepared statement에서 실행된 SQL은 해당 위치에 바인딩된 argument로 자동으로 표시되므로 가독성이 크게 향상된다.
- jdbc.sqltiming : 실행시간과 SQL log
- jdbc.audit : ResultSets을 제외한 모든 JDBC 호출 log. 이는 매우 방대한 출력이며 특정 JDBC 문제를 추적할 경우가 아니면 일반적으로 필요하지 않음
- jdbc.resultset : ResultSet 객체에 대한 모든 호출이 기록되므로 훨씬 더 방대
- jdbc.resultsettable : 테이블로 jdbc 결과를 log. Level debug는 result set에서 읽지 않은 값을 채운다
- jdbc.connection : connection open, close 이벤트를 기록하고 열려있는 모든 connection number를 dump. connection 누수 문제를 해결하는데 매우 유용
- 참고 블로그 : https://sosohanya.tistory.com/31

<hr>

## Chapter.04-6 JdbcTemplate으로 리포지토리 클래스 구현하기

- 앞서 만든 Customerepository 클래스를 NamedParameterJdbcTemplate을 사용해서 다시 Customer 객체는 메모리에서 DB로 저장됩니다.
- 지금까지 NamedParameterJdbcTemplate을 사용해서 DB를 참조 했습니다. 이제 CustomerRepository 클래스를 구현해서 DB를 업데이트하는 방법을 설명하겠습니다.

### 수정한 Customer리포지토리

```java
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.core.namedparam.BeanPropertySqlParameterSource;
import org.springframework.jdbc.core.namedparam.MapSqlParameterSource;
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate;
import org.springframework.jdbc.core.namedparam.SqlParameterSource;
import org.springframework.stereotype.Repository;
import org.springframework.transaction.annotation.Transactional;

import com.example.app.domain.Customer;

@Repository
// 이러한 클래스를 DI컨테이너 에서 가져오고 해당 클래스에 속한 각 메소드를 다른 클래스에서 호출 하면 DB 트랜잭션이 자동으로 이루어 집니다.
// 메서드가 제대로 실행되면 DB 트랜잭션이 커밋됩니다.
// 실행 도중 오류가 발생하면 DB 트랜잭션이 롤백 됩니다.
// DI 컨테이너는 각 메소드 앞뒤에 처리를 추가한 클래스를 동적으로 생성합니다.
@Transactional 
public class CustomerRepository {
	@Autowired
	NamedParameterJdbcTemplate jdbcTemplate;
	
	// 템플릿을 이용하여 query() 메소드의 람다식을 이용하여 SQL 실행 결과를 자바 객체 리스트 형태로 가져옵니다.
	private static final RowMapper<Customer> customerRowMapper = (rs, i) -> {
		Integer id = rs.getInt("id");
		String firstName = rs.getString("first_name");
		String lastName = rs.getString("last_name");
		return new Customer(id, firstName, lastName);
	};
	
	// Select All Condition Block
	public List<Customer> findAll() {
		String sql = "SELECT id,first_name,last_name FROM customers ORDER BY id";
		
		// 리스트로 가져 옵니다. 이를 위해선 위 RowMapper 클래스의 query()를 구현 해야 합니다.
		// 위에선 람다식을 사용하여 구현 했습니다.
		List<Customer> customers = jdbcTemplate.query(
				sql, customerRowMapper);
		return customers;
	}
	
	// ID값을 기준으로 행을 가져 옵니다.
	// Select Condition Block
	public Customer findOne(Integer customerId) {
		String sql = "SELECT id,first_name,last_name FROM customers WHERE id=:id";
		
		// MapSqlParam... 클래스의 addValue() 메소드를 이용해 customerId 인자값을 기준으로 SELECT문을 수행합니다.
		SqlParameterSource param = new MapSqlParameterSource().addValue("id", customerId);
		return jdbcTemplate.queryForObject(sql, param, customerRowMapper);
	}
	
	// Update Block
	public Customer save(Customer customer) {
		String insert = "INSERT INTO customers(first_name, last_name) ";
		String values = "values(:firstName, :lastName)";
		
		String update = "UPDATE customer SET first_name=:firstName, last_name=:lastName WHERE id=:id";
		
		/*
		 * 업데이트 계열의 SQL을 실행하기 위해 템플릿의 Update() 메소드를 사용합니다.
		 * Customer 객체의 id필드가 null 이면 insert 문을 수행 합니다.
		 * 객체의 id필드가 null이 아니면 update 문을 수행 합니다.
		 */
		// BeanPropertySqlParameterSource를 사용하면 자바객체에 포함된 필드 이름과 
		// 값을 매핑한 SqlParameterSource가 자동으로 작성 됩니다.
		SqlParameterSource param = new BeanPropertySqlParameterSource(customer);
		if (customer.getId() == null) {
			jdbcTemplate.update(insert+values, param);
		} else {
			jdbcTemplate.update(update, param);
		}
		
		return customer;
	}
	
	// Delete Condition Block
	public void delete(Integer customerId) {
		String sql = "DELETE FROM customers WHERE id=:id";
		
		// delete문을 사용할 떄도 update문을 사용 합니다.
		SqlParameterSource param = new MapSqlParameterSource().addValue("id", customerId);
		jdbcTemplate.update(sql, param);
	}
}
```

- 검사 예와(Checked exception)가 발생한 경우에는 롤백(rollback)되지 않습니다(중요).
- 이제 App 클래스를 수정하겠습니다.

### App.java

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import com.example.app.domain.Customer;
import com.example.app.repository.CustomerRepository;

@SpringBootApplication
public class App implements CommandLineRunner{
	@Autowired
	CustomerRepository customerRepository;

	@Override
	public void run(String... args) throws Exception {
		// 데이터 추가
		Customer created = customerRepository.save(new Customer(null, "Hidetoshi", "Dekisugi"));
		System.out.println(created + " is created!");
		
		// 데이터 표시
		// forEach
		customerRepository.findAll().forEach(System.out::println);
	}
	
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}
}
```

- 위 처럼 CustomerRepository를 구현하게 된다면 Save() 메서드로 Customer 객체를 새로 생성했을 경우 DB쪽에서 생성한 ID가 설정되지 않고. Autoincrement 정책으로 인해 DB에서 자동으로 늘려줍니다.
- 리포지토리 기능면 에서 보면 새로 객체가 만들어 질때 ID도 생성해주는게 맞습니다.
- 이를 위해 CustomerRepository를 조금 수정 하겠습니다.

```java
package com.example.app.repository;

import java.util.List;

import javax.annotation.PostConstruct;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.core.namedparam.BeanPropertySqlParameterSource;
import org.springframework.jdbc.core.namedparam.MapSqlParameterSource;
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate;
import org.springframework.jdbc.core.namedparam.SqlParameterSource;
import org.springframework.jdbc.core.simple.SimpleJdbcInsert;
import org.springframework.stereotype.Repository;
import org.springframework.transaction.annotation.Transactional;

import com.example.app.domain.Customer;

@Repository
// 이러한 클래스를 DI컨테이너 에서 가져오고 해당 클래스에 속한 각 메소드를 다른 클래스에서 호출 하면 DB 트랜잭션이 자동으로 이루어 집니다.
// 메서드가 제대로 실행되면 DB 트랜잭션이 커밋됩니다.
// 실행 도중 오류가 발생하면 DB 트랜잭션이 롤백 됩니다.
// DI 컨테이너는 각 메소드 앞뒤에 처리를 추가한 클래스를 동적으로 생성합니다.
@Transactional 
public class CustomerRepository {
	@Autowired
	NamedParameterJdbcTemplate jdbcTemplate;
	SimpleJdbcInsert insert;
	
	
	// 클래스 객체가 생성 될 떄, 아래 어노테이션을 명시한 
	// 메소드는 별도로 우선 초기화 한다.
	// 이 어노테이션은 WAS에 띄어질떄 init가 객체생성 된다.
	@PostConstruct
	public void init() {
		insert = new SimpleJdbcInsert(
				// SompleJdbcInsert에 JdbcTemplate을 설정해야 하므로 NamedParameterJdbcTemplate에 속한
				// JdbcTemplate 객체를 받아옵니다.
				(JdbcTemplate) jdbcTemplate.getJdbcOperations())
				// SimpleJdbcInsert는 INSERT SQL을 자동으로 생성하므로 테이블 이름을 명시적으로 지정.
				.withTableName("customers")
				// 자동으로 번호가 매겨지는 기본 키 칼럼명을 지정합니다.
				.usingGeneratedKeyColumns("id");
	}
	
	// 템플릿을 이용하여 query() 메소드의 람다식을 이용하여 SQL 실행 결과를 자바 객체 리스트 형태로 가져옵니다.
	private static final RowMapper<Customer> customerRowMapper = (rs, i) -> {
		Integer id = rs.getInt("id");
		String firstName = rs.getString("first_name");
		String lastName = rs.getString("last_name");
		return new Customer(id, firstName, lastName);
	};
	
	// Select All Condition Block
	public List<Customer> findAll() {
		String sql = "SELECT id,first_name,last_name FROM customers ORDER BY id";
		
		// 리스트로 가져 옵니다. 이를 위해선 위 RowMapper 클래스의 query()를 구현 해야 합니다.
		// 위에선 람다식을 사용하여 구현 했습니다.
		List<Customer> customers = jdbcTemplate.query(
				sql, customerRowMapper);
		return customers;
	}
	
	// ID값을 기준으로 행을 가져 옵니다.
	// Select Condition Block
	public Customer findOne(Integer customerId) {
		String sql = "SELECT id,first_name,last_name FROM customers WHERE id=:id";
		
		// MapSqlParam... 클래스의 addValue() 메소드를 이용해 customerId 인자값을 기준으로 SELECT문을 수행합니다.
		SqlParameterSource param = new MapSqlParameterSource().addValue("id", customerId);
		return jdbcTemplate.queryForObject(sql, param, customerRowMapper);
	}
	
	// Update Block
	public Customer save(Customer customer) {
		String update = "UPDATE customer SET first_name=:firstName, last_name=:lastName WHERE id=:id";
		
		/*
		 * 업데이트 계열의 SQL을 실행하기 위해 템플릿의 Update() 메소드를 사용합니다.
		 * Customer 객체의 id필드가 null 이면 insert 문을 수행 합니다.
		 * 객체의 id필드가 null이 아니면 update 문을 수행 합니다.
		 */
		
		// BeanPropertySqlParameterSource를 사용하면 자바객체에 포함된 필드 이름과 
		// 값을 매핑한 SqlParameterSource가 자동으로 작성 됩니다.
		SqlParameterSource param = new BeanPropertySqlParameterSource(customer);
		if (customer.getId() == null) {
			// executeAndReturnKey() 메소드를 호출하면 SQL을 실행하고 자동으로 번호가 매겨진 ID가 반환됩니다.
			Number key = insert.executeAndReturnKey(param);
			customer.setId(key.intValue());
		} else {
			jdbcTemplate.update(update, param);
		}
		
		return customer;
	}
	
	// Delete Condition Block
	public void delete(Integer customerId) {
		String sql = "DELETE FROM customers WHERE id=:id";
		
		// delete문을 사용할 떄도 update문을 사용 합니다.
		SqlParameterSource param = new MapSqlParameterSource().addValue("id", customerId);
		jdbcTemplate.update(sql, param);
	}
}
```

- 위 수정된 CustomerRepository는 객체의 번호를 생성해 줍니다.
  - 주석을 읽어봐야 합니다!

# PART 2 에서 계속...