---
date: 2020-01-05 22:00:00
layout: post
title: Java Annotation 사용
subtitle: Annotation
description: "Current JDK : == JDK 1.8.0_231(64bit)"
image: ../assets/img/postsimg/R_main_00000003.png
optimized_image: ../assets/img/postsimg/R_op_00000003.png
category: coding
tags:
  - Eclipse
  - 자료구조
  - Collection
  - 알고리즘
  - 컬렉션
  - Annotation
author: thiagorossener
# image:760*400
# optimized_image:380*200
---

## Annotation
- 어노테이션 기능
- 컴파일러에게 코드 문법 에러를 체크하도록 정보를 제공
- 소프트웨어 개발 툴이 빌드나 배치 시 코드를 자동으로 생성할 수 있도록 정보를 제공
- 실행 시(런타임 시) 특정 기능을 실행하도록 정보를 제공
<br>

| ElementType 열거 상수 | 적용 대상                     |
|-----------------------|-------------------------------|
| TYPE                  | 클래스, 인터페이스, 열거 타입 |
| ANNOTATION_TYPE       | 어노테이션                    |
| FIELD                 | 필드                          |
| CONSTRUCTOR           | 생성자                        |
| METHOD                | 메소드                        |
| LOCAL_VARIABLE        | 로컬 변수                     |
| PACKAGE               | 패키지                        |

<br>

| RetentionPolicy 열거상수 | 설명                                                                                                                    |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------|
| SOURCE                   | 소스상에서만 어노테이션 정보를 유지한다. 소스 코드를 분석할 때만<br>의미가 있으며 바이트코드 파일에는 정보가 남지 않는다. |
| CLASS                    | 바이트 코드 파일까지 어노테이션 정보를 유지한다.<br>하지만 리플렉션을 이용해서어노테이션 정보를 얻을 수는 없다.           |
| RUNTIME                  | 바이트 코드 파일까지 어노테이션 정보를 유지하면서<br>리플렉션을 이용해서 런타임 시에 어노테이션 정보를 얻을 수 있다.       |

<br>

## 런타임 시 어노테이션 정보 사용하기 
<br>

| 리턴 타입     | 메소드명(매개변수)   | 설명                                  |
|---------------|----------------------|---------------------------------------|
| Field[]       | getFields()          | 필드 정보를 Field 배열로 리턴         |
| Constructor[] | getConstructors()    | 생성자 정보를 Constructor 배열로 리턴 |
| Method[]      | getDeclaredMethods() | 메소드 정보를 Method 배열로 리턴      |

<br>

## 위 메소드로 어노테이션 타입을 반환 받은후 아래 메소드를 통해 목적에 맞는 메소드를 사용해야한다.
<br>

| 리턴 타입    | 메소드명(매개변수)                                                                                                                                          |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| boolean      | isAnnotationPresent(Class<? extnds Annotation> annotationClass)<br>Class에서 호출했을 때 상위 클래스에 정요된 경우에도 true를 리턴한다.                        |
| Annotation   | getAnnotation(Class<T> annotationClass)<br>지정한 어노테이션이 적용되어 있으면 어노테이션을 리턴하고 그렇지 않다면 null을 리턴한다. 상위클래스 적용시에도 리턴 |
| Annotation[] | getAnnotations()<br>적용된 모든 어노테이션 리턴, 상위클래스 적용된 어노테이션도 모두 리턴 없으면 0인 배열 리턴                                                 |
| Annotation[] | getDeclaredAnnotations()<br>직접 적용된 모든 어노테이션을 리턴. 상위클래스 어노테이션 포함안함                                                                 |
<br>

## import Package
> import java.lang.reflect.Method; <br>

```java
public class AnnotationExam {

	// *** invoke(적용 하다.)
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//Service 클래스로부터 메소드 정보를 얻음
		Method[] declaredMethods = Service.class.getDeclaredMethods();
		
		//Method 객체를 하나씩 처리
		for (Method method : declaredMethods) {
			//PrintAnnotation이 적용되었는지 확인
			if(method.isAnnotationPresent(PrintAnnotation.class)) {
				
				//PrintAnnotation 객체 얻기
				PrintAnnotation printAnnotation = method.getAnnotation(PrintAnnotation.class);
				
				//메소드 이름 출력
				System.out.println("[" + method.getName() + "] ");
				
				//구분선 출력
				for (int i = 0; i < printAnnotation.number(); i++) {
					System.out.print(printAnnotation.value());
					
				}
				System.out.println();
				
				try {
					//메소드 호출
					method.invoke(new Service()); //method 안에 어노테이션 정책을 적용시킴
				} catch (Exception e) {
					System.out.println();
				}
			}
		}
	}
}
```

## Annotation 구현 부분
> import java.lang.annotation.ElementType; <br>
> import java.lang.annotation.Retention; <br>
> import java.lang.annotation.RetentionPolicy; <br>
> import java.lang.annotation.Target; <br>

```java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)

public @interface PrintAnnotation {
	String value() default "-";
	int number() default 15;
}
```

## Service Class

```java
public class Service {

	@PrintAnnotation
	public void method1() {
		System.out.println("실행 내용 1");
	}
	
	@PrintAnnotation("*")
	public void method2() {
		System.out.println("실행 내용 2");
	}
	
	@PrintAnnotation(value = "#", number = 20)
	public void method3() {
		System.out.println("실행 내용 3");
	}
}
```