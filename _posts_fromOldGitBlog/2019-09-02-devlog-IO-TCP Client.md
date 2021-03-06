---
layout: post
title:  "Java Net IO-TCP Client"
subtitle:   "MarkDown"
categories: devlog
tags: java
comments: true
# tags: tip
---

# JAVA IO(TCP) Client 구현
> Package import java.net.Socket<br>
> TCP방식을 이용한 간단한 클라이언트 구현

<br>

## 생성자 사용방법

```java
// 기본 생성자 생성 방법

Socket Socket = new ServerSocket("localhost", 8000); 
// 또는
Socket Socket = new ServerSocket("127.0.0.1", 8000);
// IP 는 서버 IP 입력

// *뽀나스* 현재 클라이언트 IP 를 얻어옴.
Inet4Address.getLocalHost();
// 아래는 IP 출력
System.out.println(Inet4Address.getLocalHost());
```
***
## 클라 구현 소스

```java
try {
			Socket socket = new Socket("127.0.0.1", 8000); //서버IP 와 포트
				
			// 서버로 보낼 데이터 스트림을 통해 입력
			BufferedWriter bw = new BufferedWriter(
					new OutputStreamWriter(
							socket.getOutputStream()));
			
			bw.write("클라이언트 에서 보낸 메시지 "+Inet4Address.getLocalHost());
			bw.newLine(); // 메시지의 끝을 알림
			bw.flush(); // 힘줘서 쭉 데이터를 전송
			
			// 서버에서 보낸 데이터를 읽음
			BufferedReader br = new BufferedReader(
					new InputStreamReader(
							socket.getInputStream()));
			
			// 읽어들임
			String ser_Msg = br.readLine(); //서버 메세지 수신
			System.out.println(">> "+ser_Msg); //출력
			
			// 소켓 닫음
			socket.close();
		} catch (Exception e) {
			// TODO: handle exception
		}
```