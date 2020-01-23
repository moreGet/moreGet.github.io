---
date: 2020-01-23 22:00:00
layout: post
title: Java Consumer AndThen
subtitle: Consumer의 AndThen 메소드 사용
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