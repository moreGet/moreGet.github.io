---
layout: post
title:  "Java Net IO-TCP Server"
subtitle:   "MarkDown"
categories: devlog
tags: java
comments: true
# tags: tip
---

# JAVA IO(TCP) Server 구현
> Package import java.net.ServerSocket<br>
> Package import java.net.Socket<br>
> TCP방식을 이용한 간단한 서버 구현

<br>

## 생성자 사용방법

```java
// 기본 생성자 생성 방법

ServerSocket serverSocket = new ServerSocket(8000); 
// 8000 은 서버포트 여유있는 포트 입력 요망
```
***
## 서버 구현 소스

```java
try {	
			ServerSocket serverSocket = new ServerSocket(8000);
			while(true) {
				System.out.println("클라이언트 접속 대기중");
				Socket socket = serverSocket.accept();
				
				// 클라 <- 서버
                // IO(블록킹 방식) 스트림 으로 읽음.
                // 클라이언트 socket 으로 부터 데이터를 읽어들임
				BufferedReader bufferedReader = new BufferedReader(
						new InputStreamReader(
								socket.getInputStream()));
                
                // 읽은 데이터를 자료형에 맞게 변수에 저장                     
				String cli_Msg = bufferedReader.readLine();
				System.out.println("<< "+cli_Msg);
				
				// 서버 -> 클라
                // 서버에서 해당 클라이언트로 전송
				BufferedWriter bw = new BufferedWriter(
						new OutputStreamWriter(
								socket.getOutputStream()));
				
                //서버의 UUID 값을 클라이언트로 전송
				UUID uuid = UUID.randomUUID();
				bw.write(uuid.toString());
				bw.newLine(); // 데이터의 끝을 알림
				bw.flush(); // 스트림에 남은 데이터를 전부 전송시킴
				
				socket.close(); //소켓을 닫음
			}
		} catch (Exception e) {
			
		}
```