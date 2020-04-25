---
date: 2020-04-25 22:00:00
layout: post
title: Java BuilderPattern
subtitle: Java BuilderPattern의 활용
description: "Current JDK : == JDK 1.8.0_231(64bit)"
image: ../assets/img/postsimg/R_main_00000003.png
optimized_image: ../assets/img/postsimg/R_op_00000003.png
category: coding
tags:
  - Eclipse
  - Java
  - Builder
  - Pattern
author: moreGet
# image:760*400
# optimized_image:380*200
---

## Effective Java의 빌더 패턴

업무를 하면서 알고리즘, 자료구조의 탄탄한 기초지식 이 정말 중요하다는 느낌을 받는다.<br>
프로젝트를 진행 하면서 스프린트 백로그에서 개발 업무가 하나하나 지워지고<br>
고도화 작업에 들어가면 들어갈수록 알고리즘의 비중이 점점 커지는 것 같다.<br>
~~기업 공채 볼때만 필요한 줄 알았는데... 기사시험 때나...~~<br>

## 그! 래! 서!

여러가지 패턴에 대해 공부하고 탄탄한 기본 실력을 쌓기위해 노력 해야 겠다.<br>
실무에서 자주 마주치는 BuilderPattern에 대해 사실 Spring 환경에서는 어노테이션 으로<br>
명시 후 아 이게 이런거구나~ 라는 것만 인지하고 좋다좋다 하며 사용을 하는데...<br>
사실 제대로 알고써야 제대로 효율이 나올 거라 생각 한다. 아래는 Builder 패턴에 대한 자료이다.<br>

> 생성자 인자가 많을 때는 Builder 패턴 적용을 고려하라.<br>
> 생성자에 배개변수가 많다면 빌더를 고려하라.

## 깔끔한 객체 생성을 위한 패턴 3가지

1. 점층적 생성자 패턴
2. 점층적 생성자 패턴의 대안 자바빈 패턴
3. 자바빈 패턴의 대안 빌더 패턴
   
---

## 점층적 생성자 패턴

- 점층적 생성자 패턴을 만드는 방법
  - 필수 인자를 받는 필수 생성자를 하나 만든다
  - 1 개의 선택적 인자를 받는 생성자를 추가한다.
  - 2 개의 선택적 인자를 받는 생성자를 추가한다.
  - .... 반복
  - 모든 선택적 인자를 다 받는 생성자를 추가한다.

```java
public class Member {

	private final String name; // 필수 인자
	private final String location; // 선택적 인자
	private final String hobby; // 선택적 인자
	
	public Member(String name) {	
		this(name, "출신지역 비공개", "취미 비공개");
	}
	
	public Member(String name, String location) {
		this(name, location, "취미 비공개");
	}

	public Member(String name, String location, String hobby) {
		this.name = name;
		this.location = location;
		this.hobby = hobby;
	}
}
```

- 장점
  - new Member("홍길동", "출신지역 비공개", "취미 비공개") 같은 호출이 빈번하게 일어난다면, new Member("홍길동")로 대체할 수 있다.
- 단점
  - 다른 생성자를 호출하는 생성자가 많으므로, 인자가 추가되는 일이 발생하면 코드를 수정하기 어렵다.
  - 코드 가독성이 떨어진다.
    - 특히 인자수가 많을때 호출 코드만 봐서는 의미를 알기 어렵다.

```java
// 호출 코드만으로는 각 인자의 의미를 알기 어렵다.
NutritionFacts cocaCola = new NutritionFacts(240, 8, 100, 3, 35, 27);
NutritionFacts pepsiCola = new NutritionFacts(220, 10, 110, 4, 30);
NutritionFacts mountainDew = new NutritionFacts(230, 10);
```

---

## 자바빈 패턴

따라서 이에 대한 대안으로 자바빈 패턴(JavaBeans Pattern)을 소개한다.<br>
이 패턴은 **setter**메서드를 이용해 생성 코드를 읽기 좋게 만드는 것이다.

