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

<hr>

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

<hr>

## Chapter.02-1 REST Web Service 만들기

- REST 웹 서비스
- 와면에 표시하는 웹 어플리케이션
- REST 웹 서비스 = 웹 API 개발 이다.
- HTTP를 통해 클라이언트(JS등...) 와 데이터를 주고 받는 엔드 포인트가 되는 서비스 입니다.
- 프론트엔드 : HTML, JS, 백엔드 : REST 웹서비스 개발 추세

### CUstomerService의 CURD처리 구현하기

```java
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.transaction.annotation.Transactional;

import com.example.app.domain.Customer;
import com.example.app.repository.CustomerRepository;

//이러한 클래스를 DI컨테이너 에서 가져오고 해당 클래스에 속한 각 메소드를 다른 클래스에서 호출 하면 DB 트랜잭션이 자동으로 이루어 집니다.
//메서드가 제대로 실행되면 DB 트랜잭션이 커밋됩니다.
//실행 도중 오류가 발생하면 DB 트랜잭션이 롤백 됩니다.
//DI 컨테이너는 각 메소드 앞뒤에 처리를 추가한 클래스를 동적으로 생성합니다.
@Transactional
public class CustomerService {
	@Autowired
	CustomerRepository customerRepository;
	
	public List<Customer> findAll() {
		return customerRepository.findAllOrderByName();
	}
	
	public Customer findOne(Integer id) {
		return customerRepository.getOne(id);
	}
	
	public Customer create(Customer customer) {
		return customerRepository.save(customer);
	}
	
	public Customer update(Customer customer) {
		return customerRepository.save(customer);
	}
	
	public void delete(Integer id) {
		customerRepository.deleteById(id);
	}
}
```

- 먼저 고객 관리 시스템을 REST 방식으로 제작 하겠습니다.
- 아래는 공개할 웹 API 목록 입니다.

| API 이름                   | HTTP 메서드 | 리소스 경로         | 정상 동작 시 응답 상태 |
|----------------------------|-------------|---------------------|------------------------|
| 모든 고객 정보 얻기        | GET         | /api/customers      | 200 OK                 |
| 고객 한 명의 정보 얻기     | GET         | /api/customers/{id} | 200 OK                 |
| 신규 고객 정보 작성        | POST        | /api/customers      | 201 CREATED            |
| 고객 한 명의 정보 업데이트 | PUT         | /api/customers/{id} | 200 OK                 |
| 고객 한 명의 정보 삭제     | DELETE      | /api/customers/{id} | 204 NO CONTENT         |

- REST 웹 서비스를 만들기 위해 pom.xml을 다음과 같이 작성합니다.

<hr>

## Chapter.02-2 모든 고객 정보 얻기, 고객 한 명의 정보 얻기용 API 구현

- 구현 해야할 API 목록 입니다.

| API 이름                   | 메소드 이름    | 반환 값의 타입 |
|----------------------------|----------------|----------------|
| 모든 고객 정보 얻기        | getCustomers   | List<Customer> |
| 고객 한 명의 정보 얻기     | getCustomer    | Customer       |
| 신규 고객 정보 작성        | postCustomers  | Customer       |
| 고객 한 명의 정보 업데이트 | putCustomer    | Customer       |
| 고객 한 명의 정보 삭제     | deleteCustomer | void           |

### CustomerRestController 구현 부분

