---
date: 2020-01-31 22:00:00
layout: post
title: Java SpringBoot 입문 Part.02
subtitle: SpringBoot FrameWork 사용을 위한 기초 
description: "SpringBoot 2.0"
image: ../assets/img/postsimg/R_main_00000003.png
optimized_image: ../assets/img/postsimg/R_op_00000003.png
category: coding
tags:
  - Eclipse
  - Java
  - Lambda
  - vsCode
  - SpringBoot
  - Web
  - REST
  - JPA
author: thiagorossener
# image:760*400
# optimized_image:380*200
---

## Chapter.01-1

- JAP를 사용하기 위해 SpringBoot 프로젝트를 생성하고
- 아래 의존관계를 정의 합니다.

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

- 나머지 의존 관계는 필요에 따라 추가 합니다.
- cmd창에서 프로젝트 경로로 이동한 뒤 아래 명령어로 추가된 라이브러리를 확인 해봅니다.
  - mvn dependency:tree
- SpringData JAP와 JPA를 구현한 library인 하이버네이트를 사용함을 알 수 있습니다.

### com.example.app.domain.Customer.java

```java
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.Table;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Entity	// JPA 개체(Entity)임을 표시 합니다.

// @Table 어노테이션을 붙여 엔티티에 대응하는 테이블 이름을 지정합니다.
// 기본적으로 테이블 이름을 클래스 이름과 동일하게 맞추는게 컨벤션이지만 
// 여기선 다르게 설정 합니다.
@Table(name = "customers")

// class파일을 생성할때 각 필드의 setter/getter, toString, equals, hashCode 메소드가
// 생성되므로 소스코드가 간결해진다. 아래 필드는 final형태라 setter/getter가 생성되지 않는다.
@Data

// JPA 명세에 따르면 엔티티 클래스에는 인자를 받지 않는 기본 생성자를 만들어야 합니다.
// 롬복으로 기본 생성자를 만들려면 아래 어노테이션을 추가 합니다.
@NoArgsConstructor

// JPA와는 관계 없지만 쉽게 프로그래밍을 할 수 있게 롬복이 기본 생성자 외에 전체 필드를
// 인자로 받는 생성자를 만들도록 설정 합니다.
@AllArgsConstructor
public class Customer {
	@Id // 엔티티의 기본 키인 필드에 @Id 어노테이션을 붙입니다.
	@GeneratedValue // DB가 기본 키 번호를 자동으로 매기도록 이 어노테이션을 붙여 지정합니다.
	private Integer id;
	
	// 필드에 이 어노테이션을 붙여서 DB의 대응하는 컬럼의 이름이나 제약 조건을 설정합니다.
	// 여기서는 NotNull 제약조건을 설정 합니다.
	@Column(nullable = false)
	private String firstName;
	@Column(nullable = false)
	private String lastName;
}
```

- JPA의 대한 자세한 설명은 서적을 찾아보아야 합니다.

<hr>

## Chapter.01-2 SpringData JAP로 리포지토리 클래스 작성하기

- 스프링 데이터는 데이터 저장소 조작을 위한 범용 기능을 제공하는 하위 프로젝트 입니다.
- RDBMS, MongoDB, Redis, Neo4J 같은 데이터 저장소를 조작할 수 있습니다.
- 그리고 범용 리포지토리 클래스도 제공합니다.
- 여기선 JPA로 RDBMS를 조작하는 스프링 데이터 JPA를 이용합니다. 
- SpringData JPA가 제공하는 JpaRepository를 사용하여 CustomerRepository를 다시 작성합니다.
- 아래는 CustomerRepository 클래스 입니다.

### com.example.app.reposioCustomerRepository Class

```java
public interface CustomerRepository extends JpaRepository<Customer, Integer> {

	/*
	 *  JpaRepository 클래스 에는 다음과 같은 CRUD(Create, Read, Update, Delete)
	 *  기본 조작용 메소드가 정의되ㅣ어 있으며, JpaRepository를 상속한 인터페이스를 만드는 작업만으로 아주 쉽게
	 *  리포지토리를 작성할 수 있습니다. 
	 *  
	 *  findOne, save, findAll, delete
	 */
}
```

- 인터페이스만 있으면 실행 시에 "실행 클래스"가 생성되므로 프로그램을 장황하게 기술하지 않아도 됩니다.
- 이 리포지토리 클래스를 사용하여 스프링JDBC를 설명할 때 만든 App 클래스를 수정 없이 그대로 실행할 수 있씁니다.
- 스프링 JDBC를 사용한 DB접속 에서 작성한 App.java왕 log4jdbc 관련 설정 파일을 설정 해주시길 바랍니다.

```java
// App 클래스
@SpringBootApplication
public class App implements CommandLineRunner{
	
	@Autowired
	CustomerRepository customerRepository;
	
	@Override
	public void run(String... args) throws Exception {
		// 더미 데이터 추가
		Customer created = customerRepository.save(new Customer(null, "Hidetoshi", "Dekisugi"));
		System.out.println(created + " is created!");
		
		// 데이터 표시
		customerRepository.findAll().forEach(System.out::println);
	}
	
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}
}
```

