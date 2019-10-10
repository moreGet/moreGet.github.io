---
layout: post
title:  "Java ScreenShot"
subtitle:   "MarkDown"
categories: devlog
tags: java
comments: true
# tags: tip
---

# JAVA ScreenShot 기능 구현
> import java.awt.Rectangle;<br>
> import java.awt.Robot;<br>
> import java.awt.Toolkit;<br>
> import java.awt.image.BufferedImage;<br>
> import java.io.File;<br>
> import javax.imageio.ImageIO;<br>
> TCP방식을 이용한 간단한 클라이언트 구현

<br>

## 생성자 사용방법

```java
// 기본 생성자 생성 방법

Robot robot = new Robot(); // 스샷 캡쳐를 위한 Robot 인스턴스 선언
Rectangle rectangle = new Rectangle(Toolkit.getDefaultToolkit().getScreenSize()); // rectangle 클래스에 현재 스크린 사이즈 맵핑
BufferedImage image = robot.createScreenCapture(rectangle);
// bufferedimage 클래스에 캡쳐 정보 맵핑
image.setRGB(0,0,100); // 컬러 정보 맵핑
```
***
## 구현 소스

```java
		String saveFilePath = "C:/java/"; // 저장될 경로
        String saveFileName = "test"; // 저장할 파일 이름
        String saveFileExtension = "png"; // 확장자
        
        try {
        	
            Robot robot = new Robot();
            Rectangle rectangle = 
			new Rectangle(Toolkit.getDefaultToolkit().getScreenSize());

            BufferedImage image = robot.createScreenCapture(rectangle);
            image.setRGB(0,0,100);
            // 위와 같음

            File file = new File(saveFilePath+saveFileName+"."+saveFileExtension);
			// File 클래스로 경로 file 객체 생성
            ImageIO.write(image, saveFileExtension, file); // IO 스트림으로 씀.
        } catch (Exception e){
            e.printStackTrace();
        } // 결과는 현재 자신의 화면을 캡쳐함.
```