---
layout: post
title:  "Java ThreadPool - Future"
subtitle:   "MarkDown"
categories: devlog
tags: java
comments: true
# tags: tip
---

# JAVA ThreadPool을 이용한 작업후 결과 취합(Future 이용)
>import java.util.concurrent.ExecutorService;<br>
>import java.util.concurrent.Executors;<br>
>import java.util.concurrent.Future;<br>
>쓰레드 풀을 이용한 멀티쓰레드 작업 요청

<br>

## 생성자 사용방법

```java
//전번 포스팅에서 마찬가지로 현재 활성화된 쓰레드를 불러 옵니다.
int threadValue = Runtime.getRuntime().availableProcessors();
		ExecutorService executorService = Executors.newFixedThreadPool(threadValue);
        //Executors 클레스를 이용하여 받아옴.

//방법 1
Runnable runnable = new Runnable() { 
    //Runnable 객체 생성
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
        //현재 작업중인 쓰레드 이름 출력
    }
};

//방법 2
Callable<Runnable> callable = new Callable<Runnable>() {
    //Runnable 말고도 Callable 로도 가능.
    @Override
    public Runnable call() throws Exception {
        System.out.println(Thread.currentThread().getName());

 		int result = 0; //Integer 값으로 결과 리턴 나중에 취합
		return result;
    }
};

```
***
## 구현 소스

```java
int threadValue = Runtime.getRuntime().availableProcessors();
        ExecutorService executorService = Executors.newFixedThreadPool(threadValue); //위와 같음

// 방법 1
// 이경우 쓰레드를 한개만 쓰게 된다.
Future future = executorService.submit(new Runnable() {
    
    @Override
    public void run() {
        for(int i=0; i<threadValue; i++) {
            
            System.out.println(Thread.currentThread().getName()+" "+i);
        }
    }
});

// 방법 2
// 이경우 할당 쓰레드 수 만큼 작업을 요청하여 취합한다.
Runnable runnable = new Runnable() {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
};

Future future = null; // 결과를 취합 하기 위해 선언된 future

for(int i=0; i<threadValue; i++) {
    future = executorService.submit(runnable);
} //쓰레드 수 만큼 작업 요청후 future에 취합

executorService.shutdown(); //서비스 인터럽트 시킴
try {
    System.out.println(future.get()); //future.get() 으로 취합한 자료 가져옴
    
} catch (Exception e) { 
    //멀티 catch 로 인터럽트의 경우나 작업중 종료된 경우 예외처리함
}
```