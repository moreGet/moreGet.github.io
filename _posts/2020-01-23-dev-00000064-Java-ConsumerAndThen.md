---
date: 2020-01-23 22:00:00
layout: post
title: Java AndThen, Compose 사용
subtitle: Consumer, Function의 AndThen과 Compose 사용
description: "Current JDK : == JDK 1.8.0_231(64bit)"
image: ../assets/img/postsimg/R_main_00000003.png
optimized_image: ../assets/img/postsimg/R_op_00000003.png
category: coding
tags:
  - Eclipse
  - Java
  - Lambda
  - interfate
  - AndThen
  - Consumer
author: thiagorossener
# image:760*400
# optimized_image:380*200
---

|   종류   |  함수적 인터페이스  | andThen() | compose() |
|:--------:|:-------------------:|:---------:|:---------:|
| Consumer | Consumer<T>         |     O     |           |
|          | BiConsumer<T, U>    |     O     |           |
|          | DoubleConsumer      |     O     |           |
|          | IntConsumer         |     O     |           |
|          | LongConsumer        |     O     |           |
| Function | Function<T, R>      |     O     |     O     |
|          | BiFunction<T, U, R> |     O     |           |
| Operator | BinaryOperator<T>   |     O     |           |
|          | DoubleUnaryOperator |     O     |     O     |
|          | IntUnaryOperator    |     O     |     O     |
|          | LongUnaryOperator   |     O     |     O     |

## ConsumerAndThen

```java
/*
 * Consumer의 순차적 연결
 */

import java.util.function.Consumer;

/*
 * 디폴트 및 정적 메소드는 추상 메소드가 아니기 때문에 함수적 인터페이스에 선언되어도 여전히 함수적 인터페이스의
 * 성질을 잃지 않는다(하나의 추상메소드 그리고 람다식 으로 익명구현 객체를 생성 가능한)
 * Consumer, Function, Operator 종류의 함수적 인터페이스는 andThen()과 compose() 디폴트 메소드를 가지고 잇다.
 * 함수적 인터페이스를 순차적으로 연결하고 첫번째 처리 결과를 두 번째 매개값으로ㅗ 제공해서 최종 결 과값을 얻을때 사용한다.
 */

public class ConsumerAndThenExam {

	public static void main(String[] args) {
		Consumer<Member> consumerA = m -> {
			System.out.println("consumerA : " + m.getName());
		};
		
		Consumer<Member> consumerB = m -> {
			System.out.println("consumerB : " + m.getId());
		};
		
		Consumer<Member> consumerAB = consumerA.andThen(consumerB);
		consumerAB.accept(new Member("홍길동", "hong", null));
```

## Function Compose, AndThen

```java
import java.util.function.Function;
import S20200110AndThen.Address;
import S20200110AndThen.Member;

/*
 * Function과 Operator 종류의 함수적 인터페이스는 먼저 실행한 함 수적 인터페이스의 결과를 다음 함수적 인터페이스의
 * 매개값으로 넘겨주고, 최종 처리 결과를 리턴한다.
 */
public class FunctionAndThenComposeExam {

	public static void main(String[] args) {
		Function<Member, Address> functionA; // A는 멤버와 어드레스 객체
		Function<Address, String> functionB; // B는 어드레스와 문자열 객체
		Function<Member, String> functionAB; // AB는 위 A와 B 두개를 Member와 문자열 객체로 받을 함수
		String city; // 문자열 반환값을 받을 변수

		functionA = m -> m.getAddress(); // A는 address를 리턴받게끔 인터페이스 구현
		functionB = a -> a.getCity(); // city를 리턴받게끔 인터페이스 구현

		functionAB = functionA.andThen(functionB); // A의 값을 B로 넘긴후 B에서 최종처리
		city = functionAB.apply(new Member("홍길동", "hong", new Address("한국", "서울"))); // A객체에서 address를 받았고 이를 받은 B는 city를 리턴함
		System.out.println("거주 도시 : " + city); // B에서 최종처리 한결과는 문자열로 city를 반환받음

		functionAB = functionB.compose(functionA); // A의 값을 B로 넘긴후 B에서 최종처리
		city = functionAB.apply(new Member("홍길동", "hong", new Address("한국", "서울"))); //
		System.out.println("거주 도시 : " + city);
		
		/*
		 * andThen은 A.andThen(B) 가 A -> B -> B 최종처리
		 * compose는 A.compose(B) 가 B -> A -> A 최종처리
		 * 서로 반대로 처리한다.
		 */
	}
}
```

## Member, Address Class

```java
// member 클래스
public class Member {

	private String name;
	private String id;
	private Address address;

	public Member(String name, String id, Address address) {
		this.name = name;
		this.id = id;
		this.address = address;
	}

	public String getName() {
		return name;
	}

	public String getId() {
		return id;
	}

	public Address getAddress() {
		return address;
	}
}

// address 클래스
public class Address {
	private String country;
	private String city;

	public Address(String country, String city) {
		this.country = country;
		this.city = city;
	}

	public String getCountry() {
		return country;
	}

	public String getCity() {
		return city;
	}
}
```