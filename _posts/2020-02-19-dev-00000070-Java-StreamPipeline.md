---
date: 2020-02-19 22:00:00
layout: post
title: Java 다양한 pipeLine 예제
subtitle: flatMap, Filtering, Aggregate, Match, Optional
description: "Current JDK : == JDK 1.8.0_231(64bit)"
image: ../assets/img/postsimg/R_main_00000003.png
optimized_image: ../assets/img/postsimg/R_op_00000003.png
category: coding
tags:
  - Eclipse
  - Java
  - Lambda
  - interfate
  - Stream
  - flatMap
  - Filtering
  - Aggregate
  - Match
  - Optional
author: thiagorossener
# image:760*400
# optimized_image:380*200
---

## 여러가지 Stream PipeLine 예제

- asDoubleStream()

```java
public class AsDoubleStreamAndBoxedExam {

	public static void main(String[] args) {
		int[] intArray = { 1, 2, 3, 4, 5 };
		
		IntStream intStream = Arrays.stream(intArray); // int형 배열로부터 스트림을 얻어옴
		intStream
		.asDoubleStream() // DoubleStream 생성
		.forEach(System.out :: println); // 출력
		
		System.out.println();
		
		intStream = Arrays.stream(intArray); // 새로 스트림을 생성 하고
		intStream 
		.boxed() // Stream<Integer> 생성
		.forEach(System.out :: println); // 출력
	}
}
```

<hr>

- distinct() : 중복제거
- filter(n -> n.startsWith("신")) // 첫 글자가 "신" 인것 제거

```java
/**
 * @author dkdlw
 * 필터링은 중간 처리 기능이다. 요소를 걸러낸다.
 * distinct()는 oldValue.equels(newValue) 로 true 이면 제거
 * filter()는 인자값 Predicate가 true를 리턴하는 요소만 필터링 한다.  
 */

public class FilteringExam {
	
	public static void main(String[] args) {
		List<String> names = Arrays.asList(
				"홍길동",
				"신용권",
				"감자바",
				"신용권",
				"신민철");
		
		names.parallelStream()
		.distinct() // 중복 제거
		.forEach(System.out :: println);
		
		System.out.println();
		
		names.parallelStream()
		.filter(n -> n.startsWith("신")) // 첫 글자가 "신" 인것 걸러옴
		.forEach( System.out :: println );
		
		System.out.println();
		
		// 다중 필터
		names.parallelStream()
		.distinct() // 중복제거
		.filter( n -> n.startsWith("신") ) // 신인것만 가져옴
		.forEach( System.out :: println );
	}
}
```

<hr>

- flatMapToInt() : 인자 값을 전수형으로 반환

```java
/**
 * @author dkdlw
 * flatMapXXX() 메소드는 중간처리 기능을 하는 매핑 메소드 이다.
 * 한 요소를 대체하는 복수개의 새로운 스트림을 리턴한다.
 * A와 B 라는 요소가 인자로 주어지면 A1, A2 / B1, B2 요소를 가지는 새로운 스트림이 생성된다.
 */
public class FlatMapExam {

	public static void main(String[] args) {
		List<String> inputList1 = Arrays.asList(
				"java8 lambda",
				"stream mapping"
				);
		
		inputList1.parallelStream() // 병렬 처리 스트림
		.flatMap(data -> Arrays.stream(data.split(" "))) // 공백을 기준으로 문자열 분리
		.forEach(System.out :: println); // 문자열 출력
		
		System.out.println();
		
		List<String> inputList2 = Arrays.asList("10, 20, 30, 40, 50, 60");
		inputList2.parallelStream() // 병렬처리 스트림
		.flatMapToInt(data -> { // 중간처리 맵핑 메소드 문자열 list를 정수형 스트림으로 반환 하기.
			String[] strArr = data.split(",");  // "," 마다 잘라서 문자열 배열에 넣기
			
			// strArr에 있는 문자열 요소들을 정수형으로 맵핑 하여 정수형 배열에 넣기 위해 같은 크기로 선언
			int[] intArr = new int[strArr.length]; 
			
			// for문으로 문자열 요소를 정수형 배열에 대입
			for (int i = 0; i < strArr.length; i++) {
				intArr[i] = Integer.parseInt(strArr[i].trim()); // trim() 공백제거
			}
			
			return Arrays.stream(intArr); // 완성된 정수형 배열을 스트림 형으로 반환시킴
		}).forEach(System.out :: println); // 출력.
	}
}
```

<hr>

```java
public class MapExam {

	public static void main(String[] args) {
		List<Student> stuList = Arrays.asList(
				new Student("홍길동", 10),
				new Student("신용권", 20),
				new Student("유미선", 30));
		
		stuList.parallelStream() // stu 객체 스트림 받아옴
		.mapToInt(Student :: getCode) // Student 클래스 내에 getCode 함수로 code값 리턴 받음
		.forEach(System.out :: println); // 출력 시킴
	}
}
```

<hr>

```java
public class SortingExam {

	// Sorted() 메소드 이용 하여 정렬하기
	// 객체 요소 일 경우 클래스가 Comparable를 구현하고 있어야 한다.
	public static void main(String[] args) {
		IntStream intStream = Arrays.stream(new int[] {5, 3, 2, 1, 4});
		intStream
		.sorted()
		.forEach(System.out :: print);

		System.out.println();
		
		// 객체요소
		List<Student2> stuLists = Arrays.asList( // Stu객체 리스트 반환
				new Student2("홍길동", 30),
				new Student2("신용권", 10),
				new Student2("유미선", 20));
		
		stuLists.stream() // 스트림 받아옴
		.sorted() // score 기준으로 오름차순 정렬
		.forEach(s -> System.out.print(s.getScore() + ",")); // 출력
		System.out.println();
		
		stuLists.stream()
		.sorted(Comparator.reverseOrder()) // score기준으로 내림차순 정렬(객체를 정렬시킴)
		.forEach(s -> System.out.print(s.getScore() + ","));
	}
}
```

