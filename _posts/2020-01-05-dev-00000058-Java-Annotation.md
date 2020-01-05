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

```java
// 예시 구현
public class AbstractExam {
	// 필드
	private String owner;
	
	// 생성자
	publlic AbstractExam(String owner) {
		this.owner = owner;
	}

	// 메소드
	public String getOwner() {
		return owner;
	}

	public void setOwner(String owner) {
		this.owner = owner;
	}
}

// 상속 받을때
public class AbstractV2Exam extends AbstractExam{

	// 내용 ...
}
```

## 추상 Class
- new 예약어를 사용하지 못한다<br>
- 생성자가 반드시 존재 해야한다<br>
- 말그대로 추상적인 클래스이다. 실체 클래스를 만들기 위해 부모 클래스로만 사용된다.<br>
- 실제로 구현될 클래스들의 공통된 필드와 메소드의 이름을 통일할 목적으로 사용<br>
- 실체 클래스를 작성할 때 시간을 절약한다.

```java
// 예시 구현
public abstract class AbstractExam {
	// 필드
	private String owner;
	
	// 생성자(반드시 존재 하여아함)
	public AbstractExam(String owner) {
		this.owner = owner;
	}

	// 메소드
	public String getOwner() {
		return owner;
	}

	public void setOwner(String owner) {
		this.owner = owner;
	}
}

// 상속 받을때
public class AbstractV2Exam extends AbstractExam{

	// 내용 ...
}
```

## 구현 implements
- 상속을 받는다는 것은 일반 상속이랑 다를것이 없지만 인터페이스 class를 상속받는다.<br>
- 인터페이스 class는 정의한 메소드를 구현하지 않아도 된다.<br>
- 다중상속을 허용하지 않는 JAVA에서의 대비책이다.<br>
- 개발 코드와 객체가 서로 통신하는 접점 역할을 한다.
- 개발 코드를 수정하지 않고 사용하는 개체를 변경할 수 있도록 하기 위해 사용한다.
- 객체에 따라 내용과 리턴값이 달라진다.

```java
// 예시 구현
public interface AbstractExam {
	// 상수
	// 변수 는 생성 불가, 상수만 가능.
	// 타입 상수명 = 값;
	public int TEST_VALUE = 10;
	public String TEST_STR_VALUE = "TEST";
	
	// 추상 메소드 {}가 없다면 하위 클래스에서 반드시 구현해야함
	public int getValue();
	public void setValue(int value);

	// 디폴트 메소드
	default void chkStrValue(String strTemp) {
		if(strTemp.equals("TEST")) {
			System.out.println("TEST 입니다.");
		} else {
			System.out.println("TEST 아닙니다.");
		}
	}

	// 정적 메소드
	static void showStrValue() {
		System.out.println(TEST_STR_VALUE);
	}
}
```