```java
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import com.example.app.domain.Customer;
import com.example.app.service.CustomerService;

// REST 웹 서비스의 엔드 포인트인 컨트롤러 클래스에는 이 어노테이션을 붙입니다.
@RestController
// 이 REST 웹 서비스에 접속하기 위한 경로를 @RequestMapping 어노테이션에 지정합니다.
@RequestMapping("api/customers")
public class CustomerRestController {
	@Autowired // 이미 작성된 CustomerService 클래스를 주입합니다.
	CustomerService customerService;
	
	// 이 메소드에 HTTP메소드 중 하나인 GET을 할당 합니다.
	// @RequestMapping 에서 지정한 경로에 접근 하면 getCustomers()가 실행 됩니다.
	@RequestMapping(method = RequestMethod.GET)
	List<Customer> getCustomers() {
		List<Customer> customers = customerService.findAll();
		// 위 Mapping 어노테이션 붙인 메소드의 반환 값은 직렬화 되어 HTTP 응답 내용 안에 설정 됩니다.
		// 기본으로 자바 객체는 JSON 형식으로 직렬화
		return customers;
	}
	
	// 아래 메소드 에도 GET할당 
	// 주소값 지정을 플레이스홀더로 id값을 정해 주었기 떄문에
	// api/customers/id 값 으로 접근이 가능함
	@RequestMapping(value = "{id}", method = RequestMethod.GET)
	Customer getCustomer(@PathVariable Integer id) {
		Customer customer = customerService.findOne(id);
		return customer;
	}
}
```

- curl http://localhost:8080/api/customers -v GET 명령어로 전체 데이터 얻어와보기
- curl http://localhost:8080/api/customers/1 -v GET 로 1명의 데이터 얻어오기
- 데이터가 Json형식으로 반환 되었음을 알 수 있습니다.
- 아래는 나머지 API 구현

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseStatus;
import org.springframework.web.bind.annotation.RestController;

import com.example.app.domain.Customer;
import com.example.app.service.CustomerService;

// REST 웹 서비스의 엔드 포인트인 컨트롤러 클래스에는 이 어노테이션을 붙입니다.
@RestController
// 이 REST 웹 서비스에 접속하기 위한 경로를 @RequestMapping 어노테이션에 지정합니다.
@RequestMapping("api/customers")
public class CustomerRestController {
	@Autowired // 이미 작성된 CustomerService 클래스를 주입합니다.
	CustomerService customerService;
	
	// 이 메소드에 HTTP메소드 중 하나인 GET을 할당 합니다.
	// @RequestMapping 에서 지정한 경로에 접근 하면 getCustomers()가 실행 됩니다.
	@RequestMapping(method = RequestMethod.GET)
	List<Customer> getCustomers() {
		List<Customer> customers = customerService.findAll();
		// 위 Mapping 어노테이션 붙인 메소드의 반환 값은 직렬화 되어 HTTP 응답 내용 안에 설정 됩니다.
		// 기본으로 자바 객체는 JSON 형식으로 직렬화
		return customers;
	}
	
	// 아래 메소드 에도 GET할당 
	// 주소값 지정을 플레이스홀더로 id값을 정해 주었기 떄문에
	// api/customers/id 값 으로 접근이 가능함
	@RequestMapping(value = "{id}", method = RequestMethod.GET)
	Customer getCustomer(@PathVariable Integer id) {
		Customer customer = customerService.findOne(id);
		return customer;
	}
	
	// 신규 고객 정보 작성
	// 아래 메서드에 POST 할당
	@RequestMapping(method = RequestMethod.POST)
	// API가 정상동작 했을떄 응답 지정
	// CREATE동작은 201반환 그렇지 않으면 200 OK 반환
	@ResponseStatus(HttpStatus.CREATED)
	// HTTP요청을 Customer 객체에 매핑하기 위해 @RequestBody 어노테이현을 설정합니다.
	Customer postCustomers(@RequestBody Customer customer) {
		return customerService.create(customer);
	}
	
	// 고객 한명 정보 업데이트
	// 아래 어노테이션으로 Http 메소드 중 PUT 할당
	@RequestMapping(value = "{id}", method = RequestMethod.PUT)
	Customer putCustomer(@PathVariable Integer id, @RequestBody Customer customer) {
		customer.setId(id);
		return customerService.update(customer);
	}
	
