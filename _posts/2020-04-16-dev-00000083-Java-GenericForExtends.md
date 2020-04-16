---
date: 2020-04-16 22:00:00
layout: post
title: Java Generic And Extends
subtitle: Java Generic의 상속
description: "Current JDK : == JDK 1.8.0_231(64bit)"
image: ../assets/img/postsimg/R_main_00000003.png
optimized_image: ../assets/img/postsimg/R_op_00000003.png
category: coding
tags:
  - Eclipse
  - Java
  - Generic
  - Extends
author: moreGet
# image:760*400
# optimized_image:380*200
---

## 제네릭 의 상속

제네릭 형태의 클래스 를 상속 하는 방법에 대해 간략하게 알아보자.<br>

``` java
// 최상위 부모 클래스 Product<T, M>
public class Product<T, M> {
	
	private T kind; // 제네릭 타입변수
	private M model;
	
	// Getter, Setter
	public T getKind() {
		return kind;
	}
	public void setKind(T kind) {
		this.kind = kind;
	}
	public M getModel() {
		return model;
	}
	public void setModel(M model) {
		this.model = model;
	}
}

class Tv{} // 클래스 하나더 만듬

// 부모 클래스가 Product<T, M> 형태 이기 때문에 자식도 이 형태의 제네릭 
// 타입을 포함 해야 한다.
public class ChildProduct<T, M, C> extends Product<T, M> {

	// 자식 클래스의 별도의 제네릭 타입 변수
	private C company;

	// Getter, Setter
	public C getCompany() {
		return company;
	}

	public void setCompany(C company) {
		this.company = company;
	}
}

// Main클래스 에서 사용 해보기
public class Main {
	
	public static void main(String[] args) {
		
		// 자식 클래스의 인스턴스 생성
		ChildProduct<Tv, String, String> product = new ChildProduct<Tv, String, String>();
		
		// Tv 클래스 인스턴스를 생성해 Set
		product.setKind(new Tv());
		// String 형 매개값 전달
		product.setModel("Smart Tv");
		// String 형 매개값 전달
		product.setCompany("Samsung");
	}
}
```

간단한 개념 이지만 정확히 이해하고 지나가야 한다.<br>
위 자식클래스 가 제네릭 타입의 부모클래스 를 상속 받으려면 당연한 말이지만<br>
부모 클래스의 제네릭 타입까지 동일하게 자식 클래스 에도 선언 해주어야 한다.<br>
아래는 제네릭 인터페이스를 선언하고 구현 하는 예제 이다.<br>

```java
// 제네릭 인터페이스 구현
public interface Storage<T> {
	public void add(T item, int index);
	public T get(int index);
}

// 제네릭 인터페이스를 구현 함.
public class StorageImpl<T> implements Storage<T>{
	
	// 제네릭 형 배열
	private T[] array;
	
	public StorageImpl(int capacity) {
		// 타입 파라메터로 배열을 생성하려면 아래 양식대로 생성 해야 한다.
		this.array = (T[])(new Object[capacity]);
	}
	
	@Override
	public void add(T item, int index) {
		array[index] = item;
	}
	
	@Override
	public T get(int index) {
		return array[index];
	}
}

// 사용 해보기
public static void main(String[] args) {
	// 인스턴스 생성
	// Storage 인터페이스를 Impl이 구현 하고 있기 때문에 
	// 가능한 문법
	Storage<Tv> storage = new StorageImpl<Tv>(100);
	// 구현한 add 메소드에 인스턴스 와 int형 변수를 파라메터로 전달
	storage.add(new Tv(), 0);
	// 배열에 저장된 tv인스턴스중 0번을 꺼내옴
	Tv tv = storage.get(0);
}
```

위 제네릭 인터페이스를 상속받은 Impl도 같은 제네릭 형을 가지고 있어야 한다.<br>
이 처럼 기본 개념 이지만 정확하게 이해 하고 있어야 할 것 같다.<br>
다음엔 좀 더 심화적인 제네릭 에 대해 공부해야 겠다.