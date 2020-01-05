---
date: 2020-01-04 22:00:00
layout: post
title: Java Stack, Queue
subtitle: java.util.Stack, Queue, LinkedList 패키지 이용
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

## Import Package Lists
> import java.util.Stack; <br>
> import java.util.LinkedList; <br>
> import java.util.Queue; <br>

## Stack
```java
public class StackExam {

	public static void main(String[] args) {
		Stack<Coin> pocketCoin = new Stack<Coin>();
		
		for (int i = 10; i <= 100; i+=10) {
			
			pocketCoin.add(new Coin(i));
			System.out.println("동전 갯수 : " + pocketCoin.size());
		}
		
		// Stack pop() Lifo 구조
		// 마지막에 들어간게 맨 처음 나온다.
		while (!pocketCoin.isEmpty()) {
			
			//pop() 은 요소를 꺼내기만 한다.
			//peek() 는 요소를 꺼낸후 삭제한다.
			Coin coinTemp = pocketCoin.pop();
			System.out.println(coinTemp.getValue());
		}
	}
}
```
## Coin Class
```java
public class Coin {

	private int value;
	
	public Coin(int value) {
		this.value = value;
	}
	
	public int getValue() {
		return value;
	}
}
```
---
## Queue
```java
public class QueueExam {

	public static void main(String[] args) {
		// Queue는 LinkedList를 이용해 구현 한다.
		Queue<Message> msgQueue = new LinkedList<Message>();
		
		// offer 메소드를 이용해 Queue에 요소 쌓기
		msgQueue.offer(new Message("sendMail", "홍"));
		msgQueue.offer(new Message("sendSMS", "신"));
		msgQueue.offer(new Message("sendKakaotalk", "신"));
		
		while (!msgQueue.isEmpty()) {
			// poll() 메소드는 요소를 지우지 않고 Queue에서 뽑아만 옴
			// peek() 메소드는 요소를 지우고 Queue에서 뽑아옴 
			Message message = msgQueue.poll();
			switch (message.getCommand()) {
			case "sendMail":
				System.out.println(message.getTo() + " 님에게 MSG 전송.");
				break;
			case "sendSMS":
				System.out.println(message.getTo() + " 님에게 SMS 전송.");
				break;
			case "sendKakaotalk":
				System.out.println(message.getTo() + " 님에게 카톡 전송.");
				break;
			default:
				break;
			}
		}
	}
}
```
## Message Class
```java
public class Message {
	
	private String command;
	private String to;
	
	public Message() {
		// TODO Auto-generated constructor stub
	}
	
	public Message(String command, String to) {
		this.command = command;
		this.to = to;
	}
	
	public String getCommand() {
		return command;
	}
	
	public String getTo() {
		return to;
	}
}

```