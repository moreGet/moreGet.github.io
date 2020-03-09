---
date: 2020-02-20 22:00:00
layout: post
title: Java File Reader/Writer
subtitle: 문자/바이트 기반 스트림
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

## File - Input/Output Stream

```java
import java.io.File;
import java.io.FileInputStream;

public class FileInputStreamExam {

	public static void main(String[] args) {
		
		String path = "C:\\Users\\dkdlw\\Documents\\Scanned Documents\\a.jpg";
		File file = new File(path);
		
		try {
			FileInputStream fis = new FileInputStream(file);
			
			int data;
			while ((data = fis.read()) != -1) {
				 System.out.write(data);
			}
			
			fis.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

//----------------------------------------------------------

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;

public class FileOutputStreamExam {
	
	public static void main(String[] args) {
		/*
		 * 파일을 읽는다.
		 */
		String originFile = "C:\\Users\\dkdlw\\Documents\\Scanned Documents\\a.jpg";
		String targetFile = "C:\\Users\\dkdlw\\Documents\\Scanned Documents\\b.jpg";
		File ori_file = new File(originFile);
		File tar_file = new File(targetFile);
		
		try {
			FileInputStream fis = new FileInputStream(ori_file);
			FileOutputStream fos = new FileOutputStream(tar_file);
			
			int data;
			byte[] buf = new byte[128];
			while ((data = fis.read(buf)) != -1) {
				 
				fos.write(buf, 0, data);
			}
			
			fis.close();
			
			fos.flush();
			fos.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

## File - Reader/Writer Stream

```java
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStreamReader;
import java.nio.charset.Charset;

public class FileReaderExam {

	public static void main(String[] args) {
		
		String path = "C:\\Users\\dkdlw\\Documents\\Scanned Documents\\temp.txt";
		
		try {
//			FileReader fr = new FileReader(path); // 한글이 깨진다.
			BufferedReader br = new BufferedReader( // 차선책 
					new InputStreamReader(
							new FileInputStream(path), Charset.forName("UTF-8")));
			
			int data;
			char[] buf = new char[128]; // 128byte 의 버퍼
			
//			while ((data = fr.read(buf)) != -1) {
//				String strData = new String(buf, 0, data);
//				System.out.println(strData);
//			}
//			
//			fr.close();
			
			while ((data = br.read(buf)) != -1) {
				String strData = new String(buf, 0, data);
				System.out.println(strData);
			}
			
			br.close();
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}

//----------------------------------------------------------

import java.io.File;
import java.io.FileWriter;
import java.io.IOException;

public class FileWriterExam {

	public static void main(String[] args) {
		String path = "C:\\Users\\dkdlw\\Documents\\Scanned Documents\\tempWr.txt";
		File file = new File(path);
		
		try {
			// \r : 캐리지리턴 + \n : 라인피드 = 엔터
			String data = "문자 저장\n안녕?";
			FileWriter fw = new FileWriter(file);
			
			fw.write(data);
			fw.flush();
			fw.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
		
	}
}
```