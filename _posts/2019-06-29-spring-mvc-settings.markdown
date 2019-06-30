---
layout: post
title:  (스프링 웹 프로젝트)CH5.스프링 MVC의 기본 구조
date:   2019-06-29 16:00:00
author: Kim Yongjun
categories: Java
tags: Java
---
# 스프링 MVC의 기본 구조

- 스프링 MVC는 스프링의 서브(sub) 프로젝트
    - '별도의 설정이 존재할 수 있다'라는 개념

## 스프링 MVC 프로젝트의 내부 구조
1. root-context.xml로 사용하는 일반 Java 영역(흔히 POJO(Plain Old Java Object))
2. servelt-context.xml로 설정하는 Web 관련 영역

- 스프링은 원래 목적 자체가 웹 애플리케이션을 목적으로 나온 프레임워크가 아니기 때문에
달라지는 영역에 대해서는 완전히 분리하고 연동하는 방식으로 구현

## 스프링 MVC의 기본 사상
- 개발자는 Servlet/JSP의 API에 신경쓰지 않고 웹 애플리케이션을 제작
- Spring MVC는 내부적으로 Servlet/JSP 처리
- 스프링 2.5버전 부터 등장한 어노테이션 방식으로 인해 최근 개발에는 어노테이션이나 XML 등의 설정만으로 개발이 가능하게 됨

## 모델2와 스프링 MVC
- 모델 2방식 : '로직과 화면을 분리'하는 스타일의 개발 방식
- 스프링 MVC의 기본 구조
    1. 사용자의 Request는 Front-Controller인 DispatcherServlet을 통해서 처리.
    web.xml을 보면 모든 Request를 DispatcherServelt이 받도록 처리.
    ``` xml
    <!-- Processes application requests -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
		
	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
    ```

    2. HandlerMapping은 Request의 처리를 담당하는 컨트롤러를 찾기 위해서 존재.
    RequestMappingHandlerMapping 객체는 @RequestMapping 어노테이션이 적용된 것을 기준으로 판단. HandlerAdapter를 이용해서 해당 컨트롤러를 동작.

    3. Controller는 실제 Request를 처리하는 로직을 작성. View에 전달해야 하는 데이터는 주로 Model이라는 객체에 담아서 전달.

    4. ViewResolver는 controller가 반환한 결과를 어떤 View를 통해서 처리하는 것이 좋을지 해석하는 역할. 가장 흔하게 사용하는 설정은 servlet-context.xml에 정의된 InternalResourceViewResolver임.

    ``` xml
    <!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
    ```

    5. View는 실제로 응답 보내야 하는 데이터를 Jsp등을 이용해서 생성하는 역할.
    만들어진 응답은 DispatcherServlet을 통해서 전송.
<h1 style="margin:0px;"> 참고자료 </h1>
<hr style="height:1px; margin:0px;">

- [코드로 배우는 스프링 웹 프로젝트(개정판)](http://www.yes24.com/Product/goods/64340061 "코드로 배우는 스프링 웹 프로젝트(개정판)")