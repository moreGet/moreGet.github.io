---
layout: post
title:  "Java IO interrupt 안전하게 하기"
subtitle:   "MarkDown"
categories: devlog
tags: java
comments: true
# tags: tip
---

# JAVA IO TCP 서버 구현시 안전 하게 종료하기

> 서버가 accept() 도중 종료되면 SocketException가 발생 합니다.<br>
> 여기서 try-catch 블럭을 통해 안전하게 종료 시킵니다.

<br>

## 구현 소스

```java
public class ServerModel implements Runnable{

	private Object lock = new Object(); //Mutex로 사용할 obj
	private boolean ready = true;

	public void closeServer() {
		try {
			synchronized (lock) { //클라와 통신중인지 확인
				serverSocket.close();
			}
		} catch (Exception e) {
			// TODO: handle exception
		}
	}

	@Override
	public void run() {
		
		try {
			serverSocket = new ServerSocket(8000);
			
			while(ready) {
				Socket socket = serverSocket.accept();
				
				synchronized (lock) { // 클라이언트와 통신중일 경우 소켓 종료 보류
					br = new BufferedReader(
							new InputStreamReader(
									socket.getInputStream()));
					
					bw = new BufferedWriter(
							new OutputStreamWriter(
									socket.getOutputStream()));
					
					String msg = br.readLine();
					InetAddress name = socket.getInetAddress();
					msg = "Hi "+name.getHostName();
					
					bw.write(msg);
					bw.newLine();
					bw.flush();		
					socket.close();
				}
			}
		} catch (Exception e) {
			//서버소켓이 종료되면 예외 발생
			System.out.println("서버 종료");
		}
	}
}
```