- 별 다른 셋팅 없이 DB와 연결
- 내장 DB를 사용하면 프로그램을 실행했을 때 테이블을 삭제하고 다시 작성 하는 기본 기능이 실행됩니다. 실행결과에서 create 구문 부분을 보면 @Column 어노테이션으로 지정한 not null 속성이 그대로 적용되었음을 알 수 있습니다. 그리고 customerRepository.save를 통해 insert가 자동으로 실행되고, findAll을 통해 select가 자동으로 실행되었다는 사실도 알 수 있습니다.

<hr>

## Chapter.01-3 JPQL로 쿼리 정의하기

- JpaRepository에 정의되어 있찌 않은 검색 처리를 하려면 상속한 인터페이스에 해당 메소드를 추가합니다. JPQL로 쿼리를 작성할 수 있습니다.(Java Persistence Query Language)
- JPQL은 JPA로 엔티티를 조작할 떄 사용하는 쿼리 언어로 SQL과 닮았습니다. JPQL은 실행될 때SQL로 변환되며 RDBMS 기능에 따라 제각각인 SQL 언어들을 흡수 합니다. 그래서 JPQL을 사용하면 특정 벤더에 의존하지 않는 쿼리를 작성할 수 있습니다.
- 아래는 이름을 오름차순으로 검색하는 메소드를 정의 해보겠습니다.


```java
// CustomerRepository
public interface CustomerRepository extends JpaRepository<Customer, Integer> {

	/*
	 *  JpaRepository 클래스 에는 다음과 같은 CRUD(Create, Read, Update, Delete)
	 *  기본 조작용 메소드가 정의되ㅣ어 있으며, JpaRepository를 상속한 인터페이스를 만드는 작업만으로 아주 쉽게
	 *  리포지토리를 작성할 수 있습니다. 
	 *  
	 *  findOne, save, findAll, delete
	 */
	
	@Query("SELECT x FROM Customer x ORDER BY x.firstName, x.lastName")
	List<Customer> findAllOrderByName(); // JPQL을 기술할 떄 @query 어노테이션을 붙힌다.
}
```

## Chapter.01-4 페이징 처리 구현하기

- 스프링 데이터에는 데이터 접속 시 페이징 처리를 쉽게 할 수 있는 기능이 마련되어 있습니다. JpaRepository에 페이징 처리용 메소드가 포함되어 있습니다.
- pageable API : https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/domain/PageRequest.html

```java
//CustomerRepository
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import com.example.app.domain.Customer;

public interface CustomerRepository extends JpaRepository<Customer, Integer> {

	/*
	 *  JpaRepository 클래스 에는 다음과 같은 CRUD(Create, Read, Update, Delete)
	 *  기본 조작용 메소드가 정의되ㅣ어 있으며, JpaRepository를 상속한 인터페이스를 만드는 작업만으로 아주 쉽게
	 *  리포지토리를 작성할 수 있습니다. 
	 *  
	 *  findOne, save, findAll, delete
	 */
	
	@Query("SELECT x FROM Customer x ORDER BY x.firstName, x.lastName")
	Page<Customer> findAllOrderByName(Pageable pageable); // JPQL을 기술할 떄 @query 어노테이션을 붙힌다.
}
```

```java
//App
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;

import com.example.app.domain.Customer;
import com.example.app.repository.CustomerRepository;

@SpringBootApplication
public class App implements CommandLineRunner{
	
	@Autowired
	CustomerRepository customerRepository;
	
	@Override
	public void run(String... args) throws Exception {
		// 더미 데이터 추가
		int idx = 0;
		while (idx < 10) {
			customerRepository.save(new Customer(null, "Hidetoshi", idx));
			idx++;
		}
			
		// 데이터 표시
		// customerRepository.findAllOrderByName().forEach(System.out::println);/
		
		// 페이징 처리
		// 첫번째인자 : 페이지수(0부터 시작), 페이지당 들어갈 데이터 수
		Pageable pageable = PageRequest.of(0, 5);
		// findAll 메소드를 실행하여 지정한 페이지의 domain 데이터를 가져옵니다. 반환값은 Page<customer>
		Page<Customer> page = customerRepository.findAllOrderByName(pageable);
		// 페이지 처리후 정보 출력
		System.out.println("한 페이지당 데이터 수 = " + page.getSize());
		System.out.println("현재 페이지 = " + page.getNumber());
		System.out.println("전체 페이지 수 = " + page.getTotalPages());
		System.out.println("전체 데이터 수 = " + page.getTotalElements());
		// getContent() 로 해당 페이지의 데이터 리스트를 가져올 수 있음.
		page.getContent().forEach(System.out::println);
	}
	
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}
}
```

