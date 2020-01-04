---
date: 2020-01-03 22:00:00
layout: post
title: Java TreeSet을 이용한 정렬(Comparable 구현)
subtitle: java.util.TreeSet 패키지 이용
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

## Import Package Lists
> import java.util.Iterator; <br>
> import java.util.TreeSet; <br>

## 소스 코드
```java
public class ComparableExam {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		// TreeSet에 객체 저장시 Comparable를 필수로 구현 해야 한다.
		// 이는 오름차순 으로 정렬 된다.
		TreeSet<Person> treeSet = new TreeSet<Person>();
		
		treeSet.add(new Person("신", 25));
		treeSet.add(new Person("김", 21));
		treeSet.add(new Person("박", 27));
		treeSet.add(new Person("최", 8));
		treeSet.add(new Person("이", 24));
		
		Iterator<Person> it = treeSet.iterator();
		
		while (it.hasNext()) {
			Person person = (Person) it.next();
			
			String name = person.getName();
			int age = person.getAge();
			System.out.println(name + "/" + age);
		}
	}
}
```
## Person Class
```java
public class Person implements Comparable<Person>{

	private String name;
	private int age;
	
	public Person() {
		// TODO Auto-generated constructor stub
	}
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
	
	@Override
	public int compareTo(Person o) {
		
		if (age < o.age) {
			return -1;
		} else if(age == o.age) {
			return 0;
		} else {
			return 1;
		}
	}
	
	public int getAge() {
		return age;
	}
	
	public String getName() {
		return name;
	}
}
```