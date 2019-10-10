---
layout: post
title:  "Java ThreadPool"
subtitle:   "MarkDown"
categories: devlog
tags: java
comments: true
# tags: tip
---

# JAVA ThreadPool을 이용한 작업 요청
>import java.util.concurrent.ExecutorService;<br>
>import java.util.concurrent.Executors;<br>
>import java.util.concurrent.ThreadPoolExecutor;<br>
>쓰레드 풀을 이용한 멀티쓰레드 작업 요청

<br>

## 생성자 사용방법

```java
// 기본 생성자 생성 방법
// 작업큐에 작업요청을 하기 위해선 먼저 쓰레드 풀을 생성한다.
ExecutorService executorService = Executors.newFixedThreadPool(
        Runtime.getRuntime().
        availableProcessors()); 

// 주목할 점은 Runtime.getRuntime().availableProcessors()); 이다.
// 현재 활성화된 쓰레드 의 갯수를 가져온다. (논리코어)
```
***
## 구현 소스

```java
int threadValue = Runtime.getRuntime().availableProcessors();
ExecutorService executorService = Executors.newFixedThreadPool(threadValue);
// ExecutorService 클래스로 현재 활성화된 쓰레드 만큼 잡업큐 공간을 마련한다.

for(int i=0; i<threadValue; i++) {

    //Runnable 를 구현 하여 작업요청
    Runnable runnable = new Runnable() {
    
        @Override
        public void run() {
            //현재 작업중인 활성화된 쓰레드 번호를 불러오기위해 선언
            ThreadPoolExecutor poolExecutor = 
                    (ThreadPoolExecutor)executorService;

            //현재 까지 작업중인 쓰레드 갯수(poolSize)        
            int pool = poolExecutor.getPoolSize();
            //현재 작업중인 쓰레드 이름
            String name = Thread.currentThread().getName();
            
            //출력
            System.out.println(name+" "+pool);
            //execute() 와 submit() 의 차이점을 확인하기위해 고의로 예외발생
            Integer integer = Integer.parseInt("삼");
        }
    };
    
    executorService.execute(runnable); 
    //execute는 작업중인 쓰레드가 예외일때 쓰레드를 종료하고 새로 생성시킴
//	executorService.submit(runnable);
    //submit은 작업중인 쓰레드가 예외일때 쓰레드를 종료하지않고 재사용함
    try {
        Thread.sleep(100); //출력할 시간을 벌기위해 0.1초 멈춤
    } catch (InterruptedException e) {
        e.printStackTrace(); //예외 메시지 콘솔에 출력
    }
}
```