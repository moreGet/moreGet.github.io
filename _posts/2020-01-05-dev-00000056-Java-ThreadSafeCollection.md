---
date: 2020-01-05 22:00:00
layout: post
title: Java ThreadSafe한 동시성과 병렬성
subtitle: List, Set, Map
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
author: thiagorossener
# image:760*400
# optimized_image:380*200
---

| Collection |                        |                                             |
|------------|------------------------|---------------------------------------------|
| List       | 순서있음, 중복가능     | Vector, ArrayList, LinkedList               |
| Set        | 순서있음, 중복불가능   | HashSet, Linked HashSet, SortedSet, TreeSet |
| Map        | Key, 와 Value값을 지님 | HashTable, HashMap, SortedMap, TreeMap      |

## 동시성
> 싱글 코어에서 멀티 Thread를 동작 시키기 위한 방식

## 병렬성
> 멀티 코어에서 멀티 스레드를 동작시키는 방식으로, 한 개 이상의 스레드를 포함하는 각 코어들이 동시에 실행되는 성질을 말함.<br>
> 데이터 병렬성과 작업 병렬성으로 구분된다.

## Import Package Lists
> import java.util.ArrayList; <br>
> import java.util.Collections; <br>
> import java.util.HashMap; <br>
> import java.util.HashSet; <br>
> import java.util.List; <br>
> import java.util.Map; <br>
> import java.util.Set; <br>

## Thread Safe한 컬렉션
```java
public class ThreadSafeCollection {

	public static void main(String[] args) {
		// Vector, HashTable 은 ThreadSafe 하다.
		// ArrayList, HashSet, HashMap은 ThreadSafe 하지않다. 따라서,
		// 이를 위해 synchronizedXXX() 메소드를 활용해 threadSafe하게 만들어 준다.
		// 점유하고 있는 스레드가 존재 한다면 다른 스레드는 점유 스레드가 작업을 완료하기 전까지 점유 할 수 없다.
		
		// 쓰레드 세이프 한 컬렉션 List
		List<Person> list = Collections.synchronizedList(new ArrayList<Person>());
		list.add(new Person("신", 27));
		
		// 쓰레드 세이프 한 컬렉션 Set
		Set<Person> set = Collections.synchronizedSet(new HashSet<Person>());
		set.add(new Person("신", 27));
		
		// 쓰레드 세이프 한 컬렉션 Map
		Map<String, Integer> map = Collections.synchronizedMap(new HashMap<String, Integer>());
		map.put("신", 27);
	}
}
```

## 병렬처리를 위한 컬렉션
```java
public class ConcurrentExam {

	public static void main(String[] args) {
		// 병렬 처리용 Map
		Map<Integer, String> map = new ConcurrentHashMap<Integer, String>();
		map.put(1, "신");
		
		// 병렬 처리용 Queue
		Queue<Message> msgQuene = new ConcurrentLinkedQueue<Message>();
		msgQuene.add(new Message("SMS", "신"));
	}
}
```