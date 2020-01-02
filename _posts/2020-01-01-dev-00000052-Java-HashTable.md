---
date: 2020-01-01 22:00:00
layout: post
title: Java Hashtable을 이용한 자료구조
subtitle: java.util.Hashtable 패키지 이용
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
> import java.util.Hashtable; <br>
> import java.util.Map; <br>
> import java.util.Scanner; <br>

## 소스 코드
```java
public class HashTableExam {

	public static void main(String[] args) {
		// ID와 Key 저장을 위한 HashMap
		Map<String, String> map = new Hashtable<String, String>();
		
		map.put("Spring", "12");
		map.put("Summer", "123");
		map.put("Fall", "1234");
		map.put("Winter", "12345"); // put() 메소드로 데이터를 Hashtable에 축척시킨다
		
//		키보드 입력
		Scanner sc = new Scanner(System.in);
		
		while (true) {
			
			System.out.println("아이디와 비밀번호를 입력 하세요.");

			// ID 입력받음
			System.out.println("아이디 : ");
			String id = sc.nextLine();
			
			System.out.println("비밀번호 : ");
			String pass = sc.nextLine();
			System.out.println(); // 줄 개행
			
			if (map.containsKey(id)) {
				if (map.get(id).equals(pass)) {
					System.out.println("로그인 되었습니다.");
					break;
				} else {
					System.out.println("비밀번호가 일치하지 않습니다.");
				}
			} else {
				System.out.println("입력하신 아이디가 존재하지 않습니다.");
			}
		}
	}
}
```