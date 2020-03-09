---
date: 2020-02-20 22:00:00
layout: post
title: Java Input/Output Stream 기초
subtitle: 기본적인 Stream 사용
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

## Input/Output Stream

```java
import java.io.IOException;
import java.io.InputStream;

public class InputStreamExam {

	public static void main(String[] args) throws IOException{
		InputStream is = System.in;
		
		// 100 바이트 버퍼
		byte[] datas = new byte[100];
		
		System.out.println("이름 : ");
		int nameBytes = is.read(datas);
		// commentBytes-2를 하는 이유 엔터 제외 엔터 = 케리지리턴(13) + 라인피드(10)
		String name = new String(datas, 0, nameBytes-2);
		
		System.out.println("하고 싶은말 : ");
		int commentBytes = is.read(datas);
		// commentBytes-2를 하는 이유 엔터 제외 엔터 = 케리지리턴(13) + 라인피드(10)
		String comment = new String(datas, 0, commentBytes-2);
		
		System.out.println("입력한 이름 : " + name);
		System.out.println("입력한 하고 싶은말 : " + comment);
	}
}

//------------------------------------------------------------

import java.io.IOException;
import java.io.OutputStream;

public class OutputStreamExam {

	public static void main(String[] args) throws IOException{
		OutputStream os = System.out;
		
		String msg = "안녕하세요?";
		byte[] massage = msg.getBytes();
		
		os.write(massage);
		os.flush();
	}
}
```

## Input/Output Stream Reader/Writer

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

public class InputStreamReaderExam {

	public static void main(String[] args) {
		// 바이트 기반 InputStream을 문자 기반 InputStreamReader 로 변환
		
		InputStream is = System.in;
		InputStreamReader isr = new InputStreamReader(is);
		
		int data;
		char[] cbuf = new char[128];
		
		try {
			while ((data = isr.read(cbuf)) != -1) {
				String strData = new String(cbuf, 0, data);
				System.out.println(strData);
			}
			
			isr.close();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}	
}

// -----------------------------------------------------------

import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.io.OutputStreamWriter;

public class OutputStreamWriterExam {
	
	public static void main(String[] args) {
		String path = "C:\\\\Users\\\\dkdlw\\\\Documents\\\\Scanned Documents\\\\tempWr.txt";
		File file = new File(path);

		int dataLen;
		char[] cbuf = new char[128]; // 버퍼
		
		try {
			// InputStream 으로 키보드에서 문자를 읽고
			InputStream is = System.in;
			// 바이트 기반 -> 문자 기반 입력 스트림
			InputStreamReader isr = new InputStreamReader(is);
			
			// 문자 기반 출력 스트림 -> 바이트 기반 
			OutputStream os = new FileOutputStream(file); // 입력받은 메시지 저장할 경로
			OutputStreamWriter osw = new OutputStreamWriter(os); // 바이트 스트림으로 보내기
			
			while ( (dataLen = isr.read(cbuf)) != -1 ) {
				String msgTemp = new String(cbuf, 0, dataLen);
				System.out.println(msgTemp);
				osw.write(msgTemp); // 문자 내보내기
				osw.flush(); // 버퍼가 커서 flush 해줘야 함
			}
			
			isr.close();
			osw.flush();
			osw.close();
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
```