	// 고객 한 명의 정보 삭제
	// DELETE Http 메소드 할당
	@RequestMapping(value = "{id}", method = RequestMethod.DELETE)
	@ResponseStatus(HttpStatus.NO_CONTENT) // 정상동작 시 204 NO_CONTENT 반환
	void deleteCustomer(@PathVariable Integer id) {
		customerService.delete(id);
	}
}
```

- 이제 어플리케이션을 동작 해볼때 입니다.
- cmd로 우선 실행 합니다.
- JSON을 전송하기 위해 -d 옵션으로 요청 내용 안에 JSON 형식의 고객 정보를 설정하고, -H 옵션으로 HTTP 헤더에 'Content-Type: application/json'을 설정합니다.	

> curl http://localhost:8080/api/customers -v POST -H "Content-Type: application/json" -d "{\"firstName\" : 999, \"lastName\" : \"Nobi\"}"

- 위명령어를 cmd로 실행 시키면 http상태중 하나인 201 Create 를 반환합니다.
- 다음으로 고객 한 명의 정보 업데이트 API를 실행합니다.
- 신규 고객 정보 작성 댸와 비슷한 과정으로 실행하며, HTTP 메서드를 PUT으로 바꾸고 URL에는 업데이트할 고객의 ID를 포함하면 니다.

> curl http://localhost:8080/api/customers/11 -v -X PUT -H "Content-Type: application/json" -d "{\"firstName\" : 333, \"lastName\" : \"Nobi\"}"

- GET, POST 이외엔 명령값 에 -v와 -X를 꼭 같이 붙혀야 한다.
- 일반적으로 REST웹 서비스 에서는 POST를 통해 새로 작성한 리소스에
- 접속하기 위한 URL를 HTTP 응답의 스프링 MVC로 Location헤더에 리소스 URI를 설정하려면 소스코드를 다음과 같이 수정합니다.

```java
// Controller 클래스 POST 부분
	@RequestMapping(method = RequestMethod.POST)
	ResponseEntity<Customer> postCustomers(@RequestBody Customer customer,
			// 컨텍스트 상대경로 URI를 쉽게 만들게 해주는 UriComponentsBuilder를
			// 컨트롤러 메소드의 인자로 지정
			UriComponentsBuilder uriBuilder) {
		
		Customer created = customerService.create(customer);
		
		// UriComponentsBuilder와 Customer 객체의 id로 리소스 URI를 만듭니다.
		// path() 메소드에 있는 {id}플레이스홀더며. buildAndExpand() 메소드에 넘겨준 값으로 치환 됩니다.
		URI location = uriBuilder.path("api/customers/{id}")
				.buildAndExpand(created.getId()).toUri();
		
		HttpHeaders headers = new HttpHeaders();
		// HttpHeaders객체로 HTTP응답 헤더를 만들고 location을 인자값으로 줌
		headers.setLocation(location);

		// HTTP응답 헤더를 설정하려면 메소드에서 Customer 객체가 아닌 밑의 자료형으로 반환 해야 합니다.
		//아래 처럼 각각 customer객체, 응답헤더인 headers객체, 상태 코드인 HttpStatus를 설정 합니다.
		return new ResponseEntity<Customer>(created, headers, HttpStatus.CREATED);
	}
```

- 지금까지 CURD 조작 기능을 REST API향태로 구현하는 방법을 살펴 봤습니다. 여기서 배운 내용을 응용하면 REST API를 구축할 수 있을 것입니다.

<hr>

## Chapter.02-3 페이징 처리 구현

- 모든 고객 정보 얻기 API 코드를 조금 변경하여 한 페이지당 레코드 수를 지정할 수 있게 만듭니다.
- 아래 메소드들을 각각 수정 합니다.

```java
// CustomerRestController 클래스
@RequestMapping(method = RequestMethod.GET)
Page<Customer> getCustomers(@PageableDefault Pageable pageable) {
	Page<Customer> customers = customerService.findAll(pageable);
	return customers;
}

// CustomerService 클래스
public Page<Customer> findAll(Pageable pageable) {
	return customerRepository.findAllOrderByName(pageable);
}