<hr>

```java
public class LoopingExam {

	// 루핑 peek(), forEach() 가 있다.
	// 둘은 루핑한다는 기능은 동일하지만 peek()는 중간처리 이고 forEach()는 최종처리 이므로
	// 최종처리 단계에 따른 peek() 호출후 sum()이나 최종처리 메소드가 꼭 존재하여야 한다.
	// 반대로 forEach()는 최종처리 메소드 이기 때문에 후에 메소드가 나오면 안된다.
	public static void main(String[] args) {
		int[] intArr = {1, 2, 3, 4, 5};
		
		System.out.println("[peek()를 마지막에 호출한 경우]");
		Arrays.stream(intArr)
		.filter(a -> a%2 == 0)
		.peek(System.out :: println);
		
		System.out.println("[최종 처리 메소드를 마지막에 호출한 경우]");
		int total = Arrays.stream(intArr)
		.filter(a -> a%2 == 0)
		.peek(n -> System.out.println(n))
		.sum();
		System.out.println("총합 : " + total);
		
		System.out.println("[forEach()를 마지막에 호출한 경우");
		Arrays.stream(intArr)
		.filter(a -> a%2==0)
		.forEach(System.out :: println);
	}
}
```

<hr>

```java
public class AggregateExam {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int[] intArrays = new int[] {1, 2, 3, 4, 5};
		
		long count = Arrays.stream(intArrays)
				.filter(n -> n%2==0)
				.count();
		System.out.println("2의 배수 개수 : " + count);
		
		long sum = Arrays.stream(intArrays)
				.filter(n -> n%2==0)
				.sum();
		System.out.println("2의 배수의 합 : " + sum);
		
		double avg = Arrays.stream(intArrays)
				.filter(n -> n%2==0)
				.average()
				.getAsDouble();
		System.out.println("2의 배수의 평균 : " + avg);
		
		int max = Arrays.stream(intArrays)
				.filter(n -> n%2==0)
				.max()
				.getAsInt();
		System.out.println("최댓값 : " + max);
		
		int min = Arrays.stream(intArrays)
				.filter(n -> n%2==0)
				.min()
				.getAsInt();
		System.out.println("최솟값  : " + min);
		
		int first = Arrays.stream(intArrays)
				.filter(n -> n%3==0)
				.findFirst()
				.getAsInt();
		System.out.println("첫번째 3의 배수 : " + first);
	}
}
```

<hr>

```java
public class MatchExam {

	public static void main(String[] args) {
		int[] intArr = { 2, 4, 6 };
		
		boolean result = Arrays.stream(intArr)
				.allMatch(a -> a%2==0);
		System.out.println("모두 2의 배수인가? " + result);
		
		result = Arrays.stream(intArr)
				.anyMatch( a -> a%3==0);
		System.out.println("하나라도 3의 배수가 있는가? " + result);
		
		result = Arrays.stream(intArr)
				.noneMatch(a -> a%3==0);
		System.out.println("3의 배수가 없는가? " + result);
	}
}
```

<hr>

```java
public class OptionalExam {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		List<Integer> list = new ArrayList<Integer>();
		
		// 예외발생
		// list에 값이 존재 하지 않기 때문에 평균을 구할 수가 없다.
//		double avg = list.parallelStream()
//				.mapToInt(Integer :: intValue)
//				.average()
//				.getAsDouble();
		
		// 방법1
		OptionalDouble opDouble = list.parallelStream()
				.mapToInt(Integer :: intValue)
				.average();
		if( opDouble.isPresent() ) {
			System.out.println("방법1_평균 : " + opDouble.getAsDouble());
		} else {
			System.out.println("방법1_평균값을 구할 수 없을때 : " + 0.0);
		}
				
		// 방법2
		double avg = list.parallelStream()
				.mapToInt(Integer :: intValue)
				.average()
				.orElse(0.0);
		System.out.println("방법2_평균 : " + avg);
		
		// 방법3
		list.parallelStream()
		.mapToInt(Integer :: intValue)
		.average()
		.ifPresent(s -> System.out.println("방법3_평균 : " + s));
	}
}
```

<hr>

```java
public class ReductionExam {
	
	// 커스텀 집계
	public static void main(String[] args) {
		List<Student> stuLists = Arrays.asList(
				new Student("홍", 92),
				new Student("김", 95),
				new Student("신", 88));
		
		int sum1 = stuLists.parallelStream()
				.mapToInt(Student :: getScore)
				.sum(); // sum 함수
		
		int sum2 = stuLists.parallelStream()
				.map(Student :: getScore)
				.reduce((a, b) -> a+b)
				.get(); // reduce로 커스텀 함수
		
		int sum3 = stuLists.parallelStream()
				.map(Student :: getScore)
				.reduce(0, (a, b) -> a+b); // 인자값 중 defalt값 추가
		
		System.out.println("sum1 : " + sum1);
		System.out.println("sum2 : " + sum2);
		System.out.println("sum3 : " + sum3);
	}
}
```

<hr>