```java
NutritionFacts cocaCola = new NutritionFacts();
cocaCola.setServingSize(240);
cocaCola.setServings(8);
cocaCola.setCalories(100);
cocaCola.setSodium(35);
cocaCola.setCarbohdydrate(27);
```

- 장점
  - 이제 각 인자의 의미를 파악하기 쉬워졌다.
  - 복잡하게 여러 개의 생성자를 만들지 않아도 된다.
- 단점
  - 객체 일관성(consistency)이 깨진다.
    - 1회의 호출로 객체 생성이 끝나지 않았다.
    - 즉 한 번에 생성하지 않고 생성한 객체에 값을 떡칠하고 있다.
  - **setter** 메소드가 있으므로 변경 불가능(immutable)클래스를 만들 수가 없다.
    - 스레드 안전성을 확보하려면 점층적 생성자 패턴보다 많은 일을 해야 한다.

---

## 빌더 패턴(Effcetive Java 스타일)

```java
//Effective Java의 Builder Pattern
public class NutririonFacts {
	private final int servingSize;
	private final int servings;
	private final int calories;
	private final int fat;
	private final int sodium;
	private final int carbohydrate;
	
	public static class Builder {
		// Required parameters(필수 인자)
		private final int servingSize;
		private final int servings;
		
		// Optional parameters - initialized to default values(선택적 인자는 기본값으로 초기화)
		private int calories	  = 0;
		private int fat			  = 0;
		private int carbohydrate  = 0;
		private int sodium 	      = 0;
		
		public Builder(int servingSize, int servings) {
			this.servingSize = servingSize;
			this.servings	 = servings;
		}
		
		public Builder calories(int val) {
			calories = val;
			return this; // 이렇게 하면 . 으로 체인을 이어갈 수 있다.
		}
		
		public Builder fat(int val) {
			fat = val;
			return this;
		}
		
		public Builder carbohydrate(int val) {
			carbohydrate = val;
			return this;
		}
		
		public Builder sodium(int val) {
			sodium = val;
			return this;
		}
		
		public NutririonFacts build() {
			return new NutririonFacts(this);
		}
	}
	
	private NutririonFacts(Builder builder) {
		servingSize	 = builder.servingSize;
		servings	 = builder.servings;
		calories	 = builder.calories;
		fat			 = builder.fat;
		sodium		 = builder.sodium;
		carbohydrate = builder.carbohydrate;
	}
}
```

위와 같이 하면 다음과 같이 객체를 생성할 수 있다.
```java
NutritionFacts.Builder builder = new NutritionFacts.Builder(240, 8);
builder.calories(100);
builder.sodium(35);
builder.carbohydrate(27);
NutritionFacts cocaCola = builder.build();
```

또는 다음과 같이 사용할 수도 있다.
```java
// 각 줄마다 builder를 타이핑하지 않아도 되어 편리하다.
NutritionFacts cocaCola = new NutritionFacts
    .Builder(240, 8)    // 필수값 입력
    .calories(100)
    .sodium(35)
    .carbohydrate(27)
    .build();           // build() 가 객체를 생성해 돌려준다.
```

## 장점

- 각 인자가 어떤 의미인지 알기 쉽다.
- setter 메소드가 없으므로 변경 불가능 객체를 만들 수 있다.
- 한 번에 객체를 생성하므로 객체 일관성이 깨지지 않는다.
- build() 함수가 잘못된 값이 입력 되었는지 검증하게 할 수도 있다.

## 객체 생성을 유연하게

빌더 패턴을 사용하면 하나의 빌더 객체로 여러 객체를 만드는 것도 가능하다.<br>

> 인자가 설정된 빌더는 훌륭한 추상적 팩토리다.<br><br>

위의 인용구는 이펙티브 자바의 저자인 조슈아 블로흐가 GoF 책의 빌더 패턴 부분을 인용한 것이다.

```java
public interface Builder<T> {
    public T build();
}
```

위와 같은 인터페이스를 만들고, 빌더 클래스가 **implements** 하게 하면 된다.

- 참고 블로그 : https://johngrib.github.io/wiki/builder-pattern/