// CustomerRepository 클래스
@Query("SELECT x FROM Customer x ORDER BY x.firstName, x.lastName")
Page<Customer> findAllOrderByName(Pageable pageable);
```

- GET 명령을 보내 확인 합니다.

> 전체 명령<br>
> curl -X GET http://localhost:8080/api/customers<br>
> pageable값 넘겨주기<br>
> curl -X GET http://localhost:8080/api/customers?page=1&size=3"

<hr>


## Chapter.02-4 Thymeleaf를 사용해 화면에 표시하는 웹 어플리케이션 개발

- 지금 까지는 화면이 없고 JSON 형식으로 표시하는 어플리케이션을 개발 했습니다.
- 여기서는 GUI로 구성합니다.
- Thymeleaf는 HTML의 th:*** 속성(혹은 data-th-\*\*\* 속성)에 붙여 동적인 화면을 구성할 수 있는 템플릿 엔진 입니다.
- 이전 까지는 스프링 MVC로 웹 어플리케이션을 개발할 떄 JSP를 많이 사용 했습니다.
- 그러나 스프링 부트 에서는 Thymeleaf가 화면을 작성하기에 가장 좋습니다.
- 아래는 시스템 처리 목록 입니다.

| 처리 이름                   | HTTP 메소드 | 리소스 경로                  | 화면 이름                         |
|-----------------------------|-------------|------------------------------|-----------------------------------|
| 고객 정보 목록 표시 처리    | GET         | /customers                   | customers/list                    |
| 신규 고객 정보 작성 처리    | POST        | /customers/create            | 고객 정보 목록 표시 처리로 넘어감 |
| 고객 정보 편집 폼 표시 처리 | GET         | /customers/edit?form&id={id} | customers/edit                    |
| 고객 정보 편집 처리         | POST        | /customers/edit&id={id}      | 고객 정보 목록 표시 처리로 넘어감 |
| 고객 정보 삭제 처리         | POST        | /customers/delete?id={id}    | 고객 정보 목록 표시 처리로 넘어감 |

- 프로젝트 시작 혹은 메이븐에 아래 의존관계를 추가 합니다.

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

- 아래 처럼 프로젝트를 만듭니다.
- css, html, com.example.web 하위 파일 을 제외 하고 나머지는 이전 프로젝트에서 가져옵니다.

![customerService](/assets/sources/customerService.jpg "customerService")

<hr>

## Chapter.02-5 화면에 고객 정보 목록 표시하기

- 고객 관리 어플리케이션의 화면을 변경하는 기능을 CustomerController 클래스에 구현 합니다. 패키지 이름은 REST API를 구현할 떄와는 달리 com.example.web으로 지정 합니다.
- 다음 소스코드는 고객 정보 목록을 표시하는 소스 입니다.

```java
// CustomerController.java
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import com.example.domain.Customer;
import com.example.service.CustomerService;

// REST API와는 달리 화면 변경용 컨트롤러에는 이렇게 붙입니다.
@Controller
// URL이 /customers를 포함할 떄 list() 메소드에 매핑하도록 이렇게 붙입니다.
@RequestMapping("customers") 
public class CustomerController {
	@Autowired
	CustomerService customerService;
	
