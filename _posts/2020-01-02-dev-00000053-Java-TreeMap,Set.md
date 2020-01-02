---
date: 2020-01-01 22:00:00
layout: post
title: Java TreeMap, Set을 이용한 자료구조
subtitle: java.util.NavigableMap / TreeSet 패키지 이용
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
> import java.util.Map; <br>
> import java.util.NavigableMap; <br>
> import java.util.Set; <br>
> import java.util.TreeMap;

## 소스 코드
```java
public class TreeMapExam {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		TreeMap<Integer, String> treeMap = new TreeMap<Integer, String>();
		
		treeMap.put(1, "홍");
		treeMap.put(2, "김");
		treeMap.put(4, "박");
		treeMap.put(5, "임");
		treeMap.put(6, "최");
		treeMap.put(7, "신");
		treeMap.put(8, "이");
		
		Map.Entry<Integer, String> mapEntry = null;
		
		mapEntry = treeMap.firstEntry(); // 첫번째 값
		System.out.println(mapEntry.getKey() + " : " + mapEntry.getValue());
		
		mapEntry = treeMap.floorEntry(3); // Key값이 3이거나 바로 아래 값(더 작은 값)
		System.out.println(mapEntry.getKey() + " : " + mapEntry.getValue());
		
		mapEntry = treeMap.ceilingEntry(3); // Key값이 2이거나 바로 윗 값(더 큰 값)
		System.out.println(mapEntry.getKey() + " : " + mapEntry.getValue());
		
		// 맵 Key값 기준 Sort
		NavigableMap<Integer, String> descMap = treeMap.descendingMap(); // 내림차순
		NavigableMap<Integer, String> ascMap = descMap.descendingMap(); // 오름차순
		
		// Set값 받아오기
		Set<Map.Entry<Integer, String>> descMapSet = descMap.entrySet();
		Set<Map.Entry<Integer, String>> ascMapSet = ascMap.entrySet();
		
		// 출력
		System.out.println();
		System.out.println("for 문으로 TreeMap 출력하기(내림, 오른차순)");
		for (Map.Entry<Integer, String> temp : descMapSet) {
			System.out.println(temp.getKey() + "/" + temp.getValue());
		}
	
		System.out.println();
		
		// 오른차순 출력
		for (Map.Entry<Integer, String> temp : ascMapSet) {
			System.out.println(temp.getKey() + "/" + temp.getValue());
		}
		
		System.out.println();
		System.out.println("데이터 꺼내기");
		while (!treeMap.isEmpty()) {
			System.out.print("꺼낸 데이터 : " + treeMap.pollFirstEntry());
			System.out.println();
			System.out.print("남은 데이터 : " + treeMap.size());
		}
	}
}
```
## Exam 02
```java
public class TreeMapExam02 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		// SubSet 사용하기
		TreeMap<Integer, String> treeMap = new TreeMap<Integer, String>();
		
		treeMap.put(1, "홍");
		treeMap.put(2, "김");
		treeMap.put(4, "박");
		treeMap.put(5, "임");
		treeMap.put(6, "최");
		treeMap.put(7, "신");
		treeMap.put(8, "이");
		
		// treeMap 에서 2와 6포함 각 사이 값 엔트리 리턴
		NavigableMap<Integer, String> subMap = treeMap.subMap(2, true, 6, true);
		// set으로 treeMap 값 정렬
		Set<Map.Entry<Integer, String>> set = subMap.entrySet();
		
		System.out.println("Set으로 For문 돌림");
		for (Map.Entry<Integer, String> entry : set) {
			System.out.println(entry.getKey() + "/" + entry.getValue());
		}
		
		System.out.println();
		
		// 주어진 키보다 낮은 값들을 가져옴(2번째 인자 는 자신을 포함 하는지 안하는지.)
		NavigableMap<Integer, String> headMapEntry = treeMap.headMap(4, true);
		//treeMap.tailMap() 은 주어진 키보다 높은 값들을 가져옴
		
		System.out.println("Set으로 For문 돌림");
		for (Map.Entry<Integer, String> entry : headMapEntry.entrySet()) {
			
			System.out.println(entry.getKey() + "/" + entry.getValue());
		}
	}
}
```
<br>

## Import Package Lists (TreeSet)
> import java.util.NavigableSet; <br>
> import java.util.TreeSet; <br>

```java
public class TreeSetExam {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		TreeSet<Integer> treeSet = new TreeSet<Integer>();
		TreeSet<Integer> ascTreeSet = new TreeSet<Integer>();
		TreeSet<Integer> easySortTreeSet = new TreeSet<Integer>();
		
		treeSet.add(new Integer(87));
		treeSet.add(new Integer(98));
		treeSet.add(new Integer(75));
		treeSet.add(new Integer(95));
		treeSet.add(new Integer(80));
		
		// 컬렉션 복사를 위한 깊은 복사
		ascTreeSet = (TreeSet<Integer>) treeSet.clone();
		easySortTreeSet = (TreeSet<Integer>) ascTreeSet.clone();
		Integer score = null;
		
		score = treeSet.first();
		System.out.println("가장 낮은 점수 : " + score);
		
		score = treeSet.last();
		System.out.println("가장 높은 점수 : " + score);
		
		score = treeSet.floor(new Integer(98));
		System.out.println("98점 이거나 바로 아래 점수 : " + score);
		
		score = treeSet.ceiling(new Integer(95));
		System.out.println("95점 이거나 바로 윗 점수 : " + score);
		
		System.out.print("내림차순 : ");		
		while (!treeSet.isEmpty()) {
			System.out.print(treeSet.pollLast() + " ");
		}
		
		System.out.println();
		
		System.out.print("오름차순 : ");
		while (!ascTreeSet.isEmpty()) {
			System.out.print(ascTreeSet.pollFirst() + " ");
		}
		
		// 쉽게 정렬하기.
		// 내림
		NavigableSet<Integer> descTree = easySortTreeSet.descendingSet();
		// 오름
		NavigableSet<Integer> ascTree = descTree.descendingSet();
		
		System.out.println();
		System.out.print("NavigableSet 을 이용한 오름차순 : ");
		for (Integer integer : ascTree) {
			System.out.print(integer + " ");
		}
		
		System.out.println();
		System.out.print("NavigableSet 을 이용한 내림차순 : ");
		for (Integer integer : descTree) {
			System.out.print(integer + " ");
		}
	}
}
```

## Exam 02
```java
public class TreeSetExam02 {

	public static void main(String[] args) {
		// 영단어 Tree Set
		TreeSet<String> wordTreeSet = new TreeSet<String>();
		
		wordTreeSet.add("apple");
		wordTreeSet.add("banana");
		wordTreeSet.add("oop");
		wordTreeSet.add("java");
		wordTreeSet.add("python");
		wordTreeSet.add("cherry");
		
		System.out.println("c ~ z 단어 검색");
		// treeSet에 저장된 단어들 줄 c와 z 사이의 단어들을 추출(c와 z포함)
		NavigableSet<String> rangeSet = wordTreeSet.subSet("c", true, "z", true);
		
		for (String string : rangeSet) {
			System.out.println(string);
		}
	}
}
```