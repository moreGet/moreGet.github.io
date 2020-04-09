---
date: 2020-04-09 22:00:00
layout: post
title: Java Grouping And Reduction
subtitle: Java Stream 활용
description: "Current JDK : == JDK 1.8.0_231(64bit)"
image: ../assets/img/postsimg/R_main_00000003.png
optimized_image: ../assets/img/postsimg/R_op_00000003.png
category: coding
tags:
  - Eclipse
  - Java
  - Stream
  - Grouping
  - Reduction
  - Filtering
author: thiagorossener
# image:760*400
# optimized_image:380*200
---

## Stream 을 활용한 Grouping And Reduction 예제 01

| 리턴 타입                          | 메소드(매개 변수)                                                                             | 설명                                                    |
|------------------------------------|-----------------------------------------------------------------------------------------------|---------------------------------------------------------|
| Collector<T, ?, R>                 | mapping(Function<T,U> mapper, Collector<U,A,R> collector)                                     | T를 U로 매핑한 후, U를 R에 수집                         |
| Collector<T, ?, Double>            | averagingDouble(ToDouleFunction<T> mapper)                                                    | T를 Double로 매핑한 후, Double의 평균값을 산출          |
| Collector<T, ?, Long>              | counting()                                                                                    | T의 카운팅 수를 산출                                    |
| Collector<CharSequence, ?, String> | joining(CharSequence delimiter)                                                               | CharSequecne를 구분자(delimiter)로 연결한 String를 산출 |
| Collector<T, ?, Optional<T>>       | maxBy(Comparator<T> comparator)                                                               | Comparator를 이용해서 최대 T를 산출                     |
| Collector<T, ?, Optional<T>>       | minBy(Comparator<T> comparator)                                                               | Comparator를 이용해서 최소 T를 산출                     |
| Collector<T, ?, Integer>           | summingInt(ToIntFunction)<br> summingLong(ToLongFunction)<br> summingDouble(ToDoubleFunction) | Int, Long, Double 타입의 합계 산출                      |

```java
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

import S20200123CustomCollect.Student;

public class GroupingAndReductionExam {

	public static void main(String[] args) {
		List<Student> totalList = Arrays.asList(
				new Student("홍길동", 10, Student.Sex.MALE),
				new Student("김수애", 6, Student.Sex.FEMALE),
				new Student("신용권", 10, Student.Sex.MALE),
				new Student("박수미", 6, Student.Sex.FEMALE));

		// 성별로 평균 점수를 저장하는 Map 얻기
		Map<Student.Sex, Double> mapBySex = totalList.stream()
				.collect(Collectors.groupingBy(
						Student :: getSex, 
						Collectors.averagingDouble(Student :: getScore)));
		
		System.out.println("남학생 평균 점수 : " + mapBySex.get(Student.Sex.MALE));
		System.out.println("여학생 평균 점수 : " + mapBySex.get(Student.Sex.FEMALE));
		
		// 성별을 쉼표로 구분한 이름을 저장하는 Map 얻기
		Map<Student.Sex, String> mapByName = totalList.stream()
				.collect(Collectors.groupingBy(
						Student :: getSex,
						Collectors.mapping(Student :: getName, Collectors.joining(","))));
		
		System.out.println("남학생 전체 이름 : " + mapByName.get(Student.Sex.MALE));
		System.out.println("여학생 전체 이름 : " + mapByName.get(Student.Sex.FEMALE));
	}
}
```

## Stream 을 활용한 Grouping And Reduction 예제 02

| 리턴 타입                                  | Collectors의 정적 메소드                                                                                          | 설명                                                                           |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| Collector<T, ?, Map<K, List<T>>>           | groupingBy(Function<T,K> classifier)                                                                              | T를 K로 매핑하고 K키에 저장된<br> List에 T를 저장한 Map 생성                   |
| Collector<T, ?, ConcurrentMap<K, List<T>>> | groupingByConcurrent(Function<T,K> classifier)                                                                    | 위와 같음                                                                      |
| Collector<T, ?, Map<K, D>>                 | groupingBy(Function<T,K> classifier, <br>Collector<T,A,D> collector)                                              | T를 K로 매핑하고 K키에 저장된 D객체에 T를 누적한 Map 생성                      |
| Collector<T, ?, ConcueerntMap<K, D>>       | groupingByConcurrent(Function<T,K> classifier, <br>Collector<T,A,D> collector)                                    | 위와 같음                                                                      |
| Collector<T, ?, Map<K, D>>                 | groupingBy(Function<T,K> classifier, <br>Supplier<Map<K,D>> mapFactory, <br>Collector<T,A,D> collector)           | T를 K로 매핑하고 Supplier가 제공하는<br> Map에서 K키에 저장된 D객체에 T를 누적 |
| Collector<T, ?, ConcurrentMap<K, D>>       | groupingByConcurrent(Function<T,K> classifier, <br>Supplier<Map<K,D>> mapFactory, <br>Collector<T,A,D> collector) | 위와 같음                                                                      |

```java
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

import S20200123CustomCollect.Student;


public class GroupingByExam {

	public static void main(String[] args) {
		List<Student> totalList = Arrays.asList(
				new Student("홍길동", 10, Student.Sex.MALE, Student.City.Seoul),
				new Student("김수애", 6, Student.Sex.FEMALE, Student.City.Busan),
				new Student("신용권", 10, Student.Sex.MALE, Student.City.Busan),
				new Student("박수미", 6, Student.Sex.FEMALE, Student.City.Seoul));
		
		// Map에 Key값은 성별, Value값은 Student List로 Key값의 기준은 성별로 그룹핑한다.
		Map<Student.Sex, List<Student>> mapBySex = totalList.stream()
				.collect(Collectors.groupingBy(Student :: getSex));
		
		System.out.print("[남학생] ");
		mapBySex.get(Student.Sex.MALE).stream()
		.forEach(s -> System.out.print(s.getName() + " "));
		
		System.out.print("\n[여학생] ");
		mapBySex.get(Student.Sex.FEMALE).stream()
		.forEach(s -> System.out.print(s.getName() + " "));
		
		System.out.println();
		
		// Map에 Key값은 도시, Value값은 Student List로 Key값의 기준은 도시로 그룹핑한다.
		Map<Student.City, List<Student>> mapByCity = totalList.stream()
				.collect(Collectors.groupingBy(Student :: getCity));
		
		System.out.print("\n[서울] ");
		 mapByCity.get(Student.City.Seoul).stream()
		 .forEach(s -> System.out.print(s.getName() + " "));
		 
		System.out.print("\n[부산] ");
		mapByCity.get(Student.City.Busan).stream()
		.forEach(s -> System.out.print(s.getName() + " "));
	}
}
```