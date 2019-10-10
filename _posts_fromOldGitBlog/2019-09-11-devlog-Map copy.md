---
layout: post
title:  "Java Map(Use HashMap)"
subtitle:   "MarkDown"
categories: devlog
tags: java
comments: true
# tags: tip
---

# JAVA Map을 이용한 컬렉션 엔트리 사용
>import java.util.HashMap;<br>
>import java.util.Map;<br>
>import java.util.Set;<br>

<br>

## 생성자 사용방법

```java
Map<Integer, String> map = new HashMap<Integer, String>();
// 제네릭 기법 을 사용하여 Map 컬렉션 내부 자료형태를 정의하고 선언함
// Map 엔트리에 이미 존재하는 Key 값을 넣으면 새로운 값으로 덮어씌워짐
```
***
## 구현 소스

```java
		Map<Integer, String> map = new HashMap<Integer, String>();
		
		map.put(1, "SK");
		map.put(2, "KIA");
		map.put(3, "HANHWA");
		
        //map.put(2, "Copy"); // 덮어 씌워짐
		Set<Integer> key = map.keySet();
		
		for (Integer integer : key) {
            //String value = map.get(integer); // 권장 패턴
			System.out.print(integer+" "); // Key 출력
			System.out.print(map.get(integer));// Key 값의 맵핑 데이터 출력
            //System.out.print(value); // 권장 패턴
			System.out.println();
		}
}
```