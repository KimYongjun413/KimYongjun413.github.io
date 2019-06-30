---
layout: post
title:  (스프링 웹 프로젝트)Ch6.스프링 MVC의 Controller
date:   2019-06-30 16:26:00
author: Kim Yongjun
categories: Java
tags: Java
---
# 스프링 MVC의 Controller

- 특징
	1. HttpServeltRequest, HttpServeltResponse를 거의 사용할 필요 없이 필요한 기능 구현
	2. 다양한 타입의 파라미터 처리, 다양한 타입의 리턴 타입 사용 가능
	3. GET 방식, POST 방식 등 전송 방식에 대한 처리를 어노테이션으로 처리 가능
	4. 상속/인터페이스 방식 대신에 어노테이션만으로도 필요한 설정 가능

## @Controller, @RequestMapping
- 클래스 선언부에 @Controller : 자동으로 스프링의 객체(Bean)로 등록
	- servlet-context.xml에 그 이유가 있다.
	```xml 
	<context:component-scan base-package="org.zerock.controller" /> 
	```
- 클래스 선언부에 @RequestMapping : 현재 클래스의 모든 메서드들의 기본적인 URL 경로가 됨.
	- 메서드 선언에도 사용할 수 있다.

## @RequestMapping의 변화
- 몇 가지의 속성을 추가할 수 있다.
	- method 속성 : GET 방식, POST 방식을 구분해서 사용
- 스프링 4.3버전부터는 @GetMapping, @PostMapping 등장
- 일반적인 경우에는 GET,POST 방식만 사용하지만 최근에는 PUT, DELETE 방식 등도 점점 많이 사용.

## Controller의 파라미터 수집
- Lombok의 @Data 어노테이션을 이용
- @Data를 이용하게 되면 getter/setter, equals(), toString() 등의 메서드를 자동 생성
- Controller의 메서드가 클래스를 파라미터로 사용하게 되면 자동으로 setter 메서드가 동작하면서 파라미터를 수집.

### 파라미터의 수집과 변환
- public String ex02(@RequestParam("name") String name, @RequestParam("age") int age)
	- http://localhost:8080/sample/ex02?name=AAA&age=10
- 리스트, 배열 처리
	- public String ex02List(@RequestParam("ids")ArrayList<String> ids)
	- 프로젝트 경로/sample/ex02List?ids=111&ids=222&ids=333
- 객체 리스트
	- public String ex02Bean(SampleDTOList list)
	- 프로젝트 경로/sample/ex02Bean?list[0].name=aaa&list[2].name=bbb

### @InitBinder
- 파라미터 수집을 다른 용어로는 'binding(바인딩)'이라고 한다.
- 파라미터를 변환해서 처리해야 하는 경우에 사용할 수 있다.
```java
@InitBinder
public void initBinder(WebDataBinder binder) {
	SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
	binder.registerCustomEditor(Date.class, new CustomDateEditor(dateFormat,false));
}
```
dueData=2018-01-01 와 같이 호출했다면 서버에서 정상적으로 파라미터를 수집해서 처리
dueDate= Mon Jan 01 00:00:00 KST 2018

### @DateTimeFormat
- 파라미터로 사용되는 인스턴스 변수에 @DateTimeFormat을 적용해도 변환이 가능
```java
@DateTimeFormat(pattern = "yyyy/MM/dd")
private Date dueDate;
```

## Model이라는 데이터 전달자
- Controller의 메서드를 작성할 때는 특별하게 Model이라는 타입을파라미터로 지정할 수 있다.
- 뷰(View)로 전달해야 하는 데이터를 담아서 보낼 수 있다.
- Model을 사용해야 하는 경우는 주로 Controller에 전달된 데이터를 이용해서 추가적인 데이터를 가져와야 하는 상황

### @ModelAttribute 어노테이션
- 스프링 MVC의 Controller는 기본적으로 Java Beans 규칙에 맞는 객체는 다시 화면으로 객체를 전달.
- 좁은 의미에서 Java Beans의 규칙
	- 생성자가 없거나 빈 생성자를 가져야 한다.
	- getter/setter를 가진 클래스여야 한다.
- 기본 자료형은 기본적으로 화면까지 전달되지는 않는다.
	- @ModelAttribute는 강제로 전달받은 파라미터를 Model에 담아서 전달
	- public String ex04(SampleDTO dto, @ModelAttribute("page") int page)