	// 스프링 MVC에서는 화면에 값을 넘겨주는 데 Model 객체를 사용 합니다.
	// 인자로 Model을 받아들이고 Model.addAttribute 메소드를 사용하여 화면에 넘겨줄 속성을 설정 합니다.
	String list(Model model) {
		List<Customer> customers = customerService.findAllList();
		// findAll의 결과를 model에 설정 합니다.
		// 속성이름은 customers로 지정합니다.
		// 사용자는 이 customers를 사용하여 접속할 수 있습니다.
		model.addAttribute("customers", customers);
		// 컨트롤러에서 @Controller가 붙은 요청 처리 메소드는 뷰 이름, 즉 변경될 화면 이름을 반환 합니다.
		// 스프링 부트에서는 기본값으로 classpath:templates/+ "뷰이름" + .html이 화면 경로가 됩니다.
		// 이 예제에서는 classpath:templates/customers/list.html을 표시합니다.
		return "customers/list";
	}
}
```

- 다음으로 고객 정보 목록을 표시하기 위해 list.html을 다음과 같이 작성 합니다.

```html
<!DOCTYPE html>
<!-- Thymeleaf의 th:*** 속성을 사용하기 위한 이름 공간을 지정합니다. -->
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="EUC-KR">
<title>고객 목록</title>
</head>
<body>
	<table class="table table-striped table-bordered table-condensed">
		<!-- th:each 속성을 사용해서 List<Customer>의 내용을 한 개씩 꺼냅니다 -->
		<!-- th:each="(루프 구간 안에서 사용하는 속성 이름) : ${(루프 대상 객체의 속성 이름)}"을 입력합니다. -->
		<tr th:each="customer : ${customers}">
			<!-- th:text 속성 값을 사용하여 HTML 태그 안에 포함된 문자를 치환할 수 있습니다. -->
			<!-- 서버 쪽에서 렌더링 할때 customer.id 값으로 치환 됩니다. -->
			<!-- 치환할 값은 기본적으로 HTML 이스케이프 크로스 사이트 스크립팅(XSS) 공격을 방지할 수 있습니다. -->
			<td th:text="${customer.id}"> 100 </td>
			<td th:text="${customer.lastName}"> 홍 </td>
			<td th:text="${customer.firstName}"> 길동 </td>
			<td>
				<!-- form 태그의 action 속성 내용을 th:action 속성 값으로 치환할 수 있습니다. -->
				<!-- @(***) 형식으로 지정하여 컨텍스트 경로의 상대 경로를 절대 경로로 치환할 수 있습니다. -->
				<form th:action="@{/customers/edit}" method="get">
					<input type = "submit" name = "form" value="편집" />
					<!-- input 태그의 value 속성 내용을 th:value 속성 값으로 치환할 수 있습니다. -->
					<input type = "hidden" name = "id" th:value="${customer.id}" />
				</form>
			</td>
			<td>
				<form th:action="@{/customers/delete}" method="post">
					<input type="submit" value="삭제" />
					<input type="hidden" name="id" th:value="${customer.id}" />
				</form>
			</td>
		</tr>
	</table>
</body>
</html>
```

- 브라우저로 이 HTML 파일을 직접 열어봅니다.
- 그전에 템플릿 캐시설정을 합니다.
- 성능저하 방지를 위해 데이터 값이 저장됩니다. 따라서 개발 중일떄는
- 어플을 껏다 켜야 하기떄문이 이 기능을 꺼줍니다.

<hr>

## Chapter.02-6 신규 고객 정보 작성하기

- 업데이트용 폼을 표현할 CustomerForm 클래스를 만듭니다.
- HTML의 <form> 에서 보낸 라미터를 이 클래스가 매핑하도록 합니다.
- 입력 규칙용 어노테이션을 붙입니다.
- Bena Validation은 자바에 표준으로 포함되어 있는 입력 검사 프레임워크 입니다.
- Bean Validation을 사용하면 자바 Bean에 어노테이션을 붙여 입력 검사 규칙을 지정할 수 있습니다.

```java
// CustomerController class
이전 소스 생략...

@RequestMapping(value = "create", method = RequestMethod.POST)
String create(@Validated CustomerForm form, BindingResult result, Model model) {
	if(result.hasErrors()) {
		return list(model);
	}
	
	Customer customer = new Customer();
	BeanUtils.copyProperties(form, customer);
	customerService.create(customer);
	return "redirect:/customers";
}

// CustomerForm class
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;

import lombok.Data;

@Data
public class CustomerForm {
	@NotNull
	@Size(min = 1, max = 127)
	private String firstName;
	
	@NotNull
	@Size(min = 1, max = 127)
	private String lastName;
}

// 한글꺠짐 방지를 위한 AppConfig class
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.Ordered;
import org.springframework.core.annotation.Order;
import org.springframework.web.filter.CharacterEncodingFilter;

