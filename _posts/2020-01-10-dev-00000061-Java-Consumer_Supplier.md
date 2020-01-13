---
date: 2020-01-10 22:00:00
layout: post
title: Java 함수적 인터페이스 Consumer, Supplier
subtitle: 함수적 인터페이스의 사용
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
  - Lambda
  - interfate
  - 함수적
author: thiagorossener
# image:760*400
# optimized_image:380*200
---

| 종류      | 특징                                                                       | 설명                           |
|-----------|----------------------------------------------------------------------------|--------------------------------|
| Consumer  | 매개값은 있고, 리턴값은 없음                                               | 매개값 -> Consumer             |
| Supplier  | 매개값은 없고, 리턴값은 있음                                               | Supplier -> 리턴값             |
| Function  | 매개값도 있고, 리턴값도 있음<br> 주로 매개값을 리턴값으로 매핑(타입 변환)  | 매개값 -> Function -> 리턴값   |
| Operator  | 매개값도 있고, 리턴값도 있음<br> 주로 매개값을 연산하고 결과를 리턴        | 매개값 -> Operator -> 리턴값   |
| Predicate | 매개값은 있고, 리턴 타입은 boolean<br> 매개값을 조사해서 true/false를 리턴 | 매개값 -> Predicate -> boolean |

<br>

| 인터페이스명         | 추상 메소드                    | 설명                           |
|----------------------|--------------------------------|--------------------------------|
| Consumer<T>          | void accept(T t)               | 객체 T를 받아 소비             |
| BiConsumer<T, U>     | void accept(T t, U u)          | 객체 T와 U를 받아 소비         |
| DoubleConsumer       | void accept(double value)      | double 값을 받아 소비          |
| IntConsumer          | void accept(int value)         | int값을 받아 소비              |
| LongConsumer         | void accept(long value)        | long 값을 받아 소비            |
| ObjDoubleConsumer<T> | void accept(T t, double value) | 객체 T와 double 값을 받아 소비 |
| ObjIntConsumer<T>    | void accept(T t, int value)    | 객체 T와 int 값을 받아 소비    |
| ObjLongConsumer<T>   | void accept(T t, long value)   | 객체 T와 long 값을 받아 소비   |

## 함수적 인터페이스 Consumer

- 자바에서 제공되는 표준 API에서 한 개의 추상 메소드를 가지는 인터페이스들은 모두 람다식 이용 가능
- 자바8부터는 빈번하게 사용되는 함수적 인터페이스는 표준 API 패키지로 제공
- 자바8부터 추가되거나 변경된 API에서 이 함 수적 인터페이스들을 매개 타입으로 많이 사용한다.

```java
// Consumer 인터페이스
import java.util.function.BiConsumer;
import java.util.function.Consumer;
import java.util.function.DoubleConsumer;
import java.util.function.ObjIntConsumer;

public class ConsumerExam {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Consumer<String> consumer = str -> System.out.println(str + " 8");
		consumer.accept("Java"); // accept 함수 구현
		
		BiConsumer<String, Integer> biConsumer = (str, num) -> System.out.println(str + " " + num);
		biConsumer.accept("Java", 8);
		
		DoubleConsumer doubleConsumer = d -> System.out.println("Java" + " " + d);
		doubleConsumer.accept(8.0);
		
		ObjIntConsumer<String> objIntConsumer = (t, i) -> System.out.println(t + " " + i);
		objIntConsumer.accept("Java", 8);
	}
}
```

## 함수적 인터페이스 Supplier

| 인터페이스명         | 추상 메소드                    | 설명                           |
|----------------------|--------------------------------|--------------------------------|
| Supplier<T>          | T.get()                        | T 객체를 리턴                  |
| BooleanSupplier      | boolean getAsBoolean()         | boolean 값을 리턴              |
| DoubleSupplier       | double getAsDouble()           | double 값을 리턴               |
| IntSupplier          | int getAsInt()                 | int 값을 리턴                  |
| LongSupplier         | long getAsLong()               | long 값을 리턴                 |

```java
import java.util.function.IntSupplier;

public class SupplierExam {
	
	public static void main(String[] args) {
		IntSupplier intSupplier = () -> {
			int num = (int) (Math.random() * 6) + 1;
			return num;
		}; // 람다식
		
		int num = intSupplier.getAsInt();
		System.out.println("눈의 수 : " + num);
	}
}
```