### RedirectAttributes
- 일회성으로 데이터를 전달하는 용도로 사용
```java
rttr.addFlashAttribute("name","AAA");
rttr.addFlashAttribute("age",10);
return "redirect:/";
```

## Controller의 리턴 타입
스프링 MVC의 구조가 기존의 상속과 인터페이스에서 어노테이션을 사용하는 방식으로 변한 이후에 가장 큰 변화 중 하나는 리턴 타입이 자유로워 졌다는 점이다.

Controller의 메서드가 사용할 수 있는 리턴 타입은 주로 다음과 같다.
- String: jsp를 이용하는 경우에는 jsp 파일의 경로와 파일이름을 나타내기 위해 사용
- void: 호출하는 URL과 동일한 이름의 jsp를 의미

### 객체 타입
Controller의 메서드 리턴 타입을 VO(Value Object)나 DTO(Data Transfer Object)타입 등 복합적인 데이터가 들어간 객체 타입으로 지정할 수 있다. 이 경우는 주로 JSON 데이터를 만들어 내는 용도로 사용한다.

우선 이를 위해서는 jackson-databind 라이브러리를 pom.xml에 추가한다.

메서드 생성
```java
@GetMapping("/ex06")
public @ResponseBody SampleDTO ex06() {
	SampleDTO dto = new SampleDTO();
	dto.setAge(10);
	dto.setName("홍길동");
	return dto;
}
```
결과
```html
{"name":"홍길동","age":10}
```

### ResponseEntity 타입
HTTP 프로토콜의 헤더를 다루는 경우
```java
@GetMapping("/ex07")
public ResponseEntity<String> ex07() {
	log.info("/ex07..........");
	// {"name": "홍길동"}
	String msg = "{\"name\": \"홍길동\"}";
	HttpHeaders header = new HttpHeaders();
	header.add("Content-Type", "application/json;charset=UTF-8");
	return new ResponseEntity<>(msg, header, HttpStatus.OK);
}
```

## Controller의 Exception 처리

### @ControllerAdvice
- AOP(Aspect-Oriented-Programming)를 이용하는 방식
- 공통적인 예외사항에 대해서 별도로 분리하는 방식

org.zerock.exception 패키지 생성, CommonExceptionAdvice 클래스 생성
```java
package org.zerock.exception;

import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

import lombok.extern.log4j.Log4j;

@ControllerAdvice
@Log4j
public class CommonExceptionAdvice {

	@ExceptionHandler(Exception.class)
	public String except(Exception ex, Model model) {

		log.error("Exception ......." + ex.getMessage());
		model.addAttribute("exception", ex);
		log.error(model);
		return "error_page";
	}
}
```
- @ControllerAdvice는 해당 객체가 스프링의 컨트롤러에서 발생하는 예외를 처리하는 존재임을 명시하는 용도로 사용
- @ExceptionHandler는 해당 메서드가 () 들어가는 예외 타팁을 처리한다는 거을 의미
- JSP 화면에서도 구체적인 메시지를 보고 싶다면 Model을 이용해서 전달하는 것이 좋다.
- servelt-context.xml에 인식 하도록 <component-scam> 을 이용해 패키지의 내용을 조사하도록 한다.

### 404 에러 페이지
잘못된 URL을 호출할 때 보이는 404 에러 메시지의 경우는 조금 다르게 처리하는 것이 좋다.

스프링 MVC의 모든 요청은 DispatcherServlet을 이용해서 처리되므로 404 에러도 같이 처리할 수 있도록 web.xml을 수정한다.
```xml
<!-- Processes application requests -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
		<!-- 추가된 부분 -->
		<init-param>
			<param-name>throwExceptionIfNoHandlerFound</param-name>
			<param-value>true</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
		
	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
```

CommonExceptionAdvice 을 다음과 같이 메서드 추가함.
```java
@ControllerAdvice
@Log4j
public class CommonExceptionAdvice {
	...

	@ExceptionHandler(NoHandlerFoundException.class)
	@ResponseStatus(HttpStatus.NOT_FOUND)
	public String handle404(NoHandlerFoundException ex) {

		return "custom404";
	}

}
```

<h1 style="margin:0px;"> 참고자료 </h1>
<hr style="height:1px; margin:0px;">

- [코드로 배우는 스프링 웹 프로젝트(개정판)](http://www.yes24.com/Product/goods/64340061 "코드로 배우는 스프링 웹 프로젝트(개정판)")