@Configuration
public class AppConfig {
	@Order(Ordered.HIGHEST_PRECEDENCE)
	@Bean
	CharacterEncodingFilter characterEncodingFilter() {
		CharacterEncodingFilter filter = new CharacterEncodingFilter();
		filter.setEncoding("UTF-8");
		filter.setForceEncoding(true);
		return filter;
	}
}
```

> HTML 파일

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head lang="en">
<meta charset="UTF-8">
<title>고객 목록</title>

<link rel="stylesheet" type="text/css" href ="../../static/css/style.css" th:href="@{/css/style.css}">
</head>
<body>
	<div>
		<form th:action="@{/customers/create}" th:object="${customerForm}" method="post">
		<dl>
			<dt> <label for="lastName">성</label></dt>
			<dd>
				<input type="text" id="lastName" name="lastName" th:field="*{lastName}" th:errorclass="error-input" value="홍" />
				<span th:if="${#fields.hasErrors('lastName')}" th:errors="*{lastName}" class="error-messages">error!</span>
			</dd>
			
			<dt><label for="firstName"> 이름 </label></dt>
			<dd>
			    <input type="text" id="firstName" name="firstName" th:field="*{firstName}" th:errorclass="error-input" value="길동" />
				<span th:if="${#fields.hasErrors('firstName')}" th:errors="*{firstName}" class="error-messages">error!</span>
			</dd>
		</dl>
			<input type="submit" value="작성" />
		</form>
	</div>
	<hr/>
	<table>
		<tr th:each="customer : ${customers}">
			<td th:text="${customer.id}"> 100 </td>
			<td th:text="${customer.lastName}"> 홍 </td>
			<td th:text="${customer.firstName}"> 길동 </td>
			<td>
				<form th:action="@{/customers/edit}" method="get">
					<input type="submit" name="form" value="편집" />
					<input type="hidden" name="id" th:value="${customer.id}" />
				</form>
			</td>
			<td>
				<form th:action="@{/customers/delete}" method="post">
					<input type="submit" value="삭제" />
					<input type="hidden" name="id" th:value="${customer.id}" />
				</form>
			</td>
		</tr>
	</table>
</body>
</html>
```

- 만약 REST 웹 서비스에서 요청 내용을 JSON 형식으로 입력해서 검사할 경우
- 다음과 같이 업데이트 계열의 API에 포함된 메소드의 인자중 @RequestBody가 붙은 인자에 @Validated 어노테이션을 붙이기만 하면 됩니다. BindingResult는 필요 없습니다.

<hr>

## Chapter.02-7 고객 정보 편집하기

- 아래는 고객 정보를 편집하는 처리 입니다.
- 아래 소스에 두가지 메소드를 추가 해줍니다.
  
```java
// CustomerController 클래스
@RequestMapping(value = "edit", params = "form", method = RequestMethod.GET)
String editForm(@RequestParam Integer id, @Validated CustomerForm form) {
	Customer customer = customerService.findOne(id);
	// 고객 편집 폼이 현재 고객 정보를 표시할 수 있도록
	// CustomerService.update() 메소드로 업데이트 합니다.
	// 업데이트 처리가 끝나면 목록 표시 화면으로 리다이렉트 합니다.
	BeanUtils.copyProperties(customer, form);
	return "customers/edit";
}

@RequestMapping(value = "edit", method = RequestMethod.POST)
String edit(@RequestParam Integer id, @Validated CustomerForm form,
		BindingResult result) {
	
	if(result.hasErrors()) {
		return editForm(id, form);
	}
	
	Customer customer = new Customer();
	BeanUtils.copyProperties(form, customer);
	customer.setId(id);
	customerService.update(customer);
	return "redirect:/customer"; // 다시 화면 로딩
}

@RequestMapping(value = "edit", params = "goToTop")
String goToTop() {
	return "redirect:/customers";
}
```

> 아래는 편집 폼(HTML) 입니다.

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>고객 정보 편집</title>

