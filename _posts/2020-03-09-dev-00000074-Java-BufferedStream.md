---
date: 2020-02-20 22:00:00
layout: post
title: Java BufferedStream
subtitle: 속도 향상을 위한 보조 스트림 사용
description: "Current JDK : == JDK 1.8.0_231(64bit)"
image: ../assets/img/postsimg/R_main_00000003.png
optimized_image: ../assets/img/postsimg/R_op_00000003.png
category: coding
tags:
  - Eclipse
  - Java
  - Input
  - Ouput
  - Stream
  - Reader
  - Writer
  - buffered
author: thiagorossener
# image:760*400
# optimized_image:380*200
---

|                                                                           java.io 패키지의 주요 클래스                                                                          |                          설명                         |
|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-----------------------------------------------------:|
| File                                                                                                                                                                            |       파일 시스템의 파일 정보를 얻기 위한 클래스      |
| Console                                                                                                                                                                         |        콘솔로부터 문자를 입출력하기 위한 클래스       |
| InputStream / OuputStream                                                                                                                                                       | 바이트 단위 입출력을 위한 최상위 입출력 스트림 클래스 |
| FileInputStream / FileOutStream<br> DataInputStream / DataOuputStream<br> ObjectInputStream / ObjectOutputStream<br> PrintStream<br> BufferedInputStream / BufferedOutputStream |       바이트 단위 입출력을 위한 하위 스트림 클래스    |
| Reader / Writer                                                                                                                                                                 |  문자 단위 입출력을 위한 최상위 입출력 스트림 클래스  |
| FileReader /FileWriter<br> InputStreamReader / OutputStreamWriter<br> PrintWriter<br> BufferedReader / BufferedWriter<br>                                                       |        문자 단위 입출력을 위한 하위 스트림 클래스     |

## Buffered - Input/Output Stream

```java
import java.io.BufferedInputStream;
import java.io.File;
import java.io.FileInputStream;
import java.nio.file.Path;
import java.nio.file.Paths;

import javax.swing.JFileChooser;

public class BufferedInputStreamExam {

	public static void main(String[] args) throws Exception {
		long start = 0;
		long end  = 0;
		
		// 경로를 얻기 위해 파일선택 창 생성
		JFileChooser jfc = new JFileChooser();
		jfc.showOpenDialog(null);
		
		// OK버튼을 누름과 동시에 파일의 정보중 경로를 받아옴
		String pathTmp = jfc.getSelectedFile().getPath();
		
		// Path 클래스로 절대경로및 정규화를 거쳐 경로를 생성
		Path path1 = Paths.get(pathTmp).toAbsolutePath().normalize();
		System.out.println(path1.toString());
		File file = new File(path1.toUri()); // 그 URI를 받아 File 객체로 생성
		
		// 파일 입력 스트림을 생성함
		FileInputStream fis1 = new FileInputStream(file);
		// 보조 스트림을 사용하지 않고 파일을 읽는 시간을 기록하기 위해 아래 구문을 정의
		start = System.currentTimeMillis();
		
		// 파일을 읽음 파일을 다 읽게 되면 -1를 리턴함
		while (fis1.read() != -1) {}
		end = System.currentTimeMillis(); // 파일을 다 읽은후의 시점을 기록
		System.out.println("Buffered Stream를 사용 안했을때 : " + (end-start) + "ms"); // 시간 출력
		fis1.close(); // 사용한 스트림을 닫고 자원을 반납
		
		// 아래는 보조 스트림을 이용한 읽기 및 시간 기록 패턴
		FileInputStream fis2 = new FileInputStream(file);
		BufferedInputStream bis = new BufferedInputStream(fis2); // 보조 입력 스트림을 체이닝
		start = System.currentTimeMillis();
		
		while (bis.read() != -1) {} //읽기
		end = System.currentTimeMillis();
		System.out.println("Buffered Stream를 사용 했을때 : " + (end-start) + "ms"); // 시간 기록
		bis.close(); // 보조 스트림 닫기 
		fis1.close(); // 입력 스트림 닫기
	}
}

//------------------------------------------------

import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;

public class BufferedOuputStreamExam {
	
	public static void main(String[] args) throws Exception {
		FileInputStream fis = null;
		FileOutputStream fos = null;
		BufferedInputStream bis = null;
		BufferedOutputStream bos = null;
		
		int data = -1;
		long start = 0;
		long end = 0;
		int per = 166085682;
		double result = 0;
		
		byte[] buf = new byte[512];
		
		String path = "C:\\TMP\\a.pdf";
		fis = new FileInputStream(path);
		bis = new BufferedInputStream(fis);
		fos = new FileOutputStream("C:\\TMP\\nobuf.pdf");
		start = System.currentTimeMillis();
		while ((data = bis.read()) != -1) {
			result += ((double)data/(double)per)*100;
			System.out.format("%.2f%%%n", result);
			fos.write(buf, 0, data);
		}
		
		fos.flush();
		end = System.currentTimeMillis();
		fos.close(); bis.close(); fis.close();
		System.out.println("버퍼 사용 안 했을 때 : " + (end-start) + "ms");
		
		fis = new FileInputStream(path);
		bis = new BufferedInputStream(fis); 
		fos = new FileOutputStream("C:\\TMP\\copy.pdf");
		bos = new BufferedOutputStream(fos);
		start = System.currentTimeMillis();
		while ((data = bis.read()) != -1) {
			bos.write(data);
		}
		bos.flush();
		end = System.currentTimeMillis();
		System.out.println("버퍼 사용 했을 때 : " + (end-start) + "ms");
		bos.close(); fos.close(); bis.close(); fis.close();
	}
}
```