<link rel="Stylesheet" type="text/css" href ="../../static/css/style.css" th:href="@{/css/style.css}" />
</head>
<body>
	<div>
		<form th:action="@{/customers/edit}" th:object="${customerForm}" method="post">
		<dl>
			<dt> <label for="lastName" class="col-sm-2 control-label">성</label></dt>
			<dd>
				<input type="text" id="lastName" name="lastName" th:field="*{lastName}" class="form-control" value="홍" />
				<span th:if="${#fields.hasErrors('lastName')}" th:errors="*{lastName}" class="help-block">error!</span>
			</dd>
			
			<dt><label for="firstName" class="col-sm-2 control-label"> 명 </label></dt>
			<dd>
			    <input type="text" id="firstName" name="firstName" th:field="*{firstName}" class="form-control" value="길동" />
				<span th:if="${#fields.hasErrors('firstName')}" th:errors="*{firstName}" class="help-block">error!</span>
			</dd>
		</dl>
			<!-- 한폼에 버튼을 여러 개 배치할 때는 name속성 값으로 컨트롤러 쪽 메소드를 구별합니다. -->
			<!-- 예를 들어, name="goToTop"이 설정된 버튼을 누르면 goToTop() 메소드가 호출됩니다. -->
			<input type="submit" class="btn btn-default" name="goToTop" value="돌아가기" />
			<!-- 고객 ID를 type="hidden"이 포함된 <input> 태그로 전송합니다. -->
			<!-- param.파라미터명 으로 요청 파라미터에 접근할 수 있습니다.(접근할 파라미터는 String[] 타입이라는 점에 주의 합니다.)-->
			<input type="hidden" name="id" th:value="${param.id[0]}"/>
			<input type="submit" class="btn btn-primary" value="업데이트" />
		</form>
	</div>
</body>
</html>
```

- 삭제도 똑같이 구현 합니다.

```java
@RequestMapping(value = "delete", method = RequestMethod.POST)
String edit(@RequestParam Integer id) {
	customerService.delete(id);
	return "redirect:/customers";
}
```

<hr>

## Chapter.02-8 스프링 시큐리티를 이용한 인증, 인가 처리 추가

- 보안을 강화 하는데 사용하는 프레임 워크 입니다.
- 아래 의존 관계를 추가 합니다.

```xml
<!-- 스프링 시큐리티 -->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-security</artifactId>
</dependency>

<!-- 프로퍼티스 : ??? -->
```

- 추가할 파일
  - com.example에 SecurityConfig.java
  - domain에 User.java
  - repository에 UserRepository.java
  - Service에 LoginUserDetails.java, LoginUserDetailsServoce.java
  - Web에 LoginController.java
  - resources폴더 밑에 db/migration 폴더까지 생성
    - 그후 V3__add-user.sql 생성
  - templates에 loginForm.html 생성

```java
// User 클래스 코딩
import java.util.List;

import javax.persistence.CascadeType;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.Id;
import javax.persistence.OneToMany;
import javax.persistence.Table;

import com.fasterxml.jackson.annotation.JsonIgnore;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.ToString;

@Data
@NoArgsConstructor
@AllArgsConstructor
@Entity
@Table(name = "users")
// 클래스 에는 그에 대응하는 User 클래스의 필드들을 추가 합니다.
@ToString(exclude = "customers")
public class User {
	@Id // userName 기본키 지정
	private String userName;
	// JPA와는 관계 없지만 API로 User클래스를 JSON 형식으로 출력할 경우,
	// 암호 필드를 제외하기 위해 이 어노테이션을 붙입니다.
	@JsonIgnore 
	private String encodedPassword;
	@JsonIgnore
	// User와 Customer를 1:N관계로 만들기 위해 이어노테이션을 붙입니다.
	// caseade는 User가 조작한 것들이 Customer에도 적용 됩니다.
	// fetch의 LAZY는 관련된 엔티티의 로드가 지연 됩니다.
	// User 엔티티를 가져올떄는 Customer 엔티티를 가져오지 않습니다.
	// customers 필드에 접속한 시점에 customer 엔티티를 가져옵니다.(SELECT문 실행)
	// 양방향 관련을 위해 mappedBy 속성에 연관 지을 속성의 이름을 지정합니다.
	@OneToMany(cascade = CascadeType.ALL, fetch = FetchType.LAZY, mappedBy = "user")
	private List<Customer> customers;
}
```

> Customer 클래스도 아래와 같이 고쳐 줍니다.

```java
// User와 Customer가 N:1 관계를 가지도록 @ManyToOne 어노테이션을 붙입니다.
@ManyToOne(fetch = FetchType.LAZY)
// JoinColumn 어노테이션을 사용하여 외부 키에 해당하는 칼럼 이름을 지정할 수 있습니다.
@JoinColumn(nullable = true, name = "username")
private User user;
```

> UserRepository class 소스 입니다.

```java

```